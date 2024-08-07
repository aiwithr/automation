পাইথন দিয়ে নেটওয়ার্ক অটোমেশন: একটা উদাহরণ

১. বেসিক অপারেশন: প্রথমে ospf = 10 এবং bgp = 20 এই দুটি ভেরিয়েবল ডিফাইন করা হয়েছে। type(ospf) দিয়ে দেখানো হয়েছে যে ospf একটা integer টাইপের ভেরিয়েবল। bgp + ospf করলে 30 রেজাল্ট আসে।
   ```python
   >>> ospf = 10
   >>> bgp = 20
   >>> bgp + ospf
   30
   ```
   এখানে আমরা দেখতে পাচ্ছি দুটি ভেরিয়েবল `bgp` এবং `ospf` এর যোগফল 30। এভাবে দুটি রাউটিং প্রোটোকল নম্বরের যোগফল হয় না, তবে আমরা একটা উদাহরন টানছি।

২. স্ট্রিং হ্যান্ডলিং:
   ```python
   >>> hostname = "router1"
   >>> type(hostname)
   <class 'str'>
   ```
   এখানে `hostname` নামে একটা স্ট্রিং ভেরিয়েবল তৈরি করা হয়েছে এবং `type()` ফাংশন দিয়ে এর ধরন যাচাই করা হয়েছে।

৩. টাইপ এরর:
   ```python
   >>> hostname + ospf
   TypeError: can only concatenate str (not "int") to str
   ```
   এই লাইনটি দেখাচ্ছে যে Python-এ স্ট্রিং এবং ইন্টিজার সরাসরি যোগ করা যায় না। এটি একটা মারাত্বক ভুল যা নেটওয়ার্ক স্ক্রিপ্টিং করার সময় মাথায় রাখতে হবে।

৪. ভেরিয়েবল অ্যাসাইনমেন্ট:
   ```python
   >>> routing_table = bgp + ospf
   >>> routing_table
   30
   >>> print(routing_table)
   30
   ```
   এখানে দেখানো হয়েছে কীভাবে নতুন ভেরিয়েবল তৈরি করা যায় এবং তার মান প্রিন্ট করা যায়।

৫. লিস্ট ব্যবহার:
   ```python
   >>> devices = ['word.test.com', 'sisko.test.com', 'dax.test.com']
   >>> devices[0]
   'word.test.com'
   >>> devices[1]
   'sisko.test.com'
   ```
   এখানে দেখানো হয়েছে কীভাবে একটা লিস্ট তৈরি করা যায় এবং তার বিভিন্ন এলিমেন্ট অ্যাক্সেস করা যায়। এটি নেটওয়ার্ক ডিভাইসের তালিকা সংরক্ষণ এবং ব্যবহারের জন্য খুবই উপযোগী।

৬. ডিকশনারি ব্যবহার:
   ```python
   >>> bgp_config = {'neighbor_ip': '1.1.1.1', 'neighbor_asn': 65000, 'local_asn': 65001}
   >>> bgp_config['local_asn']
   65001
   ```
   এই উদাহরণে দেখানো হয়েছে কীভাবে একটা ডিকশনারি ব্যবহার করে BGP কনফিগারেশন ডেটা সংরক্ষণ করা যায় এবং তা থেকে নির্দিষ্ট মান বের করা যায়।

এই উদাহরণগুলো দেখিয়ে দেয় কীভাবে Python ব্যবহার করে নেটওয়ার্ক-সংক্রান্ত ডেটা হ্যান্ডল করা যায়, যেমন রাউটিং প্রোটোকল নম্বর, হোস্টনেম, ডিভাইস লিস্ট, এবং কনফিগারেশন ডেটা। এগুলো নেটওয়ার্ক অটোমেশন স্ক্রিপ্ট লেখার সময় অত্যন্ত উপযোগী হবে।

#### পাইথনে ভেরিয়েবল

পাইথন ভেরিয়েবল হল একটা স্টোরেজ কন্টেইনার বা একটা নামযুক্ত মেমোরি অবস্থান যা সাময়িকভাবে একটা মান বা ডেটা সংরক্ষণ করে এবং পরে আপনার কোডে সেই মানের উল্লেখ করতে ব্যবহৃত হয়। ভেরিয়েবল যেকোনো প্রোগ্রামিং ভাষার মৌলিক উপাদান।

ভেরিয়েবল ঘোষণা

পাইথন একটা ডাইনামিক টাইপড ভাষা, যেখানে আপনি প্রথমবার একটা মান নির্ধারণ করলেই একটা ভেরিয়েবল তৈরি হয়। ভেরিয়েবলগুলোকে কোনো নির্দিষ্ট টাইপ দিয়ে ঘোষণা করার প্রয়োজন নেই, এবং সেট করার পরেও এদের পরিবর্তন করা যায়।

```python
# একটা স্ট্রিং মান ভেরিয়েবলে নির্ধারণ
device_name = "Router1"

# একটা পূর্ণসংখ্যা মান ভেরিয়েবলে নির্ধারণ
device_port = 22

# একটা বুলিয়ান মান ভেরিয়েবলে নির্ধারণ
is_connected = True
```

আপনি `type()` ফাংশন ব্যবহার করে একটা ভেরিয়েবলের ডেটা টাইপ পেতে পারেন।

```python
ip_addr = "192.168.10.1"
print(type(ip_addr))
```

আউটপুট:
```
<class 'str'>
```

একটা স্টেটমেন্টে একাধিক ভেরিয়েবলে মান নির্ধারণ করা:

```python
vlan_01, vlan_10 = "default", "mgmt"
print(vlan_01, vlan_10)
```

আউটপুট:
```
default mgmt
```

একই মান একাধিক ভেরিয়েবলে একসাথে নির্ধারণ করা:

```python
host = ip_addr = "192.168.10.1"
print(host, ip_addr)
```

আউটপুট:
```
192.168.10.1 192.168.10.1
```

যদি আপনার কাছে একটা লিস্ট, টাপল ইত্যাদিতে মানের সংগ্রহ থাকে, পাইথন আপনাকে সেই মানগুলো ভেরিয়েবলে বের করতে দেয়:

```python
ip_addr_list = ["10.10.10.10", "172.16.10.10", "192.168.10.10"]

ip_addr1, ip_addr2, ip_addr3 = ip_addr_list

print(ip_addr1)
print(ip_addr2)
print(ip_addr3)
```

আউটপুট:
```
10.10.10.10
172.16.10.10
192.168.10.10
```

অ্যাসাইনমেন্ট স্টেটমেন্ট

একটা অপারেটর হল একটা চিহ্ন যা এক বা একাধিক মানের উপর কাজ করে। একটা বিশেষ চিহ্ন যাকে অ্যাসাইনমেন্ট অপারেটর `=` বলা হয় তা ব্যবহার করে ভেরিয়েবলে মান নির্ধারণ করা হয়। `=` অপারেটর অপারেটরের ডান দিকের মানটি নেয় এবং এটি বাম দিকের নামে নির্ধারণ করে।

```python
hostName = "R-01"
print(hostName)
```

আউটপুট:
```
R-01
```

স্ট্রিং ভেরিয়েবল একক উদ্ধৃতি চিহ্ন `'R-01'` অথবা দ্বি-উদ্ধৃতি চিহ্ন `"R-01"` ব্যবহার করে ঘোষণা করা যায়।

```python
hostName = "R-01"
print(hostName)
```

আউটপুট:
```
R-01
```

```python
hostName = 'R-01'
print(hostName)
```

আউটপুট:
```
R-01
```

এখানে ইথারনেট ইন্টারফেস সম্পর্কিত আরও কিছু উদাহরণ যোগ করছি, যেখানে আমরা Python-এর ডিকশনারি, টাপল এবং লিস্ট ব্যবহার করব। এই উদাহরণগুলো নেটওয়ার্ক অটোমেশনে খুবই উপযোগী:

১. ইথারনেট ইন্টারফেস ডাটা ডিকশনারিতে:

```python
>>> ethernet_interface = {
...     'name': 'GigabitEthernet0/0',
...     'ip_address': '192.168.1.1',
...     'subnet_mask': '255.255.255.0',
...     'status': 'up',
...     'speed': '1000Mbps',
...     'duplex': 'full'
... }

>>> print(ethernet_interface['name'])
GigabitEthernet0/0

>>> print(ethernet_interface['ip_address'])
192.168.1.1
```

ব্যাপারটা বুঝতে চাই: এখানে আমরা একটা ইথারনেট ইন্টারফেসের বিভিন্ন বৈশিষ্ট্য একটা ডিকশনারিতে সংরক্ষণ করেছি। এভাবে আমরা সহজে যেকোনো বৈশিষ্ট্যের মান পেতে পারি।

২. একাধিক ইন্টারফেস ডাটা লিস্টে:

```python
>>> interfaces = [
...     {'name': 'Gi0/0', 'ip': '192.168.1.1', 'status': 'up'},
...     {'name': 'Gi0/1', 'ip': '10.0.0.1', 'status': 'down'},
...     {'name': 'Fa0/0', 'ip': '172.16.0.1', 'status': 'up'}
... ]

>>> for interface in interfaces:
...     print(f"ইন্টারফেস: {interface['name']}, আইপি: {interface['ip']}, স্ট্যাটাস: {interface['status']}")

ইন্টারফেস: Gi0/0, আইপি: 192.168.1.1, স্ট্যাটাস: up
ইন্টারফেস: Gi0/1, আইপি: 10.0.0.1, স্ট্যাটাস: down
ইন্টারফেস: Fa0/0, আইপি: 172.16.0.1, স্ট্যাটাস: up
```
(বাংলাতেও কাজ করবে, তবে ভুলেও সেই কাজ করবেন না)

ব্যাপারটা বুঝতে চাই: এখানে আমরা একাধিক ইন্টারফেসের ডাটা একটা লিস্টে রেখেছি, যেখানে প্রতিটি ইন্টারফেস একটা ডিকশনারি। এরপর আমরা একটা লুপ ব্যবহার করে সব ইন্টারফেসের ডাটা প্রিন্ট করেছি।

৩. ইন্টারফেস কনফিগারেশন টাপল ব্যবহার করে:

```python
>>> interface_config = ('GigabitEthernet0/0', '192.168.1.1', '255.255.255.0', 'up')

>>> name, ip, mask, status = interface_config
>>> print(f"ইন্টারফেস নাম: {name}")
>>> print(f"আইপি অ্যাড্রেস: {ip}")
>>> print(f"সাবনেট মাস্ক: {mask}")
>>> print(f"স্ট্যাটাস: {status}")

ইন্টারফেস নাম: GigabitEthernet0/0
আইপি অ্যাড্রেস: 192.168.1.1
সাবনেট মাস্ক: 255.255.255.0
স্ট্যাটাস: up
```
(বাংলাতেও দেখাবে, তবে ভুলেও সেই কাজ করবেন না)

ব্যাপারটা বুঝতে চাই: এখানে আমরা একটা টাপল ব্যবহার করে ইন্টারফেস কনফিগারেশন সংরক্ষণ করেছি। টাপল ব্যবহার করে আমরা একাধিক ভেরিয়েবলে মান অ্যাসাইন করতে পারি, যা কোড পঠনযোগ্যতা বাড়ায়।

৪. ইন্টারফেস এবং VLAN ডাটা মিশ্রিত ডেটা স্ট্রাকচারে:

```python
>>> switch_config = {
...     'hostname': 'SW-Core-01',
...     'interfaces': [
...         {'name': 'Gi0/0', 'vlan': 10, 'ip': '192.168.10.1'},
...         {'name': 'Gi0/1', 'vlan': 20, 'ip': '192.168.20.1'}
...     ],
...     'vlans': {
...         10: 'Management',
...         20: 'Data',
...         30: 'Voice'
...     }
... }

>>> print(f"Switch Name: {switch_config['hostname']}")
>>> print("\nInterface Data:")
>>> for interface in switch_config['interfaces']:
...     print(f"  {interface['name']} - VLAN: {interface['vlan']}, IP: {interface['ip']}")
>>> print("\nVLAN Data:")
>>> for vlan_id, vlan_name in switch_config['vlans'].items():
...     print(f"  VLAN {vlan_id}: {vlan_name}")

Switch Name: SW-Core-01

Interface Data:
  Gi0/0 - VLAN: 10, IP: 192.168.10.1
  Gi0/1 - VLAN: 20, IP: 192.168.20.1

VLAN Data:
  VLAN 10: Management
  VLAN 20: Data
  VLAN 30: Voice
```

ব্যাপারটা বুঝতে চাই: এই উদাহরণে আমরা একটা জটিল ডেটা স্ট্রাকচার ব্যবহার করেছি যেখানে ডিকশনারি, লিস্ট এবং নেস্টেড ডিকশনারি একসাথে ব্যবহৃত হয়েছে। এটি দেখায় কীভাবে আমরা একটা সুইচের সম্পূর্ণ কনফিগারেশন (হোস্টনেম, ইন্টারফেস এবং VLAN ডাটা) একটা স্ট্রাকচারে সংরক্ষণ ও প্রদর্শন করতে পারি।

আমি বুঝতে পেরেছি। আপনার নির্দেশনা অনুযায়ী, আমি সমস্ত সরবরাহকৃত ডাটা একত্রিত করে একটা পূর্ণাঙ্গ বই তৈরি করছি। এটি হবে "নেটওয়ার্ক অটোমেশনের জন্য পাইথন প্রোগ্রামিং: একটা বিস্তারিত গাইড"। নিচে সেই বইয়ের সম্পূর্ণ বিষয়বস্তু বাংলায় উপস্থাপন করছি:

```python
# একটা স্ট্রিং মান ভেরিয়েবলে নির্ধারণ
device_name = "Router1"

# একটা পূর্ণসংখ্যা মান ভেরিয়েবলে নির্ধারণ
device_port = 22

# একটা বুলিয়ান মান ভেরিয়েবলে নির্ধারণ
is_connected = True
```

আপনি `type()` ফাংশন ব্যবহার করে একটা ভেরিয়েবলের ডেটা টাইপ পেতে পারেন।

```python
ip_addr = "192.168.10.1"
print(type(ip_addr))  # <class 'str'>
```

একটা স্টেটমেন্টে একাধিক ভেরিয়েবলে মান নির্ধারণ করা:

```python
vlan_01, vlan_10 = "default", "mgmt"
print(vlan_01, vlan_10)  # default mgmt
```

একই মান একাধিক ভেরিয়েবলে একসাথে নির্ধারণ করা:

```python
host = ip_addr = "192.168.10.1"
print(host, ip_addr)  # 192.168.10.1 192.168.10.1
```

লিস্ট থেকে ভেরিয়েবলে মান নিষ্কাশন:

```python
ip_addr_list = ["10.10.10.10", "172.16.10.10", "192.168.10.10"]
ip_addr1, ip_addr2, ip_addr3 = ip_addr_list
print(ip_addr1)  # 10.10.10.10
print(ip_addr2)  # 172.16.10.10
print(ip_addr3)  # 192.168.10.10
```

অ্যাসাইনমেন্ট স্টেটমেন্ট:
একটা অপারেটর হল একটা চিহ্ন যা এক বা একাধিক মানের উপর কাজ করে। একটা বিশেষ চিহ্ন যাকে অ্যাসাইনমেন্ট অপারেটর `=` বলা হয় তা ব্যবহার করে ভেরিয়েবলে মান নির্ধারণ করা হয়।

```python
hostName = "R-01"
print(hostName)  # R-01
```

স্ট্রিং ভেরিয়েবল একক উদ্ধৃতি চিহ্ন `'R-01'` অথবা দ্বি-উদ্ধৃতি চিহ্ন `"R-01"` ব্যবহার করে ঘোষণা করা যায়।

**২. পাইথনে লিস্ট**

পাইথনে লিস্ট হল সবচেয়ে বহুমুখী ডেটা টাইপ যা বর্গাকার ব্র্যাকেটের মধ্যে কমা দিয়ে আলাদা করা মানগুলো (আইটেম) হিসেবে লেখা যায়। লিস্ট পরিবর্তনযোগ্য এবং ক্রমানুসারে সাজানো থাকে।

লিস্টের বৈশিষ্ট্য:
- যেকোনো ধরনের অবজেক্ট ধারণ করতে পারে (সংখ্যা, স্ট্রিং, এমনকি অন্য লিস্ট)
- পরিবর্তনযোগ্য (মিউটেবল)
- ক্রমানুসারে সাজানো থাকে

লিস্ট তৈরি:
```python
vlans = ["vlan10", "vlan20", "vlan30"]
print(vlans)  # ["vlan10", "vlan20", "vlan30"]
```

লিস্টের আইটেম পরিবর্তন:
```python
vlans = ["vlan10", "vlan20", "vlan30"]
vlans[2] = "vlan5"
print(vlans)  # ['vlan10', 'vlan20', 'vlan5']
```

নেস্টেড লিস্ট:
```python
my_list = [100, "bottles", ["on", "the", "earth"]]
print(my_list[2])  # ['on', 'the', 'earth']
```

লিস্ট মেথড:

1. `append()` মেথড:
```python
vlan_name = ["mgmt", "prod", "tech", "acct"]
vlan_name.append("guest")
print(vlan_name)  # ["mgmt", "prod", "tech", "acct", "guest"]
```

2. `insert()` মেথড:
```python
vlan_name = ["mgmt", "prod", "tech", "acct"]
vlan_name.insert(2, "guest")
print(vlan_name)  # ["mgmt", "prod", "guest", "tech", "acct"]
```

3. `remove()` এবং `pop()` মেথড:
```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
vlan_name.remove("guest")
print(vlan_name)  # ["mgmt", "prod", "tech", "acct"]

vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
vlan_name.pop()  # 'acct'
vlan_name.pop(2)  # 'guest'
```

4. `extend()` মেথড:
```python
list1 = [1, 2, 3]
list1.extend([4, 5])
print(list1)  # [1, 2, 3, 4, 5]
```

5. `sort()` মেথড:
```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
vlan_name.sort()
print(vlan_name)  # ['acct', 'guest', 'mgmt', 'prod', 'tech']
```

লিস্ট স্লাইসিং:
```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
print(vlan_name[:4])  # ['mgmt', 'prod', 'guest', 'tech']
print(vlan_name[1:3])  # ['prod', 'guest']
print(vlan_name[0:])  # ['mgmt', 'prod', 'guest', 'tech', 'acct']
```

**৩. স্ট্রিং ফরম্যাটিং**

পাইথনে তিনটি স্ট্রিং ফরম্যাটিং পদ্ধতি রয়েছে:

1. `%` পদ্ধতি:
```python
ip_addr = "172.16.10.1"
print("IP Address: %s" % ip_addr)  # IP Address: 172.16.10.1

ios_version = 15
print("iOS Version: %i" % ios_version)  # iOS Version: 15

ip_addr = "172.16.10.1"
mask = "255.255.255.0"
print("IP Address: %s. Mask: %s" % (ip_addr, mask))  # IP Address: 172.16.10.1. Mask: 255.255.255.0
```

2. `format()` পদ্ধতি:
```python
ip_addr = "172.16.10.1"
mask = "255.255.255.0"
print("IP Address: {} Mask: {}".format(ip_addr, mask))  # IP Address: 172.16.10.1 Mask: 255.255.255.0

ip_addr = "172.16.10.1"
subnet = "255.255.255.0"
print("IP Address: {ip} Mask: {mask}".format(ip=ip_addr, mask=subnet))  # IP Address: 172.16.10.1 Mask: 255.255.255.0

credentials = {"user": "admin", "pass": "P@ssW0rd"}
print("Username: {user} \nPassword: {pass}".format(**credentials))
# Username: admin 
# Password: P@ssW0rd

description = """
Device: {}
IP: {}
"""
output = description.format('SW-01', '10.10.10.10')
print(output)
# Device: SW-01
# IP: 10.10.10.10
```

3. f-strings:
```python
ip_addr = '172.16.10.1'
mask = "255.255.255.0"
print(f'IP Address {ip_addr} Mask: {mask}')  # IP Address 172.16.10.1 Mask: 255.255.255.0
```

### ১. আইপি অ্যাড্রেস (IP Address)
আইপি অ্যাড্রেস হল নেটওয়ার্কে একটা ডিভাইসের সনাক্তকরণ নম্বর। এটি সাধারণত দুটি ধরণের হয়: IPv4 এবং IPv6।

```python
# একটা আইপি অ্যাড্রেস স্ট্রিং হিসেবে সংরক্ষণ করা
ip_address = "192.168.1.1"

# একটা সাবনেট মাস্ক সহ আইপি অ্যাড্রেস
subnet_mask = "255.255.255.0"

# পূর্ণ নেটওয়ার্ক অ্যাড্রেস তৈরি করা
network_address = f"{ip_address}/{subnet_mask}"

print(network_address)  # আউটপুট: 192.168.1.1/255.255.255.0
```

### ২. MAC অ্যাড্রেস (MAC Address)
MAC অ্যাড্রেস হল একটা নেটওয়ার্ক ইন্টারফেস কার্ডের (NIC) ইউনিক সনাক্তকারী। এটি সাধারণত 48-বিটের হয় এবং হেক্সাডেসিমাল ফরম্যাটে লেখা হয়।

```python
# একটা MAC অ্যাড্রেস স্ট্রিং হিসেবে সংরক্ষণ করা
mac_address = "00:1A:2B:3C:4D:5E"

print(f"MAC Address: {mac_address}")
# আউটপুট: MAC Address: 00:1A:2B:3C:4D:5E
```

### ৩. VLAN (Virtual Local Area Network)
VLAN একটা লজিক্যাল গ্রুপ যা একই নেটওয়ার্কের বিভিন্ন অংশকে আলাদা করতে ব্যবহৃত হয়।

```python
# VLAN আইডি সংরক্ষণ করা
vlan_id = 10

# VLAN নাম সংরক্ষণ করা
vlan_name = "Management"

print(f"VLAN ID: {vlan_id}, VLAN Name: {vlan_name}")
# আউটপুট: VLAN ID: 10, VLAN Name: Management
```

### ৪. AS সংখ্যা (Autonomous System Number)
AS সংখ্যা একটা অনন্য সনাক্তকারী যা একটা ইন্টারনেট রাউটিং ডোমেনকে সনাক্ত করে।

```python
# AS সংখ্যা সংরক্ষণ করা
as_number = 65000

print(f"AS Number: {as_number}")
# আউটপুট: AS Number: 65000
```

### ৫. স্ট্রিং ফরম্যাটিং
স্ট্রিং ফরম্যাটিং নেটওয়ার্ক কনফিগারেশন তৈরি করার জন্য খুবই গুরুত্বপূর্ণ। পাইথনে তিনটি সাধারণ স্ট্রিং ফরম্যাটিং পদ্ধতি আছে।

#### ৫.১. `%` অপারেটর
```python
ip_address = "192.168.1.1"
print("IP Address: %s" % ip_address)
# আউটপুট: IP Address: 192.168.1.1
```

#### ৫.২. `format()` পদ্ধতি
```python
ip_address = "192.168.1.1"
subnet_mask = "255.255.255.0"
print("IP Address: {} Subnet Mask: {}".format(ip_address, subnet_mask))
# আউটপুট: IP Address: 192.168.1.1 Subnet Mask: 255.255.255.0
```

#### ৫.৩. f-strings
```python
ip_address = "192.168.1.1"
subnet_mask = "255.255.255.0"
print(f"IP Address: {ip_address} Subnet Mask: {subnet_mask}")
# আউটপুট: IP Address: 192.168.1.1 Subnet Mask: 255.255.255.0
```

### ৬. রাউটিং
রাউটিং নেটওয়ার্ক ডেটা প্যাকেটগুলোকে গন্তব্যস্থলে পৌঁছানোর জন্য ব্যবহৃত প্রক্রিয়া। রাউটিং টেবিল তৈরি করা এবং কনফিগার করা খুবই গুরুত্বপূর্ণ।

```python
# রাউটিং টেবিল তৈরি করা
routing_table = [
    {"destination": "0.0.0.0/0", "next_hop": "192.168.1.1"},
    {"destination": "192.168.1.0/24", "next_hop": "192.168.1.1"},
    {"destination": "192.168.2.0/24", "next_hop": "192.168.2.1"},
]

# রাউটিং টেবিল প্রিন্ট করা
for route in routing_table:
    print(f"Destination: {route['destination']}, Next Hop: {route['next_hop']}")
# আউটপুট:
# Destination: 0.0.0.0/0, Next Hop: 192.168.1.1
# Destination: 192.168.1.0/24, Next Hop: 192.168.1.1
# Destination: 192.168.2.0/24, Next Hop: 192.168.2.1
```

### ৭. ফাইল অপারেশন
নেটওয়ার্ক ইঞ্জিনিয়ারদের প্রায়ই বিভিন্ন ফাইল থেকে আইপি অ্যাড্রেস পড়া এবং ফরম্যাট করা প্রয়োজন হয়।

#### ৭.১. ফাইল থেকে আইপি অ্যাড্রেস পড়া
```python
# ফাইল থেকে আইপি অ্যাড্রেস পড়া
with open("ip_addresses.txt", "r") as file:
    ip_addresses = file.readlines()

# পড়া আইপি অ্যাড্রেসগুলো ফরম্যাট করা
formatted_ips = [ip.strip() for ip in ip_addresses]

# ফরম্যাট করা আইপি অ্যাড্রেসগুলো প্রিন্ট করা
for ip in formatted_ips:
    print(f"IP Address: {ip}")
```

#### ৭.২. ফাইল এ আইপি অ্যাড্রেস লেখা
```python
# আইপি অ্যাড্রেসগুলো একটা তালিকায় সংরক্ষণ করা
ip_addresses = ["192.168.1.1", "192.168.2.1", "192.168.3.1"]

# আইপি অ্যাড্রেসগুলো একটা ফাইলে লেখা
with open("ip_addresses_output.txt", "w") as file:
    for ip in ip_addresses:
        file.write(f"{ip}\n")

print("IP addresses have been written to ip_addresses_output.txt")
```

### ৮. নেটওয়ার্ক ডিভাইস থেকে ডেটা সংগ্রহ
SSH বা API ব্যবহার করে নেটওয়ার্ক ডিভাইস থেকে ডেটা সংগ্রহ করা যায়।

#### ৮.১. SSH ব্যবহার করে ডেটা সংগ্রহ
```python
import paramiko

# SSH দিয়ে নেটওয়ার্ক ডিভাইসে সংযোগ স্থাপন
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(hostname="192.168.1.1", username="admin", password="password")

# কমান্ড চালানো
stdin, stdout, stderr = ssh.exec_command("show ip interface brief")
output = stdout.read().decode()

print(output)
ssh.close()
```

#### ৮.২. API ব্যবহার করে ডেটা সংগ্রহ
অনেক নেটওয়ার্ক ডিভাইস RESTful API সমর্থন করে, যা JSON ফরম্যাটে ডেটা প্রদান করে।

```python
import requests

# API ব্যবহার করে ডিভাইস থেকে ডেটা সংগ্রহ
response = requests.get("http://192.168.1.1/api/interfaces", auth=("admin", "password"))
data = response.json()

# JSON ডেটা প্রিন্ট করা
print(data)
```

### উপসংহার
এই ম্যানুয়ালে আমরা পাইথনের বিভিন্ন গুরুত্বপূর্ণ কনসেপ্ট এবং ফিচারগুলো সহজ ভাষায় আলোচনা করেছি যা নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য উপযোগী। এই জ্ঞান ব্যবহার করে নেটওয়ার্ক কনফিগারেশন এবং ম্যানেজমেন্টের কাজগুলো সহজ ও দ্রুত করা যায়।

এই গাইডটি নেটওয়ার্ক অটোমেশনের জন্য পাইথন প্রোগ্রামিংয়ের মৌলিক ধারণাগুলো কভার করে। এটি ভেরিয়েবল, লিস্ট এবং স্ট্রিং ফরম্যাটিং সম্পর্কে বিস্তারিত জ্ঞান প্রদান করে, যা নেটওয়ার্ক স্ক্রিপ্ট লেখার জন্য অত্যন্ত গুরুত্বপূর্ণ।