অ্যান্সিবল ব্যবহার করে কীভাবে সুইচ, মাইক্রোটিক এবং রাউটারগুলোতে ভিন্ন ভিন্ন কাজ করা যায় তা একটি সাধারণ উদাহরণ দিয়ে ব্যাখ্যা করছি। ধরে নিই, আপনি একটি অফিসে কাজ করেন যেখানে আপনার অনেক কম্পিউটার এবং নেটওয়ার্ক ডিভাইস (যেমন সুইচ, মাইক্রোটিক এবং রাউটার) আছে এবং আপনাকে এগুলোর জন্য নিয়মিত কনফিগারেশন চেঞ্জ করতে হয়। অ্যান্সিবল ব্যবহার করে এই কাজটা খুব সহজে এবং অটোমেটিক্যালি করা যায়।

### স্টেপ ১: প্রয়োজনীয় জিনিসপত্র

প্রথমে আমাদের যা যা লাগবে:

- একটা লিনাক্স/উইন্ডোজ কম্পিউটার
- অ্যানসিবল ইনস্টল করা
- নেটওয়ার্ক ডিভাইসগুলোর আইপি এড্রেস

অ্যানসিবল ইনস্টল করার কমান্ড:
```bash
# উবুন্টুতে
sudo apt-get update
sudo apt-get install ansible

# লিনাক্স/উইন্ডোজে pip দিয়ে
pip install ansible
```

### স্টেপ ২: ইনভেন্টরি ফাইল বানানো

এবার আমাদের ডিভাইসগুলোর একটা তালিকা বানাব। এটা করব `hosts` নামের একটা ফাইলে:

আপনি ঠিক বলেছেন। আমি মূল লেখা অনুসারে ইনভেন্টরি ফাইলটি আবার লিখছি:

```ini
[routers]
router1 ansible_host=192.168.1.1
router2 ansible_host=192.168.1.2

[switches]
switch1 ansible_host=192.168.2.1
switch2 ansible_host=192.168.2.2

[mikrotiks]
mikrotik1 ansible_host=192.168.3.1
mikrotik2 ansible_host=192.168.4.1

[all:vars]
ansible_connection=network_cli
ansible_user=admin
ansible_password=cisco123
```

এখানে তিন ধরনের ডিভাইস আছে:

- দুটো রাউটার (192.168.1.x নেটওয়ার্কে)
- দুটো সুইচ (192.168.2.x নেটওয়ার্কে)
- দুটো মাইক্রোটিক (192.168.3.x এবং 192.168.4.x নেটওয়ার্কে)

চলুন প্লেবুক লিখি। এটি `site.yml` ফাইলে লিখব:

```yaml
---
- name: রাউটার কনফিগার করা
  hosts: routers
  gather_facts: no
  tasks:
    - name: রাউটারে কমান্ড পাঠানো
      ios_config:
        lines:
          - interface GigabitEthernet0/1
          - ip address 192.168.1.1 255.255.255.0
          - no shutdown
          - ip route 10.10.10.0 255.255.255.0 192.168.1.254

- name: সুইচ কনফিগার করা
  hosts: switches
  gather_facts: no
  tasks:
    - name: VLAN তৈরি করা
      ios_config:
        lines:
          - vlan 10
          - name Sales
          - vlan 20
          - name Engineering
          - interface FastEthernet0/1
          - switchport mode access
          - switchport access vlan 10

- name: মাইক্রোটিক কনফিগার করা
  hosts: mikrotiks
  gather_facts: no
  tasks:
    - name: আইপি এড্রেস এবং ডিএনএস সেট করা
      routeros_command:
        commands:
          - /ip address add address=192.168.100.1/24 interface=VLAN_10
          - /ip dns set servers=8.8.8.8,1.1.1.1
```

এই প্লেবুকে তিনটি প্রধান অংশ আছে:

১. **রাউটার কনফিগারেশন**:

   - ইন্টারফেস চালু করা
   - আইপি এড্রেস দেওয়া
   - রাউট সেট করা

২. **সুইচ কনফিগারেশন**:

   - VLAN তৈরি করা
   - পোর্টে VLAN এসাইন করা

৩. **মাইক্রোটিক কনফিগারেশন**:

   - আইপি এড্রেস সেট করা
   - ডিএনএস সার্ভার সেট করা

চলুন প্রতিটা ডিভাইসের জন্য আলাদা কনফিগ ফাইল বানাই।

### ১. রাউটার কনফিগারেশন
`configs/router_config.cfg` ফাইলে লিখব:

```
# ইন্টারফেস সেটআপ
interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/2
 ip address 10.0.0.1 255.255.255.0
 no shutdown

# ডিফল্ট রাউট সেটআপ
ip route 0.0.0.0 0.0.0.0 192.168.1.254

# বেসিক সিকিউরিটি
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
access-list 100 deny ip any any log

# NAT সেটআপ
ip nat inside source list 100 interface GigabitEthernet0/2 overload
interface GigabitEthernet0/1
 ip nat inside
interface GigabitEthernet0/2
 ip nat outside
```

### ২. সুইচ কনফিগারেশন
`configs/switch_config.cfg` ফাইলে লিখব:

```
# VLAN তৈরি
vlan 10
 name Sales
vlan 20
 name IT
vlan 30
 name Management

# এক্সেস পোর্ট সেটআপ
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
 no shutdown

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
 no shutdown

# ট্রাঙ্ক পোর্ট সেটআপ
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 no shutdown

# স্প্যানিং ট্রি
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30 priority 4096
```

### ৩. মাইক্রোটিক কনফিগারেশন
`configs/mikrotik_config.cfg` ফাইলে লিখব:

```
# ইন্টারফেস এবং VLAN সেটআপ
/interface vlan add name=VLAN_10 vlan-id=10 interface=ether1
/interface vlan add name=VLAN_20 vlan-id=20 interface=ether1

# আইপি কনফিগারেশন
/ip address add address=192.168.10.1/24 interface=VLAN_10
/ip address add address=192.168.20.1/24 interface=VLAN_20

# ডিএনএস সেটআপ
/ip dns set servers=8.8.8.8,1.1.1.1

# ডিএইচসিপি সার্ভার
/ip pool add name=dhcp_pool1 ranges=192.168.10.2-192.168.10.254
/ip dhcp-server add address-pool=dhcp_pool1 interface=VLAN_10 name=dhcp1
/ip dhcp-server network add address=192.168.10.0/24 gateway=192.168.10.1
```

এই কনফিগারেশন ফাইলগুলো খুবই বেসিক। আপনার প্রয়োজন অনুযায়ী এগুলো পরিবর্তন করতে পারেন।

চলুন দেখি কীভাবে প্লেবুক চালাতে হয়। প্রথমে আমাদের ফাইল স্ট্রাকচারটা দেখে নেই:

```
network_automation/
├── hosts                           # ইনভেন্টরি ফাইল
├── site.yml                        # মূল প্লেবুক
└── configs/                        # কনফিগ ফাইলগুলো
    ├── router_config.cfg
    ├── switch_config.cfg
    └── mikrotik_config.cfg
```

### প্লেবুক চালানোর আগে চেক করার বিষয়গুলো:

1. সব ডিভাইসে SSH এনাবল আছে কিনা
2. ইউজারনেম-পাসওয়ার্ড সঠিক আছে কিনা
3. নেটওয়ার্ক কানেকশন আছে কিনা

### প্লেবুক চালানো:

প্রথমে শুধু চেক করে দেখি সব ঠিক আছে কিনা:
```bash
# ড্রাই-রান মোডে চালানো (কোনো পরিবর্তন হবে না)
ansible-playbook -i hosts site.yml --check
```

এরপর আসল প্লেবুক চালানো:
```bash
# সব ডিভাইসে একসাথে কাজ করবে
ansible-playbook -i hosts site.yml

# শুধু রাউটারে কাজ করবে
ansible-playbook -i hosts site.yml --limit routers

# শুধু মাইক্রোটিকে কাজ করবে
ansible-playbook -i hosts site.yml --limit mikrotiks
```

আউটপুট দেখতে এমন হবে:
```
PLAY [রাউটার কনফিগার করা] *********************

TASK [রাউটারে কমান্ড পাঠানো] *******************
ok: [router1]
ok: [router2]

PLAY [সুইচ কনফিগার করা] ***********************

TASK [VLAN তৈরি করা] ***************************
ok: [switch1]
ok: [switch2]

PLAY [মাইক্রোটিক কনফিগার করা] *****************

TASK [আইপি এড্রেস সেট করা] *********************
ok: [mikrotik1]
ok: [mikrotik2]

PLAY RECAP *************************************
router1    : ok=1    changed=0    unreachable=0    failed=0
router2    : ok=1    changed=0    unreachable=0    failed=0
switch1    : ok=1    changed=0    unreachable=0    failed=0
switch2    : ok=1    changed=0    unreachable=0    failed=0
mikrotik1  : ok=1    changed=0    unreachable=0    failed=0
mikrotik2  : ok=1    changed=0    unreachable=0    failed=0
```

### কি করা হয়েছে?

এখন আমরা আলাপ করি কী করা হয়েছে:

1. **ইনভেন্টরি ফাইল**: এটি হলো একটি তালিকা যেখানে আপনার সব ডিভাইসের নাম এবং তাদের আইপি এড্রেস আছে। আপনি একে একটি কন্টাক্ট লিস্টের মতো ভাবতে পারেন।
2. **প্লেবুক**: এটা হলো একটা ইন্সট্রাকশন যেখানে আপনি বলছেন কোন ডিভাইসে কী কাজ করতে হবে। 
   - প্রথম অংশে বলা হচ্ছে "রাউটার গ্রুপের জন্য এই কাজগুলো করো"।
   - দ্বিতীয় অংশে বলা হচ্ছে "সুইচ গ্রুপের জন্য এই কাজগুলো করো"। মাইক্রোটিকে একই জিনিস।
 - 
3. **টাস্ক**: প্রতিটি ডিভাইসে কী কাজ হবে তা টাস্ক হিসেবে লেখা আছে। উদাহরণস্বরূপ:
4. 
   - রাউটারের জন্য একটি নির্দিষ্ট ফাইল কপি করা এবং সেটি প্রয়োগ করা।
   - সুইচ এবং মাইক্রোটিকের জন্যও একই কাজ।

### সুবিধা

- **স্বয়ংক্রিয়তা**: আপনি একবার নির্দেশিকা তৈরি করলে, এটি সব ডিভাইসে একইভাবে কাজ করবে।
- **দ্রুততা**: একসাথে অনেক ডিভাইসে পরিবর্তন করতে পারবেন, এক এক করে করার দরকার নেই।
- **ভুল কমানো**: একই কাজ বারবার করতে গেলে ভুল হওয়ার সম্ভাবনা থাকে, অ্যান্সিবল তা কমিয়ে আনে।

### উদাহরন ৩ (বোনাস): মাইক্রোটিক রাউটারের বেজলাইন কনফিগারেশন + অটোমেশন

আমাদের নেটওয়ার্কে অনেকগুলো মাইক্রোটিক রাউটারে একটা বেজলাইন কনফিগারেশন কিভাবে পুশ করা যায় সেটার একটা উদাহরণ দিচ্ছি এখানে। (স্টেপ ১ [mikrotiks] দেখুন)

বিশেষ করে, (ফর লুপ চালিয়ে)

```
:local x;
:for x from=10 to=20 do={
      /interface vlan add name="VLAN_$x" vlan-id=$x interface=ether3
```

বিস্তারিত দেখুন এখানে --

```yaml
- hosts: mikrotiks
  connection: network_cli
  gather_facts: no
  vars:
    ansible_network_os: routeros
    ansible_user: rhassan
    ansible_password: password
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
    force_basic_auth: yes
    validate_certs: false

  tasks:
    - name: Adding Vlans to Ether3
      routeros_command:
        commands:
          - >
            :local x;
            :for x from=10 to=20 do={
              /interface vlan add name="VLAN_$x" vlan-id=$x interface=ether3
            }

    - name: Adding IP addresses to LAN
      routeros_command:
        commands:
          - /ip address add address=192.168.100.1/24 interface=VLAN_10

    - name: Add DNS Servers
      routeros_command:
        commands:
          - /ip dns set servers=8.8.8.8,1.1.1.1

    - name: Adding DHCP configuration
      routeros_command:
        commands:
          - /ip pool add name="DHCP LAN" ranges=192.168.100.2-192.168.100.254
          - /ip dhcp-server add address-pool="DHCP LAN" disabled=no interface=VLAN_10 lease-time=8h name="DHCP LAN"
          - /ip dhcp-server network add address=192.168.100.0/24 gateway=192.168.100.1 dns-server=8.8.8.8


    - name: Add Basic FW Rules
      routeros_command:
        commands:
          - /ip firewall nat add chain=srcnat out-interface=ether2 action=masquerade
```
মাথা খুলেছে তো?