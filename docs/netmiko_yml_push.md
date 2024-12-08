আপনার এক বন্ধু আছে উজ্জল। সে একটা বড় টেলিকম কোম্পানিতে নেটওয়ার্ক স্পেশালিস্ট। একদিন অফিসে যাবার পর তার বস বলল, "উজ্জল, আমাদের কাস্টমারের ৫০০টা রাউটারে নতুন কনফিগারেশন দিতে হবে। কাল সকালের মধ্যে চাই।"

উজ্জল প্রথমে একটু ঘাবড়ে গেল। মনে মনে হিসাব করল - প্রতি রাউটারে ১০ মিনিট, ৫০০টা রাউটার, মোট লাগবে ৮৩ ঘণ্টা! কিন্তু উজ্জল হাল ছাড়ল না। আসলে সে জানে এই কাজটা অটোমেট করে ১৫ মিনিটেই শেষ করা যায়।

### টেমপ্লেট ব্যবহার করে লুপব্যাক ইন্টারফেস যোগ

এই কাজে আমরা YAML, Jinja2 এবং Netmiko একসাথে ব্যবহার করে লুপব্যাক ইন্টারফেস কনফিগার করব। এটা পারলেই সব কিছু পুশ করতে পারবেন। খালি কনফিগারেশন পাল্টে দেবেন।

### প্রজেক্ট স্ট্রাকচার 

এবার আমাদের ফাইলগুলো সুন্দর করে সাজিয়ে রাখি;
```
network_automation/
│
├── configs/               # কনফিগারেশন ফাইল
│   ├── routers.yml
│   └── interfaces.yml
│
├── templates/            # টেমপ্লেট ফাইল
│   └── loopback.j2
│
├── scripts/             # পাইথন স্ক্রিপ্ট
│   └── deploy.py
│
└── logs/               # লগ ফাইল (পৃষ্টার লিমিটেশনের জন্য দেখাইনি)
    └── automation.log
```    
## প্রথম ধাপ: প্রস্তুতি

প্রথমে আমরা পাইথনে প্রয়োজনীয় টুল ইনস্টল করব:

```bash
# প্রথমে পাইথন ভার্চুয়াল এনভায়রনমেন্ট তৈরি করি 
python -m venv network_env
source network_env/bin/activate

# এরপর প্রয়োজনীয় লাইব্রেরি ইনস্টল করি
pip install netmiko pyyaml jinja2
```

প্রজেক্ট স্ট্রাকচার থেকে আমাদের তিনটা জিনিস লাগবে:

১. **templates/loopback.j2** - এখানে রাউটার কমান্ড লিখব
২. **scripts/deploy.py** - এখানে মূল স্ক্রিপ্ট থাকবে
৩. **configs/** - এই ফোল্ডারটা রাখব ভবিষ্যতের জন্য

প্রথমে আমরা `loopback.j2` টেমপ্লেটে রাউটার কমান্ড হিসেবে লিখি:

```jinja2
interface Loopback{{ interface.number }}
 description {{ interface.description }}
 ip address {{ interface.ip }} {{ interface.mask }}
 no shutdown
```

এরপর আসি মূল স্ক্রিপ্টে। আমি `deploy.py` স্ক্রিপ্টটাকে ছোট ছোট অংশে ভেঙে বুঝাই:

### ১. লাইব্রেরি ইম্পোর্ট
```python
# আমাদের দরকারি লাইব্রেরি
import yaml                                    # ডাটা প্রসেসিংয়ের জন্য
from jinja2 import Environment, FileSystemLoader    # টেমপ্লেট ইঞ্জিনের জন্য
from netmiko import ConnectHandler            # রাউটার কানেকশনের জন্য
import ipaddress                              # আইপি এড্রেস হ্যান্ডলিংয়ের জন্য
```

### ২. রাউটার কনফিগ জেনারেটর
```python
def generate_routers_config(start_ip, total_routers):
    """
    রাউটার কনফিগারেশন জেনারেট করে
    
    Args:
        start_ip: শুরুর আইপি (যেমন: '192.168.1.0')
        total_routers: কতগুলো রাউটার (যেমন: 500)
    """
    
    # নেটওয়ার্ক থেকে আইপি লিস্ট বের করি
    network = ipaddress.IPv4Network(f"{start_ip}/24", strict=False)
    ip_list = list(network.hosts())[:total_routers]
    
    # রাউটার কনফিগ ডিকশনারি
    routers = {
        'routers': []
    }
    
    # প্রতিটা রাউটারের জন্য কনফিগ জেনারেট করি
    for i, ip in enumerate(ip_list, 1):
        router = {
            'name': f'router_{i}',      # যেমন: router_1, router_2
            'ip': str(ip),              # যেমন: 192.168.1.1, 192.168.1.2
            'username': 'admin',
            'password': 'secure123',
            'type': 'cisco_ios'
        }
        routers['routers'].append(router)
        
    return routers
```

### ৩. ইন্টারফেস কনফিগ জেনারেটর
```python
def generate_interface_config():
    """লুপব্যাক ইন্টারফেসের কনফিগারেশন জেনারেট করে"""
    
    return {
        'interfaces': {
            'loopback': {
                'number': 0,
                'description': 'Management Interface',
                'ip': '10.0.0.1',
                'mask': '255.255.255.0'
            }
        }
    }
```

### ৪. মেইন ফাংশন
```python
def main():
    # ১. কনফিগারেশন জেনারেট
    routers = generate_routers_config('192.168.1.0', 500)
    interfaces = generate_interface_config()
    
    # ২. জিনজা টেমপ্লেট সেটআপ
    env = Environment(loader=FileSystemLoader('templates'))
    template = env.get_template('loopback.j2')
    
    # ৩. কনফিগ জেনারেট
    config = template.render(interface=interfaces['interfaces']['loopback'])
    
    # ৪. প্রতিটা রাউটারে কনফিগ পাঠাই
    for router in routers['routers']:
        print(f"\n{router['name']} ({router['ip']}) এ কাজ শুরু...")
        try:
            # রাউটারে কানেক্ট
            connection = ConnectHandler(**router)
            
            # কনফিগ পাঠাই
            output = connection.send_config_set(config.split('\n'))
            print(f"{router['name']} এ কাজ সফল!")
            
            # কানেকশন বন্ধ
            connection.disconnect()
        except Exception as e:
            print(f"সমস্যা হয়েছে {router['name']} এ: {str(e)}")

if __name__ == "__main__":
    main()
```

এখন বুঝি কীভাবে স্ক্রিপ্টটা কাজ করে:

১. প্রথমে এটা `generate_routers_config()` ফাংশন কল করে। এই ফাংশন:
   - `192.168.1.0` নেটওয়ার্ক থেকে ৫০০টা আইপি জেনারেট করে
   - প্রতিটা আইপির জন্য রাউটার কনফিগ বানায়
   
২. তারপর `generate_interface_config()` ফাংশন কল করে। এটা:
   - লুপব্যাক ইন্টারফেসের কনফিগ জেনারেট করে
   - যেমন - আইপি, মাস্ক, ডেসক্রিপশন ইত্যাদি

৩. এরপর টেমপ্লেট ইঞ্জিন সেটআপ করে:
   - `loopback.j2` ফাইল থেকে টেমপ্লেট লোড করে
   - টেমপ্লেটে ভ্যারিয়েবল বসিয়ে কনফিগ জেনারেট করে

৪. সবশেষে প্রতিটা রাউটারের জন্য:
   - SSH দিয়ে রাউটারে কানেক্ট করে
   - কনফিগারেশন পাঠায়
   - কানেকশন বন্ধ করে দেয়

এভাবে উজ্জল ভাই একটা স্মার্ট সলিউশন বের করলেন। তিনি বুঝলেন যে, ৫০০টা রাউটারের কনফিগারেশন হাতে করা বোকামি হবে। তার বদলে পাইথনের শক্তি ব্যবহার করে অটোমেশন করে ফেললেন।

!!! tips "ইচ্ছেকৃত ভুল"
    কাজটা করতে গিয়ে ছোট একটা ভুল করে রেখেছি, কাজ করলেই বুঝতে পারবেন। তবে, সেটা গিটহাবে দেয়া আছে।

```python
def main():
    # ১. প্রথমে রান্নার সব উপকরণ (ingredients) রেডি করি
    routers = generate_routers_config('192.168.1.0', 500)
    interfaces = generate_interface_config()
    """
    এখানে কী হচ্ছে:
    - generate_routers_config() ফাংশন থেকে আমরা ৫০০টা রাউটারের ইনফরমেশন পাচ্ছি
    - প্রতিটা রাউটারের জন্য নাম, আইপি, ইউজারনেম, পাসওয়ার্ড আছে
    - interfaces থেকে আমরা পাচ্ছি লুপব্যাক কনফিগ - আইপি, মাস্ক, ডেসক্রিপশন
    """
    
    # ২. রান্নার রেসিপি (টেমপ্লেট) রেডি করি
    env = Environment(loader=FileSystemLoader('templates'))
    template = env.get_template('loopback.j2')
    """
    এখানে কী হচ্ছে:
    - templates ফোল্ডার থেকে loopback.j2 ফাইল লোড করছি
    - এই ফাইলে সিসকো রাউটারের কমান্ড লেখা আছে
    - Environment সেটআপ করছি যেখানে টেমপ্লেট ফাইল আছে
    """
    
    # ৩. রেসিপি অনুযায়ী উপকরণ মিক্স করে রান্না রেডি করি
    config = template.render(interface=interfaces['interfaces']['loopback'])
    """
    এখানে কী হচ্ছে:
    - টেমপ্লেটে ভ্যারিয়েবল বসিয়ে আসল কমান্ড বানাচ্ছি
    - যেমন {{ interface.number }} এর জায়গায় 0 বসবে
    - {{ interface.ip }} এর জায়গায় 10.0.0.1 বসবে
    """
    
    # ৪. এবার রান্না (কনফিগ) প্রতিটা প্লেটে (রাউটারে) সার্ভ করি
    for router in routers['routers']:
        print(f"\n{router['name']} ({router['ip']}) এ কাজ শুরু...")
        try:
            # রাউটারে কানেক্ট - যেন প্লেট রেডি করা
            connection = ConnectHandler(**router)
            """
            এখানে কী হচ্ছে:
            - ConnectHandler রাউটারে SSH কানেকশন তৈরি করে
            - **router দিয়ে সব প্যারামিটার পাস করছি
            - এতে username, password, ip, device_type সব যাচ্ছে
            """
            
            # কনফিগ পাঠাই - যেন খাবার প্লেটে সাজানো
            output = connection.send_config_set(config.split('\n'))
            """
            এখানে কী হচ্ছে:
            - config.split('\n') দিয়ে কমান্ডগুলো আলাদা করছি
            - send_config_set দিয়ে কমান্ড পাঠাচ্ছি
            - রাউটার কনফিগারেশন মোডে ঢুকে কমান্ড রান করবে
            """
            print(f"{router['name']} এ কাজ সফল!")
            
            # কানেকশন বন্ধ - যেন প্লেট ডেলিভারি শেষ
            connection.disconnect()
            
        except Exception as e:
            # কোন সমস্যা হলে এরর মেসেজ দেখাই
            print(f"সমস্যা হয়েছে {router['name']} এ: {str(e)}")

# স্ক্রিপ্ট সরাসরি রান করলে main() কল করি
if __name__ == "__main__":
    main()
```
