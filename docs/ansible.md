### অ্যান্সিবল: নেটওয়ার্ক অটোমেশনের একটি শক্তিশালী টুল

অ্যান্সিবল একটি ওপেন সোর্স অটোমেশন টুল যা অত্যন্ত জনপ্রিয় নেটওয়ার্ক ইঞ্জিনিয়ারদের মধ্যে। এটি সরলতা এবং কার্যকারিতার জন্য পরিচিত। অ্যান্সিবলের মাধ্যমে নেটওয়ার্ক ডিভাইস কনফিগারেশন, ডিপ্লয়মেন্ট, এবং অরকেস্ট্রেশন প্রক্রিয়া খুব সহজে এবং দ্রুত সম্পন্ন করা যায়। অ্যান্সিবল মূলত YAML ভাষায় লেখা প্লেবুক ব্যবহার করে কাজ করে, যা সাধারণ মানুষের পড়তে এবং বুঝতে সহজ।

#### অ্যান্সিবল-এর বৈশিষ্ট্য
1. **সহজ**: অ্যান্সিবল ইনস্টল এবং ব্যবহার করা খুব সহজ। কোন এজেন্ট ইনস্টল করার প্রয়োজন হয় না।
2. **ইয়্যামএল ভিত্তিক প্লেবুক**: প্লেবুকগুলো ইয়্যামএল ভাষায় লেখা হয়, যা সহজবোধ্য এবং মানব-পঠনযোগ্য।
3. **এজেন্টবিহীন**: অ্যান্সিবল কোনো এজেন্ট ব্যবহার করে না, এটি SSH এর মাধ্যমে সরাসরি নেটওয়ার্ক ডিভাইসে সংযোগ করে।
4. **দ্রুত ডেপ্লয়মেন্ট**: অ্যান্সিবলের মাধ্যমে দ্রুত কনফিগারেশন এবং ডিপ্লয়মেন্ট সম্পন্ন করা যায়।

### উদাহরণ ১: একাধিক ইন্টারফেস কনফিগারেশন

নিচের প্লেবুকটি একটি সুইচে একাধিক ইন্টারফেস কনফিগার করবে।

##### প্লেবুক (multi_interface_config.yaml)

```yaml
---
- name: Configure multiple interfaces
  hosts: switches
  gather_facts: no
  tasks:
    - name: Configure interface GigabitEthernet0/1
      ios_config:
        lines:
          - description Link to Router
          - ip address 192.168.1.1 255.255.255.0
          - no shutdown
        parents: interface GigabitEthernet0/1

    - name: Configure interface GigabitEthernet0/2
      ios_config:
        lines:
          - description Link to Access Switch
          - ip address 192.168.2.1 255.255.255.0
          - no shutdown
        parents: interface GigabitEthernet0/2
```

এই প্লেবুকটি দুটি ইন্টারফেসে কনফিগারেশন প্রয়োগ করবে। প্রথমে `GigabitEthernet0/1` ইন্টারফেসে এবং তারপর `GigabitEthernet0/2` ইন্টারফেসে।

### উদাহরণ ২: VLAN কনফিগারেশন

এই উদাহরণে, একটি প্লেবুক ব্যবহার করে সুইচে VLAN কনফিগার করা হবে।

##### প্লেবুক (vlan_config.yaml)

```yaml
---
- name: Configure VLANs
  hosts: switches
  gather_facts: no
  tasks:
    - name: Create VLAN 10
      ios_config:
        lines:
          - vlan 10
          - name Management
          
    - name: Create VLAN 20
      ios_config:
        lines:
          - vlan 20
          - name Data
```

এই প্লেবুকটি দুটি VLAN তৈরি করবে: VLAN 10 (Management) এবং VLAN 20 (Data)।

### উদাহরণ ৩: ব্যাকআপ কনফিগারেশন

এই উদাহরণে, একটি প্লেবুক ব্যবহার করে সুইচের কনফিগারেশন ব্যাকআপ নেওয়া হবে।

##### প্লেবুক (backup_config.yaml)

```yaml
---
- name: Backup Switch Configuration
  hosts: switches
  gather_facts: no
  tasks:
    - name: Backup running-config
      ios_command:
        commands:
          - show running-config
      register: running_config

    - name: Save running-config to file
      copy:
        content: "{{ running_config.stdout[0] }}"
        dest: "/path/to/backup/{{ inventory_hostname }}_running-config.txt"
```

এই প্লেবুকটি সুইচের রানিং কনফিগারেশন বের করে এবং সেটি একটি ফাইলে সংরক্ষণ করে।

### উদাহরণ ৪: লুপ ব্যবহার করে অটোমেশন

নিচের প্লেবুকটি একটি লুপ ব্যবহার করে একাধিক VLAN কনফিগার করবে।

##### প্লেবুক (loop_vlan_config.yaml)

```yaml
---
- name: Configure multiple VLANs using loop
  hosts: switches
  gather_facts: no
  tasks:
    - name: Create VLANs
      ios_config:
        lines:
          - vlan {{ item.id }}
          - name {{ item.name }}
      loop:
        - { id: 10, name: Management }
        - { id: 20, name: Data }
        - { id: 30, name: Voice }
```

এই প্লেবুকটা একটা লুপ ব্যবহার করে তিনটি VLAN তৈরি করবে: VLAN 10 (Management), VLAN 20 (Data), এবং VLAN 30 (Voice)।

### অ্যান্সিবল ইনস্টলেশন এবং ব্যবহার

অ্যান্সিবল ইনস্টল করা খুবই সহজ। আপনি নিচের কমান্ডটি ব্যবহার করে অ্যান্সিবল ইনস্টল করতে পারেন:

```bash
pip install ansible
```

ইনস্টলেশনের পরে, আপনি একটি ইনভেন্টরি ফাইল তৈরি করতে পারেন যেখানে আপনার সুইচগুলির আইপি অ্যাড্রেস এবং লগইন ক্রেডেনশিয়াল থাকবে। উদাহরণস্বরূপ:

##### ইনভেন্টরি ফাইল (hosts)

```ini
[switches]
192.168.1.10 ansible_user=admin ansible_password=admin
192.168.1.11 ansible_user=admin ansible_password=admin
```

এরপর, আপনি প্লেবুক রান করতে পারবেন নিচের কমান্ডটি ব্যবহার করে:

```bash
ansible-playbook -i hosts multi_interface_config.yaml
```
এখানে:

- ansible-playbook হল মূল কমান্ড
- -i hosts দিয়ে বলে দিচ্ছি কোন ফাইলে সুইচের লিস্ট আছে
- multi_interface_config.yaml হল আপনার প্লেবুক ফাইলের নাম

কমান্ড রান করার পর অ্যান্সিবল প্রতিটা ধাপে কী করছে তা দেখাবে। সবুজ রঙ মানে কাজ সফল, লাল রঙ মানে কিছু একটা ভুল হয়েছে।

আমি আপনাকে কয়েকটি পূর্ণাঙ্গ উদাহরণ দেখাবো। প্রতিটি উদাহরণে আমরা ধাপে ধাপে দেখব কীভাবে অ্যান্সিবল ব্যবহার করে বিভিন্ন কাজ করা যায়।

### কমপ্লেক্স উদাহরণ ৫: সুইচে ভিল্যান + ইন্টারফেস

ধরুন, আপনার অফিসে ৫টা নতুন সুইচ এসেছে। প্রতিটি সুইচে ৩টা ভিল্যান এবং কয়েকটা ইন্টারফেস কনফিগার করতে হবে। চলুন দেখি কীভাবে করা যায়:

১. প্রথমে হোস্ট ফাইল বানাই:
```ini
# office_switches.ini
[switches]
switch01 ansible_host=192.168.1.10 ansible_user=admin ansible_password=secure123
switch02 ansible_host=192.168.1.11 ansible_user=admin ansible_password=secure123
switch03 ansible_host=192.168.1.12 ansible_user=admin ansible_password=secure123
switch04 ansible_host=192.168.1.13 ansible_user=admin ansible_password=secure123
switch05 ansible_host=192.168.1.14 ansible_user=admin ansible_password=secure123
```

২. এরপর প্লেবুক লিখি:
```yaml
# office_setup.yml
---
- name: অফিসের নতুন সুইচ সেটআপ
  hosts: switches
  gather_facts: no
  tasks:
    # প্রথমে ভিল্যান বানাই
    - name: ভিল্যান তৈরি
      ios_config:
        lines:
          - vlan {{ item.id }}
          - name {{ item.name }}
      loop:
        - { id: 10, name: Management }
        - { id: 20, name: Staff }
        - { id: 30, name: Guest }
      
    # এরপর ইন্টারফেস কনফিগার করি
    - name: ইন্টারফেস সেটআপ
      ios_config:
        lines:
          - description {{ item.desc }}
          - switchport mode {{ item.mode }}
          - switchport access vlan {{ item.vlan }}
          - no shutdown
        parents: interface {{ item.name }}
      loop:
        - { name: GigabitEthernet0/1, desc: "Management Port", mode: access, vlan: 10 }
        - { name: GigabitEthernet0/2, desc: "Staff Port", mode: access, vlan: 20 }
        - { name: GigabitEthernet0/3, desc: "Guest Port", mode: access, vlan: 30 }
```

৩. প্লেবুক রান করি:
```bash
ansible-playbook -i office_switches.ini office_setup.yml
```

### কমপ্লেক্স উদাহরণ ৬: ব্যাকআপ এবং মনিটরিং

এবার আমরা দেখব কীভাবে সব সুইচের ব্যাকআপ নিতে হয় এবং তাদের স্ট্যাটাস চেক করতে হয়:

১. প্লেবুক লিখি:
```yaml
# backup_monitor.yml
---
- name: সুইচের ব্যাকআপ এবং স্ট্যাটাস চেক
  hosts: switches
  gather_facts: no
  tasks:
    # কনফিগারেশন ব্যাকআপ
    - name: রানিং কনফিগ দেখা
      ios_command:
        commands:
          - show running-config
      register: backup
    
    - name: ব্যাকআপ সেভ করা
      copy:
        content: "{{ backup.stdout[0] }}"
        dest: "./backups/{{ inventory_hostname }}_{{ ansible_date_time.date }}.txt"
    
    # স্ট্যাটাস চেক
    - name: ইন্টারফেস স্ট্যাটাস
      ios_command:
        commands:
          - show ip interface brief
      register: interfaces
    
    - name: ভিল্যান স্ট্যাটাস
      ios_command:
        commands:
          - show vlan brief
      register: vlans
    
    # রিপোর্ট তৈরি
    - name: HTML রিপোর্ট বানানো
      template:
        src: report.j2
        dest: "./reports/{{ inventory_hostname }}_report.html"
```

২. রিপোর্টের টেমপ্লেট লিখি (report.j2):
```html
<html>
<head>
    <title>সুইচ স্ট্যাটাস রিপোর্ট</title>
</head>
<body>
    <h1>{{ inventory_hostname }} এর স্ট্যাটাস</h1>
    <h2>ইন্টারফেস স্ট্যাটাস:</h2>
    <pre>{{ interfaces.stdout[0] }}</pre>
    
    <h2>ভিল্যান স্ট্যাটাস:</h2>
    <pre>{{ vlans.stdout[0] }}</pre>
</body>
</html>
```

৩. প্লেবুক রান করি:
```bash
ansible-playbook -i office_switches.ini backup_monitor.yml
```

### কমপ্লেক্স উদাহরণ ৭: বড় আপডেট

ধরুন, অফিসের সব সুইচে একসাথে আপডেট করতে হবে:

```yaml
# major_update.yml
---
- name: সুইচ আপডেট
  hosts: switches
  gather_facts: no
  tasks:
    # প্রথমে ব্যাকআপ নিই
    - name: আপডেটের আগে ব্যাকআপ
      ios_command:
        commands:
          - show running-config
      register: pre_update_backup
    
    - name: ব্যাকআপ সেভ
      copy:
        content: "{{ pre_update_backup.stdout[0] }}"
        dest: "./pre_update_backup/{{ inventory_hostname }}.txt"
    
    # সিকিউরিটি আপডেট
    - name: নতুন সিকিউরিটি পলিসি
      ios_config:
        lines:
          - service password-encryption
          - security passwords min-length 8
          - login block-for 120 attempts 3 within 60
    
    # নতুন ভিল্যান এবং ACL
    - name: নতুন নেটওয়ার্ক পলিসি
      ios_config:
        lines:
          - vlan {{ item.id }}
          - name {{ item.name }}
      loop:
        - { id: 100, name: DevTeam }
        - { id: 110, name: Finance }
    
    # কনফিগ সেভ
    - name: কনফিগারেশন সেভ
      ios_command:
        commands:
          - write memory
```

প্রতিটি উদাহরণে দেখুন কীভাবে ধাপে ধাপে কাজগুলো করা হচ্ছে। মনে রাখবেন:

১. প্রতিবার কোন বড় পরিবর্তন করার আগে ব্যাকআপ নিন
২. টেস্ট এনভায়রনমেন্টে আগে চেক করুন
৩. প্লেবুকে ভালো কমেন্ট দিন
৪. একটা লগ ফাইল রাখুন কী কী পরিবর্তন করলেন

এভাবে অ্যান্সিবল ব্যবহার করে আপনি জটিল নেটওয়ার্ক ম্যানেজমেন্টের কাজগুলোও সহজে করে ফেলতে পারবেন।