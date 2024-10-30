### পোস্টম্যানে সিসকো স্যান্ডবক্স টেস্ট

নেটওয়ার্ক অটোমেশন শুরু করার আগে আমাদের বুঝতে হবে কিভাবে REST API কাজ করে। REST API হল একটা পদ্ধতি যেখানে আমরা HTTP রিকোয়েস্ট (GET, POST, PUT, DELETE) পাঠিয়ে নেটওয়ার্ক ডিভাইস কন্ট্রোল করি। পোস্টম্যান হল একটা টুল যা দিয়ে আমরা এই API টেস্ট করতে পারি।

## বেসিক সেটআপ
শুরুতেই একটা নতুন কালেকশন বানাতে হবে। কালেকশন হল অনেকগুলো রিকোয়েস্টের গ্রুপ। আমরা "Network-Automation" নামে একটা কালেকশন বানাব।

## অথেনটিকেশন সেটআপ
প্রতিটা রিকোয়েস্টের জন্য আমাদের Basic Auth ব্যবহার করতে হবে। এটা অনেকটা রাউটারে লগইন করার মত। ইউজারনেম হবে 'admin' এবং পাসওয়ার্ড হবে 'C1sco12345'। SSL সার্টিফিকেট ভেরিফিকেশন অফ করে রাখতে হবে।

## প্রথমে GET দিয়ে চেক করা
কোন কিছু চেঞ্জ করার আগে আমরা প্রথমে GET রিকোয়েস্ট দিয়ে চেক করে নিব বর্তমান কনফিগারেশন। এটা করার জন্য:

1. নতুন রিকোয়েস্ট বানান "Check-Current-Config" নামে
2. মেথড GET সিলেক্ট করুন
3. হেডার সেট করুন:
```
Accept: application/yang-data+json
Content-Type: application/yang-data+json
```

### হোস্টনেম চেক
প্রথমে হোস্টনেম চেক করি:
```
GET https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/hostname
```

### ইন্টারফেস চেক
ইন্টারফেস দেখার জন্য:
```
GET https://devnetsandboxiosxe.cisco.com:443/restconf/data/ietf-interfaces:interfaces
```

### রাউট চেক
স্ট্যাটিক রাউট দেখার জন্য:
```
GET https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/ip/route
```

### এরপর কনফিগারেশন চেঞ্জ করি 
GET দিয়ে বর্তমান কনফিগ দেখে নেওয়ার পর আমরা PUT রিকোয়েস্ট দিয়ে চেঞ্জ করব। এখানে একটা গুরুত্বপূর্ণ বিষয় হল JSON ফরম্যাট। JSON এ কোন ভুল থাকলে রিকোয়েস্ট ফেইল হবে।

### হোস্টনেম চেঞ্জ
হোস্টনেম চেঞ্জ করার জন্য:
```
PUT https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/hostname

বডি:
{
    "Cisco-IOS-XE-native:hostname": "Link3_Test"
}
```

### লুপব্যাক ইন্টারফেস যোগ
লুপব্যাক ইন্টারফেস যোগ করার জন্য:
```
PUT https://devnetsandboxiosxe.cisco.com:443/restconf/data/ietf-interfaces:interfaces

বডি:
{
  "ietf-interfaces:interface": {
    "name": "Loopback152",
    "description": "Link3 loopback interface",
    "type": "iana-if-type:softwareLoopback",
    "enabled": true,
    "ietf-ip:ipv4": {
      "address": [
        {
          "ip": "10.1.11.8",
          "netmask": "255.255.255.0"
        }
      ]
    }
  }
}
```

### রাউটিং কনফিগারেশন

রাউটিং কনফিগারেশন করার আগে আমাদের বুঝতে হবে কি কি রাউট আছে এবং কি কি নতুন রাউট যোগ করব। RESTCONF এর মাধ্যমে আমরা স্ট্যাটিক রাউট যোগ করতে পারি। প্রথমেই দেখে নিব বর্তমানে কি কি রাউট আছে।

## বর্তমান রাউটিং টেবিল চেক
প্রথমে GET রিকোয়েস্ট দিয়ে দেখে নিব কি কি রাউট আছে:

```
GET https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/ip/route
```

### স্ট্যাটিক রাউট যোগ করা
এখন আমরা একটা ডিফল্ট রাউট যোগ করব। এটা হবে 0.0.0.0/0 যা সব ট্র্যাফিক নেক্সট-হপ আইপি এর দিকে পাঠাবে:

```
PUT https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/ip/route

বডি:
{
    "Cisco-IOS-XE-native:route": {
        "ip-route-interface-forwarding-list": [
            {
                "prefix": "0.0.0.0",
                "mask": "0.0.0.0",
                "fwd-list": [
                    {
                        "fwd": "GigabitEthernet1",
                        "interface-next-hop": [
                            {
                                "ip-address": "10.10.40.254"
                            }
                        ]
                    }
                ]
            }
        ]
    }
}
```

## ভেরিফিকেশন
রাউট যোগ করার পর আবার GET রিকোয়েস্ট দিয়ে চেক করে নিব:

```
GET https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/ip/route
```

## গুরুত্বপূর্ণ টিপস
1. রাউট যোগ করার আগে নেটওয়ার্ক টপোলজি ভালো করে বুঝে নিন
2. নেক্সট-হপ আইপি অবশ্যই রিচেবল হতে হবে
3. ডিফল্ট রাউট যোগ করার সময় খুব সাবধান থাকতে হবে
4. প্রতিটা চেঞ্জের পর GET দিয়ে ভেরিফাই করুন

## সম্ভাব্য এরর
- যদি নেক্সট-হপ আইপি রিচেবল না হয়
- যদি ইন্টারফেস নাম ভুল হয়
- যদি সাবনেট মাস্ক ভুল হয়

এই এরর গুলো ঠিক করার জন্য:
1. প্রথমে GET দিয়ে চেক করুন
2. কনফিগারেশন ডাবল চেক করুন
3. নেক্সট-হপ ping করে দেখুন
4. ইন্টারফেস স্ট্যাটাস চেক করুন

কি পারা গেছে?