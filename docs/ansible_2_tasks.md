অ্যান্সিবল ব্যবহার করে কীভাবে সুইচ এবং রাউটারগুলিতে ভিন্ন ভিন্ন কাজ করা যায় তা একটি সাধারণ উদাহরণ দিয়ে ব্যাখ্যা করছি। ধরে নিই, আপনি একটি অফিসে কাজ করেন যেখানে আপনার অনেক কম্পিউটার এবং নেটওয়ার্ক ডিভাইস (যেমন সুইচ এবং রাউটার) আছে এবং আপনাকে এগুলোর জন্য নিয়মিত কনফিগারেশন পরিবর্তন করতে হয়। অ্যান্সিবল ব্যবহার করে এই কাজটি খুব সহজে এবং স্বয়ংক্রিয়ভাবে করা যায়।

### স্টেপ ১: ইনভেন্টরি ফাইল তৈরি

প্রথমে আমাদের একটি ইনভেন্টরি ফাইল তৈরি করতে হবে যেখানে আমরা আমাদের ডিভাইসগুলির তালিকা রাখব। ধরুন, আপনার দুটি রাউটার এবং দুটি সুইচ আছে। ইনভেন্টরি ফাইলটি এমন দেখতে হবে:

```ini
[routers]
router1 ansible_host=192.168.1.1
router2 ansible_host=192.168.1.2

[switches]
switch1 ansible_host=192.168.2.1
switch2 ansible_host=192.168.2.2
```

এখানে `routers` গ্রুপে দুটি রাউটার রয়েছে এবং `switches` গ্রুপে দুটি সুইচ রয়েছে। প্রতিটি ডিভাইসের আইপি এড্রেস (যেটা তাদের নেটওয়ার্কের মাধ্যমে অ্যাক্সেস করা যায়) উল্লেখ করা হয়েছে।

### স্টেপ ২: প্লেবুক তৈরি

এরপর আমরা একটি প্লেবুক তৈরি করব। প্লেবুক হলো অ্যান্সিবলের জন্য একটি নির্দেশিকা যেখানে আমরা বলি কোন ডিভাইসে কী কাজ করতে হবে। এই প্লেবুকটি দেখুন:

```yaml
---
- name: রাউটার কনফিগার করা
  hosts: routers
  gather_facts: no
  tasks:
    - name: নিশ্চিত করা যে রাউটারের কনফিগারেশন ফাইল আছে
      copy:
        src: configs/router_config.cfg
        dest: /etc/router_config.cfg

    - name: রাউটার কনফিগারেশন প্রয়োগ করা
      command: /usr/bin/router_apply_config /etc/router_config.cfg

- name: সুইচ কনফিগার করা
  hosts: switches
  gather_facts: no
  tasks:
    - name: নিশ্চিত করা যে সুইচের কনফিগারেশন ফাইল আছে
      copy:
        src: configs/switch_config.cfg
        dest: /etc/switch_config.cfg

    - name: সুইচ কনফিগারেশন প্রয়োগ করা
      command: /usr/bin/switch_apply_config /etc/switch_config.cfg
```

উদাহরণ অনুযায়ী নিচে কিছু কনফিগারেশন ফাইল যুক্ত করা হলো। এখানে আমরা রাউটার এবং সুইচের বিভিন্ন সেটিংস কনফিগার করব।

### স্টেপ ৩: কনফিগারেশন ফাইল তৈরি

### উদাহরণ ১: Router Configuration (`router_config.cfg`)

    ধরি, আপনি একটি রাউটারে নিচের কনফিগারেশন করতে চান:
    - ইন্টারফেস IP এড্রেস সেট করা
    - স্ট্যাটিক রুট যোগ করা
    - ACL (Access Control List) তৈরি করা
    - NAT (Network Address Translation) কনফিগার করা

`router_config.cfg` এর উদাহরণ হতে পারে:

```
# Router Configuration

# Set interface IP address
interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/2
 ip address 10.0.0.1 255.255.255.0
 no shutdown

# Add static route
ip route 10.10.10.0 255.255.255.0 192.168.1.254

# Configure Access Control List (ACL)
access-list 100 permit ip any any
access-list 100 deny ip 192.168.1.0 0.0.0.255 any

# Configure NAT
ip nat inside source list 100 interface GigabitEthernet0/2 overload
interface GigabitEthernet0/1
 ip nat inside
interface GigabitEthernet0/2
 ip nat outside
```

### উদাহরণ ২: Switch Configuration (`switch_config.cfg`)

    একইভাবে, ধরি আপনি একটি সুইচে নিচের কনফিগারেশন করতে চান:
    - VLAN তৈরি করা এবং নাম দেয়া
    - ইন্টারফেসগুলোতে VLAN এসাইন করা
    - ট্রাঙ্ক পোর্ট কনফিগার করা
    - স্প্যানিং ট্রি প্রোটোকল (STP) সক্রিয় করা

`switch_config.cfg` এর উদাহরণ হতে পারে:

```
# Switch Configuration

# Create VLANs
vlan 10
 name Sales

vlan 20
 name Engineering

vlan 30
 name Management

# Configure Access Interfaces
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
 no shutdown

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
 no shutdown

interface FastEthernet0/3
 switchport mode access
 switchport access vlan 30
 no shutdown

# Configure Trunk Interface
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 no shutdown

# Enable Spanning Tree Protocol (STP)
spanning-tree mode rapid-pvst
spanning-tree vlan 10 priority 4096
spanning-tree vlan 20 priority 8192
spanning-tree vlan 30 priority 12288
```

### স্টেপ ৩: প্লেবুক চালানো

এই প্লেবুক চালানোর জন্য আপনাকে শুধু একটি কমান্ড চালাতে হবে:

```sh
ansible-playbook -i hosts site.yml
```

### কি করা হয়েছে?

এখন আমরা আলাপ করি কী করা হয়েছে:

1. **ইনভেন্টরি ফাইল**: এটি হলো একটি তালিকা যেখানে আপনার সব ডিভাইসের নাম এবং তাদের আইপি এড্রেস আছে। আপনি একে একটি কন্টাক্ট লিস্টের মতো ভাবতে পারেন।
2. **প্লেবুক**: এটি হলো একটি নির্দেশিকা যেখানে আপনি বলছেন কোন ডিভাইসে কী কাজ করতে হবে। 
   - প্রথম অংশে বলা হচ্ছে "রাউটার গ্রুপের জন্য এই কাজগুলো করো"।
   - দ্বিতীয় অংশে বলা হচ্ছে "সুইচ গ্রুপের জন্য এই কাজগুলো করো"।
3. **টাস্ক**: প্রতিটি ডিভাইসে কী কাজ হবে তা টাস্ক হিসেবে লেখা আছে। উদাহরণস্বরূপ:
   - রাউটারের জন্য একটি নির্দিষ্ট ফাইল কপি করা এবং সেটি প্রয়োগ করা।
   - সুইচের জন্যও একই কাজ।

### সুবিধা

- **স্বয়ংক্রিয়তা**: আপনি একবার নির্দেশিকা তৈরি করলে, এটি সব ডিভাইসে একইভাবে কাজ করবে।
- **দ্রুততা**: একসাথে অনেক ডিভাইসে পরিবর্তন করতে পারবেন, এক এক করে করার দরকার নেই।
- **ভুল কমানো**: একই কাজ বারবার করতে গেলে ভুল হওয়ার সম্ভাবনা থাকে, অ্যান্সিবল তা কমিয়ে আনে।
