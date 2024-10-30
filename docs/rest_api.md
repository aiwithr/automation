### পোস্টম্যানে সিসকো স্যান্ডবক্স টেস্ট

চলুন একটা সিম্পল টেস্ট করি। আমরা সিস্কোর একটা ডেমো রাউটারের হোস্টনেম চেঞ্জ করব:

প্রথমে পোস্টম্যান ওপেন করে নতুন একটা রিকোয়েস্ট বানাতে হবে। 

### নতুন রিকোয়েস্ট তৈরি:
```
Collection নামঃ Network-Automation
Request নামঃ Change-Hostname
```

এখানে মেথড হিসেবে PUT সিলেক্ট করব, কারণ আমরা কিছু চেঞ্জ করতে চাইছি। তারপর URL দিতে হবে - এটা হবে আমাদের রাউটারের ঠিকানা। URL-এ আমরা লিখব: https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/hostname

এরপর আমাদের অথেনটিকেশন সেট করতে হবে। এটা অনেকটা রাউটারে লগইন করার মতো। Basic Auth সিলেক্ট করে ইউজারনেম 'admin' এবং পাসওয়ার্ড 'C1sco12345' দিতে হবে।

এবার আসি হেডার সেটআপে। হেডার হল এক্সট্রা ইনফরমেশন যা রাউটারকে বলে আমরা কী ধরনের ডাটা পাঠাচ্ছি এবং কী ধরনের উত্তর চাই। এখানে Content-Type এবং Accept দুটোই application/yang-data+json হবে।

সবশেষে বডিতে JSON ফরম্যাটে আমাদের কমান্ড লিখতে হবে। এটা অনেকটা CLI কমান্ডের মতো, তবে JSON ফরম্যাটে:
```json
{
    "hostname": "Link3_Test"
}
```

এবার Send বাটনে ক্লিক করলেই আমাদের রিকোয়েস্ট রাউটারে চলে যাবে। যদি সব ঠিক থাকে তাহলে 200 OK রেসপন্স আসবে, মানে কাজ সফল হয়েছে।

!!! tip "মনে রাখুন"
    - SSL সার্টিফিকেট ভেরিফিকেশন বন্ধ রাখতে হবে
    - JSON ফরম্যাট ঠিক রাখতে হবে
    - হেডার ঠিকমতো দিতে হবে
  
### রেজাল্ট বোঝা

যদি সব ঠিক থাকে:
- Status: 200 OK
- রাউটারের হোস্টনেম চেঞ্জ হয়ে যাবে

যদি এরর আসে:
- 401: ক্রেডেনশিয়াল ভুল
- 404: URL ভুল
- 500: রাউটারে কোন সমস্যা

### সিসকো স্যান্ডবক্সে VLAN এবং রাউটিং কনফিগারেশন

আজকে আমরা সিসকো স্যান্ডবক্স দিয়ে VLAN এবং রাউটিং কনফিগারেশন শিখব। একই ক্রেডেনশিয়াল ব্যবহার করব:

### Interface অ্যাড করছি 

PUT মেথড: https://devnetsandboxiosxe.cisco.com:443/restconf/data/ietf-interfaces:interfaces

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


!!! tip "মনে রাখবেন"
    - প্রতিটা রিকোয়েস্টে Basic Auth ব্যবহার করতে হবে
    - SSL verification off রাখতে হবে
    - প্রথমে GET দিয়ে চেক করুন
    - তারপর PUT/POST দিয়ে চেঞ্জ করুন

## ৪. রাউট চেক করা

স্ট্যাটিক রাউট দেখা:
```
মেথড: GET
URL: https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/ip/route

হেডার:
Accept: application/yang-data+json
Content-Type: application/yang-data+json
```

স্ট্যাটিক রাউট যোগ করা:
```
মেথড: PUT
URL: https://devnetsandboxiosxe.cisco.com:443/restconf/data/Cisco-IOS-XE-native:native/ip/route

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
কি পারা গেছে?