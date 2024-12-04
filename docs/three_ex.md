!!! tldr "শুধুমাত্র কোড বোঝার জন্য"
    এই চ্যাপ্টারের কোডগুলো বোঝার জন্য, প্র্যাকটিস করার জন্য নয়। প্র্যাকটিসের কোড পাবেন পরের চ্যাপ্টার থেকে। পূর্ণাঙ্গ কোড পাবেন "আমাদের কিছু 'ইউজড' কেস" চ্যাপ্টারে।

# তিনটা প্রধান পাইথন লাইব্রেরি: লাইন বাই লাইন বোঝা যাক

চলুন আমরা একটা একটা করে তিনটা প্রধান পাইথনের নেটওয়ার্ক অটোমেশন লাইব্রেরি বুঝে নেই। আরো অনেক লাইব্রেরি আছে, তবে এগুলো দিয়ে শুরু করা যাক। প্রতিটা লাইন কি কাজ করে, কেন করে - সব দেখব। আমাদের আগের চ্যাপ্টারের পাইথন থেকে শেখা জিনিস নিয়েই এখানে আলাপ।

## ১. পাইথন রিকোয়েস্টস (Python Requests)

এটা পাইথনের সবচেয়ে জনপ্রিয় এবং শক্তিশালী লাইব্রেরিগুলোর একটা। Requests লাইব্রেরি কেন এত জনপ্রিয়? কারণ এটা ইন্টারনেট থেকে ডাটা আনা-নেওয়াকে খুব সহজ করে দেয়। আমরা যেমন ব্রাউজারে ওয়েবসাইট দেখি, requests দিয়েও তেমনি ওয়েবসাইট থেকে ডাটা নিয়ে আসতে পারি। প্রথমে পাইথনে ইনস্টলেশন: pip হলো পাইথনের প্যাকেজ ম্যানেজার - এটা দিয়ে আমরা পাইথনের জন্য নতুন লাইব্রেরি ইনস্টল করতে পারি। কমান্ড প্রম্পট এ লিখুন:
```bash
pip install requests
```
এই কমান্ড requests লাইব্রেরি আমাদের সিস্টেমে ইনস্টল করবে।

এবার বেসিক ব্যবহার দেখি:
```python
# requests মডিউল আনি
import requests

# একটা সহজ GET রিকোয়েস্ট করি
response = requests.get('http://api.example.com/network/devices')
```
এখানে requests.get() ফাংশন একটা URL এ GET রিকোয়েস্ট পাঠায়। যেমন আমরা ব্রাউজারে URL লিখে এন্টার দেই, ঠিক তেমন।

```python
# রেসপন্স চেক করি
print(response.status_code)    # HTTP স্ট্যাটাস কোড দেখাবে (200 মানে সফল)
print(response.json())        # সার্ভার যে JSON ডেটা পাঠিয়েছে তা দেখাবে
```
response.status_code দিয়ে আমরা বুঝতে পারি রিকোয়েস্ট সফল হয়েছে কিনা। 200 মানে সব ঠিক আছে।

এবার একটু জটিল উদাহরণ:
```python
# কনফিগারেশন ডেটা তৈরি করি
device_config = {
    "hostname": "core-switch-01",    # সুইচের নাম
    "interfaces": {                  # ইন্টারফেস সেটিংস
        "Ethernet1": {
            "description": "Link to Router",  # পোর্টের বর্ণনা
            "enabled": True                   # পোর্ট চালু থাকবে
        }
    }
}

# POST রিকোয়েস্ট পাঠাই - নতুন কনফিগারেশন সেট করার জন্য
response = requests.post(
    'http://api.example.com/devices/config',  # কোথায় পাঠাব
    json=device_config,                       # কি ডেটা পাঠাব
    auth=('admin', 'secret')                  # লগইন ক্রেডেনশিয়াল
)
```
এখানে আমরা একটা POST রিকোয়েস্ট পাঠাচ্ছি নতুন কনফিগারেশন সেট করার জন্য। device_config একটা ডিকশনারি যেখানে আমরা কনফিগারেশনের বিবরণ রেখেছি।

## ২. NETCONF ক্লায়েন্ট (ncclient)

```python
# ncclient মডিউল থেকে manager ক্লাস নিয়ে আসি
from ncclient import manager

# NETCONF সেশন তৈরি করি
with manager.connect(
    host="192.168.1.1",        # ডিভাইসের IP
    port=830,                  # NETCONF পোর্ট নম্বর
    username="admin",          # ইউজারনেম
    password="secret",         # পাসওয়ার্ড
    hostkey_verify=False       # সার্ভার ভেরিফিকেশন বন্ধ (শুধু টেস্টিংয়ে)
) as m:
    # বর্তমান কনফিগারেশন দেখি
    config = m.get_config(source='running')  # রানিং কনফিগ নিয়ে আসি
    print(config.xml)                        # XML ফরম্যাটে দেখি
```
এখানে manager.connect() দিয়ে আমরা NETCONF সেশন খুলছি। with স্টেটমেন্ট ব্যবহার করায় সেশন অটোমেটিক বন্ধ হয়ে যাবে।

## NETCONF ক্লায়েন্ট (ncclient) - চালিয়ে যাই

```python
# নতুন কনফিগারেশন তৈরি করি XML ফরম্যাটে
new_config = """
    <config>
        <interfaces>
            <interface>
                <name>GigabitEthernet1</name>             
                <description>Updated via NETCONF</description>
            </interface>
        </interfaces>
    </config>
"""
# এখানে আমরা XML ফরম্যাটে কনফিগ লিখছি কারণ NETCONF XML ব্যবহার করে
# interfaces ট্যাগের মধ্যে interface এর বিবরণ দিচ্ছি

# কনফিগারেশন আপডেট করি
m.edit_config(
    target='running',      # রানিং কনফিগ আপডেট করব
    config=new_config      # আমাদের নতুন কনফিগ
)
```

এখানে edit_config() ফাংশন দিয়ে আমরা নতুন কনফিগারেশন পুশ করছি। target='running' মানে বর্তমান চলমান কনফিগারেশন আপডেট করা হবে।

## ৩. Netmiko - SSH ক্লায়েন্ট

```python
# Netmiko মডিউল থেকে ConnectHandler নিয়ে আসি
from netmiko import ConnectHandler

# ডিভাইসের বিবরণ ডিকশনারিতে রাখি
cisco_switch = {
    'device_type': 'cisco_ios',     # এটা সিসকো IOS ডিভাইস
    'host': '192.168.1.1',          # ডিভাইসের IP ঠিকানা
    'username': 'admin',             # SSH ইউজারনেম
    'password': 'secret',            # SSH পাসওয়ার্ড
    'secret': 'enable_secret'        # এনেবল মোড পাসওয়ার্ড
}

# ডিভাইসে কানেক্ট করি
connection = ConnectHandler(**cisco_switch)
# ** মানে ডিকশনারির সব কী-ভ্যালু পেয়ার আলাদা আলাদা আর্গুমেন্ট হিসেবে পাঠানো হবে
```

এবার কমান্ড চালানোর পার্ট:

```python
# একটা সিঙ্গেল কমান্ড চালাই
output = connection.send_command('show interfaces')
print(output)
# এই কমান্ড সব ইন্টারফেসের স্ট্যাটাস দেখাবে

# কনফিগারেশন মোডে একাধিক কমান্ড চালাই
config_commands = [
    'interface GigabitEthernet0/1',  # কোন ইন্টারফেস কনফিগার করব
    'description Core Link',          # ইন্টারফেসের বর্ণনা
    'no shutdown'                     # ইন্টারফেস চালু করা
]

# কমান্ডগুলো একসাথে পাঠাই
connection.send_config_set(config_commands)
# send_config_set() অটোমেটিক configure terminal মোডে ঢুকে কমান্ড চালাবে

# কাজ শেষে কানেকশন বন্ধ করি
connection.disconnect()
```

## সবগুলো টুল একসাথে ব্যবহার করার উদাহরণ

```python
import requests
from ncclient import manager
from netmiko import ConnectHandler
import json

def get_device_info_rest(device_ip):
    """REST API দিয়ে ডিভাইস ইনফো নেই"""
    response = requests.get(f'http://{device_ip}/api/info')
    return response.json()

def update_netconf_config(device_ip, new_config):
    """NETCONF দিয়ে কনফিগ আপডেট করি"""
    with manager.connect(host=device_ip, ...) as m:
        m.edit_config(target='running', config=new_config)

def backup_via_ssh(device_ip):
    """Netmiko দিয়ে কনফিগ ব্যাকআপ নেই"""
    device = {
        'device_type': 'cisco_ios',
        'host': device_ip,
        'username': 'admin',
        'password': 'secret'
    }
    
    net_connect = ConnectHandler(**device)
    config = net_connect.send_command('show run')
    
    # ফাইলে সেভ করি
    with open(f'backup_{device_ip}.txt', 'w') as f:
        f.write(config)
```

প্রতিটা টুল নিজের নিজের জায়গায় সেরা। REST API দিয়ে সাধারণ তথ্য আদান-প্রদান, NETCONF দিয়ে স্ট্রাকচারড কনফিগারেশন, আর Netmiko দিয়ে CLI কমান্ড অটোমেশন - এভাবে মিলিয়ে ব্যবহার করলে সম্পূর্ণ সমাধান পাওয়া যায়।