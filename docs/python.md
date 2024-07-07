### পাইথন দিয়ে নেটওয়ার্ক অটোমেশন: হাটি হাটি পা পা 

১. বেসিক অপারেশন: প্রথমে ospf = 10 এবং bgp = 20 এই দুটি ভেরিয়েবল ডিফাইন করা হয়েছে। type(ospf) দিয়ে দেখানো হয়েছে যে ospf একটি integer টাইপের ভেরিয়েবল। bgp + ospf করলে 30 ফলাফল আসে।
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
   এখানে `hostname` নামে একটি স্ট্রিং ভেরিয়েবল তৈরি করা হয়েছে এবং `type()` ফাংশন দিয়ে এর ধরন যাচাই করা হয়েছে।

৩. টাইপ এরর:
   ```python
   >>> hostname + ospf
   TypeError: can only concatenate str (not "int") to str
   ```
   এই লাইনটি দেখাচ্ছে যে Python-এ স্ট্রিং এবং ইন্টিজার সরাসরি যোগ করা যায় না। এটি একটি মারাত্বক ভুল যা নেটওয়ার্ক স্ক্রিপ্টিং করার সময় মাথায় রাখতে হবে।

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
   এখানে দেখানো হয়েছে কীভাবে একটি লিস্ট তৈরি করা যায় এবং তার বিভিন্ন এলিমেন্ট অ্যাক্সেস করা যায়। এটি নেটওয়ার্ক ডিভাইসের তালিকা সংরক্ষণ এবং ব্যবহারের জন্য খুবই উপযোগী।

৬. ডিকশনারি ব্যবহার:
   ```python
   >>> bgp_config = {'neighbor_ip': '1.1.1.1', 'neighbor_asn': 65000, 'local_asn': 65001}
   >>> bgp_config['local_asn']
   65001
   ```
   এই উদাহরণে দেখানো হয়েছে কীভাবে একটি ডিকশনারি ব্যবহার করে BGP কনফিগারেশন ডেটা সংরক্ষণ করা যায় এবং তা থেকে নির্দিষ্ট মান বের করা যায়।

এই উদাহরণগুলি দেখিয়ে দেয় কীভাবে Python ব্যবহার করে নেটওয়ার্ক-সংক্রান্ত ডেটা হ্যান্ডল করা যায়, যেমন রাউটিং প্রোটোকল নম্বর, হোস্টনেম, ডিভাইস লিস্ট, এবং কনফিগারেশন ডেটা। এগুলি নেটওয়ার্ক অটোমেশন স্ক্রিপ্ট লেখার সময় অত্যন্ত উপযোগী হবে।

#### পাইথনে ভেরিয়েবল: একটু ডিপ ডাইভ

পাইথন ভেরিয়েবল হল একটি স্টোরেজ কন্টেইনার বা একটি নামযুক্ত মেমোরি অবস্থান যা সাময়িকভাবে একটি মান বা ডেটা সংরক্ষণ করে এবং পরে আপনার কোডে সেই মানের উল্লেখ করতে ব্যবহৃত হয়। ভেরিয়েবল যেকোনো প্রোগ্রামিং ভাষার মৌলিক উপাদান।

ভেরিয়েবল ডিক্লেয়ার

পাইথন একটি ডাইনামিক টাইপড ভাষা, যেখানে আপনি প্রথমবার একটি মান নির্ধারণ করলেই একটি ভেরিয়েবল তৈরি হয়। ভেরিয়েবলগুলিকে কোনো নির্দিষ্ট টাইপ দিয়ে ঘোষণা করার প্রয়োজন নেই, এবং সেট করার পরেও এদের পরিবর্তন করা যায়।

```python
# একটি স্ট্রিং মান ভেরিয়েবলে নির্ধারণ
device_name = "Router1"

# একটি পূর্ণসংখ্যা মান ভেরিয়েবলে নির্ধারণ
device_port = 22

# একটি বুলিয়ান মান ভেরিয়েবলে নির্ধারণ
is_connected = True
```

আপনি `type()` ফাংশন ব্যবহার করে একটি ভেরিয়েবলের ডেটা টাইপ পেতে পারেন।

```python
ip_addr = "192.168.10.1"
print(type(ip_addr))
```

আউটপুট:
```
<class 'str'>
```

একটি স্টেটমেন্টে একাধিক ভেরিয়েবলে মান নির্ধারণ করা:

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

যদি আপনার কাছে একটি লিস্ট, টাপল ইত্যাদিতে মানের সংগ্রহ থাকে, পাইথন আপনাকে সেই মানগুলি ভেরিয়েবলে বের করতে দেয়:

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

### অ্যাসাইনমেন্ট স্টেটমেন্ট

একটি অপারেটর হল একটি চিহ্ন যা এক বা একাধিক মানের উপর কাজ করে। একটি বিশেষ চিহ্ন যাকে অ্যাসাইনমেন্ট অপারেটর `=` বলা হয় তা ব্যবহার করে ভেরিয়েবলে মান নির্ধারণ করা হয়। `=` অপারেটর অপারেটরের ডান দিকের মানটি নেয় এবং এটি বাম দিকের নামে নির্ধারণ করে।

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

### ১. আইপি ঠিকানা (IP Address)
আইপি ঠিকানা হল নেটওয়ার্কে একটি ডিভাইসের সনাক্তকরণ নম্বর। এটি সাধারণত দুটি ধরণের হয়: IPv4 এবং IPv6।

```python
# একটি আইপি ঠিকানা স্ট্রিং হিসেবে সংরক্ষণ করা
ip_address = "192.168.1.1"

# একটি সাবনেট মাস্ক সহ আইপি ঠিকানা
subnet_mask = "255.255.255.0"

# পূর্ণ নেটওয়ার্ক ঠিকানা তৈরি করা
network_address = f"{ip_address}/{subnet_mask}"

print(network_address)  # আউটপুট: 192.168.1.1/255.255.255.0
```

### ২. MAC ঠিকানা (MAC Address)
MAC ঠিকানা হল একটি নেটওয়ার্ক ইন্টারফেস কার্ডের (NIC) ইউনিক সনাক্তকারী। এটি সাধারণত 48-বিটের হয় এবং হেক্সাডেসিমাল ফরম্যাটে লেখা হয়।

```python
# একটি MAC ঠিকানা স্ট্রিং হিসেবে সংরক্ষণ করা
mac_address = "00:1A:2B:3C:4D:5E"

print(f"MAC Address: {mac_address}")
# আউটপুট: MAC Address: 00:1A:2B:3C:4D:5E
```

### ৩. VLAN (Virtual Local Area Network)
VLAN একটি লজিক্যাল গ্রুপ যা একই নেটওয়ার্কের বিভিন্ন অংশকে আলাদা করতে ব্যবহৃত হয়।

```python
# VLAN আইডি সংরক্ষণ করা
vlan_id = 10

# VLAN নাম সংরক্ষণ করা
vlan_name = "Management"

print(f"VLAN ID: {vlan_id}, VLAN Name: {vlan_name}")
# আউটপুট: VLAN ID: 10, VLAN Name: Management
```

### ৪. AS সংখ্যা (Autonomous System Number)
AS সংখ্যা একটি অনন্য সনাক্তকারী যা একটি ইন্টারনেট রাউটিং ডোমেনকে সনাক্ত করে।

```python
# AS সংখ্যা সংরক্ষণ করা
as_number = 65000

print(f"AS Number: {as_number}")
# আউটপুট: AS Number: 65000
```

### লিস্ট মেলানো (List Concatenation)

লিস্টের সংযোজনের জন্য আমরা + অপারেটর ব্যবহার করতে পারি। এটি একটি নতুন লিস্ট তৈরি করে দেয় যা দুটি লিস্টকে যুক্ত করে।

```python
list1 = [10, 11, 12, 13, 14]
list2 = [20, 30, 42]

result = list1 + list2
print(result)
# [10, 11, 12, 13, 14, 20, 30, 42]
```

### লিস্ট মেথড

লিস্টের কিছু মেথড রয়েছে যা লিস্টে তথ্য যোগ করতে এবং ম্যানেজ করতে ব্যবহৃত হয়।

#### `append()` মেথড

এই মেথডটি দিয়ে নতুন আইটেম/উপাদান যোগ করা যায় একটি বর্তমান লিস্টে।

```python
vlan_name = ["mgmt", "prod", "tech", "acct"]
print(vlan_name)  # ["mgmt", "prod", "tech", "acct"]

vlan_name.append("guest")
print(vlan_name)  # ["mgmt", "prod", "tech", "acct", "guest"]
```

#### `insert()` মেথড

এই ফাংশনটি `append()` ফাংশনের মতোই কাজ করে কিন্তু এটি নতুন আইটেমটি বর্তমান লিস্টের কোন নির্দিষ্ট অবস্থানে প্রদর্শন করে।

```python
vlan_name = ["mgmt", "prod", "tech", "acct"]
print(vlan_name)  # ["mgmt", "prod", "tech", "acct"]

vlan_name.insert(2, "guest")
print(vlan_name)  # ["mgmt", "prod", "guest", "tech", "acct"]
```

#### `remove()` এবং `pop()` মেথড

এটি লিস্ট থেকে একটি আইটেম মুছে ফেলতে ব্যবহৃত হয়। যখন একটি আইটেম লিস্টে একাধিকবার উপস্থিত থাকে, `remove()` কমান্ড শুধুমাত্র প্রথম আইটেমটি মুছে ফেলবে।

```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
print(vlan_name)  # ["mgmt", "prod", "guest", "tech", "acct"]

vlan_name.remove("guest")
print(vlan_name)  # ["mgmt", "prod", "tech", "acct"]
```

যদি কমান্ডটি লিস্টে থাকা না থাকে তবে ফাংশনগুলি একটি ত্রুটি ফেরত দেবে যা বুঝায় যে আইটেমটি লিস্টে নেই।

```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
vlan_name.remove("tomato")
# ValueError: list.remove(x): x not in list
```

আইটেমটির অবস্থান দিয়ে আইটেম মুছে ফেলতে `pop()` ফাংশনটি ব্যবহৃত হয়। যখন এই মেথডটি অবস্থান উল্লেখ ছাড়াই কল করা হয়, তখন শেষের আইটেম মুছে ফেলে।

```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
vlan_name.pop()  # শেষের আইটেম মুছে ফেলবে
# 'acct'

vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
vlan_name.pop(2)  # সূচক 2 এ নম্বর 3 মুছে ফেলবে
# 'guest'
```

#### `extend()` মেথড

এই মেথডটি একটি লিস্টকে অন্য লিস্টের সাথে যুক্ত করতে ব্যবহৃত হয়। এটি `append()` থেকে আলাদা যে, এটি দুটি লিস্টের যোগাযোগ সম্পর্কিত। এই মেথডটি প্রথম লিস্টের শেষে দ্বিতীয় লিস্টটি যুক্ত করে এবং প্রথম লিস্টটি পরিবর্তন করে।

```python
list1 = [1, 2, 3]
list1.extend([4, 5])
print(list1)
# [1, 2, 3, 4, 5]
```

### লিস্টে আইটেম সাজানো

লিস্টের আইটেমগুলি সাজানোর জন্য আমরা একটি মেথড ব্যবহার করতে পারি।

```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
vlan_name.sort()
print(vlan_name)
# ['acct', 'guest', 'mgmt', 'prod', 'tech']
```

### `del` স্টেটমেন্ট

একটি লিস্ট থেকে একটি আইটেম মুছে ফেলতে হলে তার ইনডেক্স ব্যবহার করা যাবে, তার বিপরীতে তার মান: ডেল স্টেটমেন্ট। এটা একটা জনপ্রিয় ফাংশন।

```python
# remove an item from the list
del vlan_name[0]
print(vlan_name)
# ['guest', 'mgmt', 'prod', 'tech']
```

### লিস্ট স্লাইসিং

লিস্ট স্লাইসিং হ'ল পাইথনের একটি নীতি, যা সাহায্য করে একটি লিস্ট পড়া এবং একটি নির্দিষ্ট সেগমেন্ট ফিরে একটি নতুন লিস্টের সীমা যোগ করে। লিস্ট স্লাইসিং-এর সবচেয়ে গুরুত্বপূর্ণ বিষয় হ'ল colon : এবং তার নাম যা স্লাইসিং বিবৃতি সৃষ্টি করার ক্ষেত্রে ব্যবহৃত হয়। একক শব্দে কোন লিস্ট স্লাইসিং হচ্ছে তা নিম্নে প্রদর্শিত হয়।

```python
vlan_name = ["mgmt", "prod", "guest", "tech", "acct"]
print(vlan_name[:4])
print(vlan_name[1:3])  # সূচক 3 ব্যতীত উপস্থিত নয়
print(vlan_name[0:])
# ['mgmt', 'prod', 'guest', 'tech']
# ['prod', 'guest']
# ['mgmt', 'prod', 'guest', 'tech', 'acct']
```

একটি শর্টকাট হিসাবে, প্রথম সূচক বিন্যাস একই হিসাবে 0 এর চেয়ে, বা লিস্টের শেষে পূর্ণাঙ্গ লিস্ট স্লাইসিং হচ্ছে।

এখানে ইথারনেট ইন্টারফেস সম্পর্কিত আরও কিছু উদাহরণ যোগ করছি, যেখানে আমরা Python-এর ডিকশনারি, টাপল এবং লিস্ট ব্যবহার করব। এই উদাহরণগুলি নেটওয়ার্ক অটোমেশনে খুবই উপযোগী:

১. ইথারনেট ইন্টারফেস তথ্য ডিকশনারিতে:

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

ব্যাখ্যা: এখানে আমরা একটি ইথারনেট ইন্টারফেসের বিভিন্ন বৈশিষ্ট্য একটি ডিকশনারিতে সংরক্ষণ করেছি। এভাবে আমরা সহজে যেকোনো বৈশিষ্ট্যের মান পেতে পারি।

২. একাধিক ইন্টারফেস তথ্য লিস্টে:

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
(বাংলাতেও কাজ করবে, তবে স্ক্রিপ্টে ভুলেও সেই কাজ করবেন না - যদি না ওয়েবসার্ভিসে কাজ করেন)

ব্যাখ্যা: এখানে আমরা একাধিক ইন্টারফেসের তথ্য একটি লিস্টে রেখেছি, যেখানে প্রতিটি ইন্টারফেস একটি ডিকশনারি। এরপর আমরা একটি লুপ ব্যবহার করে সব ইন্টারফেসের তথ্য প্রিন্ট করেছি।

৩. ইন্টারফেস কনফিগারেশন টাপল ব্যবহার করে:

```python
>>> interface_config = ('GigabitEthernet0/0', '192.168.1.1', '255.255.255.0', 'up')

>>> name, ip, mask, status = interface_config
>>> print(f"ইন্টারফেস নাম: {name}")
>>> print(f"আইপি ঠিকানা: {ip}")
>>> print(f"সাবনেট মাস্ক: {mask}")
>>> print(f"স্ট্যাটাস: {status}")

ইন্টারফেস নাম: GigabitEthernet0/0
আইপি ঠিকানা: 192.168.1.1
সাবনেট মাস্ক: 255.255.255.0
স্ট্যাটাস: up
```
(বাংলাতেও দেখাবে, তবে ভুলেও সেই কাজ করবেন না - যদি না ওয়েবসার্ভিসে কাজ করেন)

ব্যাখ্যা: এখানে আমরা একটি টাপল ব্যবহার করে ইন্টারফেস কনফিগারেশন সংরক্ষণ করেছি। টাপল ব্যবহার করে আমরা একাধিক ভেরিয়েবলে মান অ্যাসাইন করতে পারি, যা কোড পঠনযোগ্যতা বাড়ায়।

৪. ইন্টারফেস এবং VLAN তথ্য মিশ্রিত ডেটা স্ট্রাকচারে:

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

ব্যাখ্যা: এই উদাহরণে আমরা একটি জটিল ডেটা স্ট্রাকচার ব্যবহার করেছি যেখানে ডিকশনারি, লিস্ট এবং নেস্টেড ডিকশনারি একসাথে ব্যবহৃত হয়েছে। এটি দেখায় কীভাবে আমরা একটি সুইচের সম্পূর্ণ কনফিগারেশন (হোস্টনেম, ইন্টারফেস এবং VLAN তথ্য) একটি স্ট্রাকচারে সংরক্ষণ ও প্রদর্শন করতে পারি।

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
রাউটিং নেটওয়ার্ক ডেটা প্যাকেটগুলিকে গন্তব্যস্থলে পৌঁছানোর জন্য ব্যবহৃত প্রক্রিয়া। রাউটিং টেবিল তৈরি করা এবং কনফিগার করা খুবই গুরুত্বপূর্ণ।

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
নেটওয়ার্ক ইঞ্জিনিয়ারদের প্রায়ই বিভিন্ন ফাইল থেকে আইপি ঠিকানা পড়া এবং ফরম্যাট করা প্রয়োজন হয়।

#### ৭.১. ফাইল থেকে আইপি ঠিকানা পড়া
```python
# ফাইল থেকে আইপি ঠিকানা পড়া
with open("ip_addresses.txt", "r") as file:
    ip_addresses = file.readlines()

# পড়া আইপি ঠিকানাগুলি ফরম্যাট করা
formatted_ips = [ip.strip() for ip in ip_addresses]

# ফরম্যাট করা আইপি ঠিকানাগুলি প্রিন্ট করা
for ip in formatted_ips:
    print(f"IP Address: {ip}")
```

#### ৭.২. ফাইল এ আইপি ঠিকানা লেখা
```python
# আইপি ঠিকানাগুলি একটি তালিকায় সংরক্ষণ করা
ip_addresses = ["192.168.1.1", "192.168.2.1", "192.168.3.1"]

# আইপি ঠিকানাগুলি একটি ফাইলে লেখা
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

### কিছু ধারণা তো হলো?
এই ম্যানুয়ালে আমরা পাইথনের বিভিন্ন গুরুত্বপূর্ণ কনসেপ্ট এবং ফিচারগুলি সহজ ভাষায় আলোচনা করেছি যা নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য উপযোগী। এই জ্ঞান ব্যবহার করে নেটওয়ার্ক কনফিগারেশন এবং ম্যানেজমেন্টের কাজগুলি সহজ ও দ্রুত করা যায়।

এই গাইডটি নেটওয়ার্ক অটোমেশনের জন্য পাইথন প্রোগ্রামিংয়ের মৌলিক ধারণাগুলি কভার করে। এটি ভেরিয়েবল, লিস্ট এবং স্ট্রিং ফরম্যাটিং সম্পর্কে বিস্তারিত জ্ঞান প্রদান করে, যা নেটওয়ার্ক স্ক্রিপ্ট লেখার জন্য অত্যন্ত গুরুত্বপূর্ণ।