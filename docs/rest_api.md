### পোস্টম্যানে সিসকো স্যান্ডবক্স টেস্ট

চলুন একটা সিম্পল টেস্ট করি। আমরা সিস্কোর একটা ডেমো রাউটারের হোস্টনেম চেঞ্জ করব:

১. নতুন রিকোয়েস্ট তৈরি:
```
Collection নামঃ Network-Automation
Request নামঃ Change-Hostname
```

২. রিকোয়েস্ট ডিটেইলস:
```
Method: POST
URL: https://devnetsandboxiosxe.cisco.com:443/restconf/data/
```

৩. Authentication:
    - টাইপ: Basic Auth
    - ইউজারনেম: admin
    - পাসওয়ার্ড: C1sco12345

৪. Headers:
```
Accept: application/yang-data+json
Content-Type: application/yang-data+json
```

৫. Body:
```xml
<config>
    <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
        <hostname>Link3_NewRouter</hostname>
    </native>
</config>
```

৬. Send বাটন চাপুন

### রেজাল্ট বোঝা

যদি সব ঠিক থাকে:
- Status: 200 OK
- রাউটারের হোস্টনেম চেঞ্জ হয়ে যাবে

যদি এরর আসে:
- 401: ক্রেডেনশিয়াল ভুল
- 404: URL ভুল
- 500: রাউটারে কোন সমস্যা

## পর্ব ২: বাস্তব উদাহরণ

চলুন কয়েকটা বাস্তব কাজ করি:

### ১. ইন্টারফেস স্ট্যাটাস চেক

```
Method: GET
URL: https://devnetsandboxiosxe.cisco.com:443/restconf/data/ietf-interfaces:interfaces-state
```

বডি লাগবে না, শুধু Send চাপুন। সব ইন্টারফেসের স্ট্যাটাস দেখাবে।

### ২. VLAN তৈরি করা

```
Method: POST
URL: https://devnetsandboxiosxe.cisco.com:443/restconf/data/native/vlan

Body:
<config>
    <vlan>
        <id>100</id>
        <name>IT_Department</name>
    </vlan>
</config>
```

### ৩. এক্সেস লিস্ট যোগ করা

```
Method: POST
URL: https://devnetsandboxiosxe.cisco.com:443/restconf/data/native/ip/access-list

Body:
<config>
    <access-list>
        <id>100</id>
        <rule>
            <action>permit</action>
            <source>192.168.1.0/24</source>
        </rule>
    </access-list>
</config>
```

!!! tip "পোস্টম্যান টিপস"
    - প্রতিটা রিকোয়েস্ট সেভ করে রাখুন
    - Environment সেটআপ করে ভেরিয়েবল ব্যবহার করুন
    - টেস্ট রান Auto করতে পারেন
    - রেজাল্ট এক্সপোর্ট করে রাখুন