### টেমপ্লেটিং ল্যাঙ্গুয়েজ

এই পৃথিবীতে কে ফাঁকিবাজি না করতে পছন্দ করে? তবে এই ফাঁকিবাজি করতে গিয়ে একই কনফিগারেশন অন্য ডিভাইসে যখন কপি পেস্ট করি তখন শুরু হয় ঝামেলা। অনেক সময় আমরা সিঙ্গেল কনফিগার ডিভাইস থেকে কপি পেস্ট করে যখন আরেকটা ডিভাইসে বসাই বা আরো অনেক ডিভাইসে বসাই তখন পুরনো ভিল্যান অথবা কিছু মাস্টার কনফিগ ফাইল থেকে কপি করলে সেটাতে কাজ করার সম্ভাবনা থাকে না। কারণ এটা একটা নতুন এনভায়রনমেন্ট। আর এই ঝামেলার জন্য কিছু টেমপ্লেটিং ল্যাঙ্গুয়েজ ব্যবহার করি নেটওয়ার্কের যাতে যে কোন নতুন এনভায়রনমেন্টে এ ধরনের টেমপ্লেটগুলো ডায়নামিক্যালি অ্যাডাপ্ট করতে পারে। 

নেটওয়ার্কে সফটওয়্যার এর একটা কথা আছে, ড্রাই। ডোন্ট রিপিট ইউরসেলফ। আমরা এমনভাবে কোড অথবা টেমপ্লেট তৈরি করতে চায় যেখানে এই একই জিনিস ব্যবহার করা যাবে ভিন্ন ভিন্ন জায়গায় যার পেছনে থাকবে মডিউলার আর্কিটেকচার। এতে একটা ফাইলকে বারবার রি রাইট না করে বিভিন্ন মডিউল থেকে কনফিগ ফাইল এসে ডাটা ফাইল থেকে ডাটা নিয়ে ভিন্ন ভিন্ন এনভায়রনমেন্টে কনফিগারেশন জেনারেট করে দিবে। এটার জন্য আমরা ব্যবহার করব জিনজা টেমপ্লেট।

নেটওয়ার্ক অটোমেশনের জন্য একটা জিনিস খুবই প্রয়োজন সেটা হচ্ছে কনফিগারেশন কনসিসটেন্সি। আমরা একই জিনিস যখন রিপিটেডলি প্রোডাকশন এনভারমেন্ট এর জন্য ব্যবহার করব তখন সেখানে যাতে মানুষের ভুলের কারণে নেটওয়ার্কের সমস্যা না হয় সেটার জন্যই দরকার যুক্তিসঙ্গত টেমপ্লেটিং ইঞ্জিন। সবচেয়ে বড় কথা হচ্ছে এ ধরনের টেমপ্লেট ব্যবহার করলে নেটওয়ার্ক ইঞ্জিনিয়ারদের অনেক সময় বেঁচে যায়। 

নেটওয়ার্ক কনফিগারেশনের জায়গায় আমরা একই ধরনের আইডিয়া ব্যবহার করতে পারি। বিভিন্ন এনভায়রনমেন্ট এর জন্য আমরা ছোট ছোট বেশ কিছু দিন যা টেমপ্লেট তৈরি করব যাতে যেকোনো একটা কাজ করতে গেলে যেমন ধরা যাক ইন্টারফেস কনফিগারেশন অথবা বিজিপি কনফিগারেশন অথবা ওএসপিএফ কনফিগারেশন তৈরি করতে এ ধরনের টেম্প্লেটিং ইঞ্জিন ব্যবহার করব। ফাইনালি যখন আমরা এ ধরনের আলাদা আলাদা টেমপ্লেটকে একটা জায়গা থেকে কম্বাইন করে একটা পূর্ণ কনফিগারেশন ফাইল তৈরি করতে চাইবো যেটা একটা মাস্টার টেমপ্লেট হবে। এর কাজ হচ্ছে সবগুলো জায়গা থেকে ডাটা পুল করে আনবে ফাইনাল কনফিগারেশনে। 

আমার অভিজ্ঞতায় দেখেছি যে প্রচুর নেটওয়ার্ক এডমিশন প্রজেক্টে এ ধরনের টেমপ্লেট আর মানুষ ব্যবহার করেন না। এই টেমপ্লেট একবার ব্যবহার শুরু করলে এ ধরনের টেমপ্লেট এনসিবল এর মত automation software platform ব্যবহার করতে পারে তাদের কনফিগারেশন চেঞ্জ গুলোকে নেটওয়ার্কের অর্থাৎ লাইভ বা প্রোডাকশন এনভায়রনমেন্টে পুশ করার জন্য।

### জিনজা কি? 

জিনজার আসলে পাইথনের একটা টেমপ্লেটিং ইঞ্জিন। ওয়েবসাইট তৈরি করার সময় ডিজঙ্গ অথবা ফ্লাসকে এটার ব্যবহার প্রচুর। তবে নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য আমরা বিভিন্ন ধরনের নেটওয়ার্ক কনফিগারেশন এই টেমপ্লেটে তৈরি করে থাকি। বিভিন্ন ধরনের আইপি অ্যাড্রেস অথবা বি ল্যান্ড আইডি একটা টেক্সট ডকুমেন্টে বিভিন্ন প্লেস হোল্ডার টেক্সট ব্যবহার করে ভ্যারিয়েবল আইটেম হিসেবে রাখা যায়। সবচেয়ে মজার কথা হচ্ছে এটা একটা সিঙ্গেল কনফিগারেশন টেমপ্লেট ফাইল থেকে বিভিন্ন লিস্ট ধরে ধরে ঘুরিয়ে-ফিরিয়ে আলাদা আলাদা কোড ব্লক তৈরি করা যায় প্রতিটা আইটেম ধরে ধরে। যেমন আমরা যদি ফ্রিল্যান্স তৈরি করতে চাই তাহলে নতুন সুইচে বিভিন্ন ভিলেন তৈরি করতে গেলে আলাদা আলাদা ভাবে কপি পেস্ট করে আসল ভিলেন আইডি ঢোকানো বেশ কষ্টের। সেখানে জিনজা দিয়ে আমরা ফ্রীলান্স নাম্বারের লিস্ট পাস করে দিতে পারি ফাইনাল কনফিগারেশন এর জন্য। 

একটা উদাহরণ দেখি। শুরুতেই জিনজা ইনস্টলেশন। (এরপর থেকে আর ইনস্টলেশনের ব্যাপারটা আর বলবো না)

```python
$ pip install jinja2
```
স্ক্রিপ্টটা দেখুন, বোঝা লাগবে না। শুধু চোখ বুলিয়ে নিন।

```python 

#start by importing the Template function from jinja2
from jinja2 import Template

#Set up your jinja template
jtemplate = Template("vlan {{ vlan }}\n name VLAN{{ vlan }}_ABC_Bank\n")

#Create the list of VLANs you want to generate configuration for
vlans = [10,20,30,40,50,60,70,80,90,100]

#Iterate over the list of vlans and print a useable configuration
for vlan in vlans:
    output = jtemplate.render(vlan=vlan)
    print(output)
```

এই স্ক্রিপ্টটা চালালে এটা একটা কনফিগারেশন ফাইল তৈরি করে দিবে যার মধ্যে ভিল্যান ১০ থেকে ১০০ পর্যন্ত থাকবে।

এর আসল খেলাটা এখানে,

```python

vlans = [10,20,30,40,50,60,70,80,90,100]

for vlan in vlans:
    output = jtemplate.render(vlan=vlan)
    print(output)
```


এটার ফাইনাল আউটপুট কি হতে পারে?

vlan 10
 name VLAN10_ABC_Bank
vlan 20
 name VLAN20_ABC_Bank
vlan 30
 name VLAN30_ABC_Bank
vlan 40
 name VLAN40_ABC_Bank
vlan 50
 name VLAN50_ABC_Bank
vlan 60
 name VLAN60_ABC_Bank
vlan 70
 name VLAN70_ABC_Bank
vlan 80
 name VLAN80_ABC_Bank
vlan 90
 name VLAN90_ABC_Bank
vlan 100
 name VLAN100_ABC_Bank

ডিফিকাল্ট মনে হচ্ছে? ফিরে যাই একদম বেসিক ধারণায়। একটা ইন্ডাস্ট্রি স্ট্যান্ডার্ড সি এল আই সিনট্যাক্স ব্যবহার করে একটা সুইচপোর্ট কনফিগার করতে গেলে আমরা কি দিতাম?

interface GigabitEthernet0/1
    description Server Port
    switchport access vlan 10
    switchport mode access

এখানে আরেকটা বেসিক জিনজার টেমপ্লেট দেখি যেখানে ইন্ডাস্ট্রি স্ট্যান্ডার্ড সি এল আই সিনট্যাক্সকে যেটাকে আমরা টেমপ্লেটে কনভার্ট করব যাতে ভবিষ্যতে আমরা শত শত সুইচপোর্ট কনফিগার করতে পারি আমাদের এনভায়রনমেন্টে।

interface {{ interface_name }}
description {{ interface_description }}
switchport access vlan {{ interface_vlan }}
switchport mode access

এখানে ইন্টারফেস নেইম তার পাশাপাশি ইন্টারফেস ডিসক্রিপশন এবং ইন্টারফেস ভিলেন কে আমরা কার্লি ব্রেস এর মধ্যে রেখেছি যাতে এখানে ডায়নামিক্যালি ডাটা ইন্সার্ট করা যায়। সাধারণ প্রোগ্রামিং কনসেপ্ট এর মত এটা অনেকটাই কমন যে ক্লাস অথবা ডিকশনারি একটা ল্যাঙ্গুয়েজে কিভাবে ব্যবহার করতে পারে যখন একটা টেমপ্লেটকে রেন্ডার করতে পারি পাইথনের ভেতর দিয়ে। এ ধরনের কমন কনফিগারেশনের এর সুবিধা হচ্ছে আমরা এটাতে মাল্টিপল ইনস্ট্যান্ট এর ডাটা কে লুক করাতে পারবো যাতে সে এটাকে মাল্টিপল টাইম রাইট করবে যাতে আমাদের ফাইনাল কনফিগারেশন পাওয়া যায়। 

### পাইথনে জিনজা টেম্পলেট ফাইলকে রেন্ডার 

```python
from jinja2 import Environment, FileSystemLoader
ENV = Environment(loader=FileSystemLoader('.'))
template = ENV.get_template("template.j2")
```

প্রথমেই ইমপোর্ট করে নেই দরকারি অবজেক্ট যেটা আসলে লাগবে আমাদের টেমপ্লেটগুলোকে রেন্ডার করার জন্য। দ্বিতীয় লাইনের মানে হচ্ছে আমরা এখানে একটা এনভায়রনমেন্ট অবজেক্ট সেটাপ করে নিলাম যেখানে একটা সিঙ্গেল ডট ব্যবহার করছি যাতে সে বুঝতে পারে যে টেমপ্লেট ফাইলগুলো রাখা আছে একই ডাইরেক্টরি তে যেই ডাইরেক্টরিতে পাইথন ইন্টারপ্রেটর কাজ করছে। তৃতীয় লাইনটার মানে হচ্ছে আমরা যেই টেমপ্লেটটা জেনারেট করছি তা একটা টেমপ্লেট অবজেক্ট ব্যবহার করে, বিশেষ করে স্ট্যাটিকভাবে স্পেসিফাই করা আছে যার নাম হচ্ছে টেমপ্লেট.জে২।

এখন আমাদেরকে ডাটা দিয়ে রেন্ডার করে দেখতে হবে। কনফিগারেশন তৈরি করতে হবে না? আমরা যেহেতু কাজটা করে ফেলেছি এখন এই জায়গাটাতে আমাদের ডাটা ঢোকাতে হবে। আমরা চেষ্টা করব আমাদের ডাটাকে আলাদা ভাবে রাখার জন্য যাতে পাইথন স্ক্রিপ্ট এর মধ্যে ডাটা না থাকে। এটা একটা ইন্ডাস্ট্রি বেস্ট প্রাকটিস যে পাইথন স্ক্রিপ্ট এর মধ্যে কনফিগারেশন টেমপ্লেট থাকলেও ডাটা আসবে বাইরে থেকে।

```python
interface_dict = {
"name": "GigabitEthernet0/1",
"description": "Server Port",
"vlan": 10,
"uplink": False
}
```
আমাদের কাছে যেহেতু এখন সবকিছুই এসেছে, এখন আমাদেরকে টেমপ্লেট ধরে রেন্ডার করতে হবে। এই টেন্ডারিং এর জন্য আমরা রেন্ডার ফাংশন কে কল করব আমাদের টেমপ্লেটের অবজেক্টে যাতে আমরা এর ভেতরে ডাটা পাস করাতে পারি টেমপ্লেট ইঞ্জিনের মধ্যে। এবং ফাইনালি প্রিন্ট ফাংশন ব্যবহার করে সেটার আউটপুট দেখব আমাদের স্ক্রিনে। আমাদের কাজ হচ্ছে টেমপ্লেটটা ঠিকমতো কাজ করছে কিনা সেটা নিশ্চিত করা।

```python
print(template.render(interface=interface_dict))
```
সেটার আউটপুট এরকম হতে পারে।
```python
interface GigabitEthernet0/1
description Server Port
switchport access vlan 10
switchport mode access
```
আমরা একটা পাইথন স্ক্রিপ্ট দেখছি এখানে যার মধ্যে নেটওয়ার্ক ইন্টারফেস একটা অবজেক্ট ক্লাস তৈরি করা হয়েছে। এর পাশাপাশি আমরা সেই ডাটা ইন্টারফেস অবজেক্টটাকে আমরা টেমপ্লেটের ভেতরে ঢুকিয়ে রেন্ডার করার চেষ্টা করেছি।  এখানে ডাটা অংশটিকে স্ক্রিপ্টের মধ্যে ঢোকানো ঠিক না,  তবে উদাহরণ দেখানোর জন্য এখানে আমরা ঢুকিয়েছি। ফাইনালি সেটাকে প্রিন্ট করে আমরা দেখব তার আউটপুট কনফিগারেশন ফাইল দেখায় কিনা।

```python
from jinja2 import Environment, FileSystemLoader
ENV = Environment(loader=FileSystemLoader('.'))
template = ENV.get_template("template.j2")
class NetworkInterface(object):
    def __init__(self, name, description, vlan, uplink=False):
    self.name = name
    self.description = description
    self.vlan = vlan
    self.uplink = uplink
data_interface_obj = NetworkInterface("GigabitEthernet0/1", "Server Port", 10)
print(template.render(interface=data_interface_obj))
```
### কন্ডিশনাল লজিক ব্যবহার করে সুইচপোর্ট কনফিগারেশন

আমাদের আগের উদাহরণ টাকে এখানে নিয়ে আসি। তবে এই নতুন উদাহরণের মধ্যে আমরা একটা সিদ্ধান্তের লুপ ঢুকাবো যে কিভাবে কোন পরিস্থিতিতে কখন একটা কন্ডিশনের উপরে টেমপ্লেট ফাইলটা নিজের মতো করে জেনারেট করবে।  এখানে আপনারা বুঝতেই পারছেন যে এ ধরনের কার্লিব্রেসগুলো হচ্ছে একটা স্পেশাল জিনজা ট্যাগ, যার মাধ্যমে বোঝা যায় যে এখানে কিছু কন্ডিশন অর্থাৎ লজিক ব্যবহার হবে। এই জায়গায় আমরা একটা জিনজা এর লুপ ব্যবহার করছি যাতে অনেকগুলো সুইচপোর্টের জন্য আমরা একটা ফাইনাল কনফিগারেশন তৈরি করতে পারি। 

```python
interface {{ interface.name }}
    description {{ interface.description }}
{% if interface.uplink %}
    switchport mode trunk
{% else %}
    switchport access vlan {{ interface.vlan }}
    switchport mode access
{% endif %}
```
এটা একটা বেসিক টেমপ্লেট, তবে সামনেই এটাকে আমরা দেখাবো কিভাবে সব মিলিয়ে আসল কনফিগারেশন বের করা যায়। এই উদাহরণে আমরা একই ধরনের কনফিগারেশন দশটা সুইচপোর্ট এর জন্য তৈরি করছি, তবে এটা কেমন হয় যদি আমরা কিছু কিছু সুইচপোর্ট এর জন্য আলাদা কনফিগারেশন তৈরি করতে চাই?

```python
{% for n in range(10) %}
    interface GigabitEthernet0/{{ n+1 }}
        description {{ interface.description }}
        switchport access vlan {{ interface.vlan }}
        switchport mode access
{% endfor %}
```
### ফর লুপ ব্যবহার করে ভেরিয়েবল দিয়ে কনফিগারেশন জেনারেশন 


```python
{% for n in range(10) %}
interface GigabitEthernet0/{{ n+1 }}
description {{ interface.description }}
{% if n+1 == 1 %}
switchport mode trunk
{% else %}
switchport access vlan {{ interface.vlan }}
switchport mode access
{% endif %}
{% endfor %}
```

```python
intlist = [
"GigabitEthernet0/1",
"GigabitEthernet0/2",
"GigabitEthernet0/3"
]
print(template.render(interface_list=intlist))
```

```python
{% for iface in interface_list %}
interface {{ iface }}
{% if iface == "GigabitEthernet0/1" %}
switchport mode trunk
{% else %}
switchport access vlan 10
switchport mode access
{% endif %}
{% endfor %}
```

```python
intdict = {
"GigabitEthernet0/1": "Server port number one",
"GigabitEthernet0/2": "Server port number two",
"GigabitEthernet0/3": "Server port number three"
}
print(template.render(interface_dict=intdict))
```

```python
{% for name, desc in interface_dict.items() %}
interface {{ name }}
description {{ desc }}
{% endfor %}
```


```python
interfaces = [
{
"name": "GigabitEthernet0/1",
"desc": "uplink port",
"uplink": True
},
{
"name": "GigabitEthernet0/2",
"desc": "Server port number one",
"vlan": 10
},
{
"name": "GigabitEthernet0/3",
"desc": "Server port number two",
"vlan": 10
}
]
print(template.render(interface_list=interfaces)
)
```


```python
{% for interface in interface_list %}
interface {{ interface.name }}
description {{ interface.desc }}
{% if interface.uplink %}
switchport mode trunk
{% else %}
switchport access vlan {{ interface.vlan }}
switchport mode access
{% endif %}
{% endfor %}
```

```python
---
-   name: GigabitEthernet0/1
    desc: uplink port
    uplink: true
-   name: GigabitEthernet0/2
    desc: Server port number one
    vlan: 10
-   name: GigabitEthernet0/3
    desc: Server port number two
    vlan: 10
```

```python
from jinja2 import Environment, FileSystemLoader
import yaml
ENV = Environment(loader=FileSystemLoader('.'))
template = ENV.get_template("template.j2")
    with open("data.yml") as f:
interfaces = yaml.load(f, Loader=yaml.SafeLoader)
print(template.render(interface_list=interfaces))
```