আপনার এক বন্ধু আছে উজ্জল। সে একটা বড় টেলিকম কোম্পানিতে নেটওয়ার্ক স্পেশালিস্ট। একদিন অফিসে যাবার পর তার বস বলল, "উজ্জল, আমাদের কাস্টমারের ৫০০টা রাউটারে নতুন কনফিগারেশন দিতে হবে। কাল সকালের মধ্যে চাই।"

উজ্জল প্রথমে একটু ঘাবড়ে গেল। মনে মনে হিসাব করল - প্রতি রাউটারে ১০ মিনিট, ৫০০টা রাউটার, মোট লাগবে ৮৩ ঘণ্টা! কিন্তু উজ্জল হাল ছাড়ল না। আসলে সে জানে এই কাজটা অটোমেট করে ১৫ মিনিটেই শেষ করা যায়।

### টেমপ্লেট ব্যবহার করে লুপব্যাক ইন্টারফেস যোগ

এই কাজে আমরা YAML, Jinja2 এবং Netmiko একসাথে ব্যবহার করে লুপব্যাক ইন্টারফেস কনফিগার করব। এটা পারলেই সব কিছু পুশ করতে পারবেন। খালি কনফিগারেশন পাল্টে দেবেন।

### প্রজেক্ট স্ট্রাকচার 

এবার আমাদের ফাইলগুলোকে স্ট্রাকচারাল লেভেলে সুন্দর করে সাজিয়ে রাখি;
```
network_automation/
│
├── configs/               # কনফিগারেশন ফাইল - এই মুহুর্তে লাগছে না
│   ├── routers.yml        # এটা লাগতো যদি আমাদের কাছে এই ডাটাটা
│   └── interfaces.yml     # আগে থেকে থাকতো, তবে ভবিষ্যতে লাগবে
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

### ১. প্রথমে লাইব্রেরি ইম্পোর্ট:
```python
# নেটওয়ার্ক অটোমেশনের জন্য প্রয়োজনীয় লাইব্রেরি
import yaml                                    # কনফিগারেশন ফাইল হ্যান্ডলিং
from jinja2 import Environment, FileSystemLoader    # টেমপ্লেট ইঞ্জিন
from netmiko import ConnectHandler            # রাউটার কানেকশন
import ipaddress                              # আইপি এড্রেস ক্যালকুলেশন
```

### ২. লুপব্যাক ইন্টারফেস কনফিগারেশন জেনারেটর:
```python
def generate_interface_config(router_number):
    """
    প্রতিটা রাউটারের জন্য আলাদা লুপব্যাক আইপি জেনারেট করে
    
    কীভাবে কাজ করে:
    - রাউটার ১-৫০০ পর্যন্ত নাম্বার নেয়
    - প্রতিটার জন্য আলাদা আইপি দেয়:
      router_1   -> 10.0.0.1
      router_2   -> 10.0.0.2
      router_256 -> 10.0.1.0
      router_500 -> 10.0.1.244
    
    Args:
        router_number (int): রাউটার নাম্বার (1-500)
    """
    # আইপি এর তৃতীয় এবং চতুর্থ অক্টেট ক্যালকুলেট করি
    third_octet = (router_number - 1) // 256   # প্রতি 256 রাউটারে এটা বাড়ে
    fourth_octet = (router_number - 1) % 256 + 1   # 1-256 পর্যন্ত ঘুরে ফিরে আসে
    
    # রাউটারের লুপব্যাক কনফিগারেশন ডিকশনারি
    return {
        'interfaces': {
            'loopback': {
                'number': 0,                    # লুপব্যাক নাম্বার সবার 0
                'description': f'Management Interface Router {router_number}',
                'ip': f'10.0.{third_octet}.{fourth_octet}',
                'mask': '255.255.255.255'      # /32 মাস্ক ব্যবহার করছি
            }
        }
    }
```

### ৩. রাউটার কনফিগারেশন জেনারেটর:
```python
def generate_routers_config(start_ip, total_routers):
    """
    সব রাউটারের মূল কনফিগারেশন জেনারেট করে
    
    কীভাবে কাজ করে:
    - 192.168.0.0/23 নেটওয়ার্ক থেকে আইপি নেয়
    - প্রতি রাউটারের জন্য:
      * ইউনিক নাম (router_1, router_2, ...)
      * ইউনিক আইপি (192.168.0.1, 192.168.0.2, ...)
      * লগইন ক্রেডেনশিয়াল
    
    Args:
        start_ip (str): বেস আইপি যেমন '192.168.0.0'
        total_routers (int): মোট কত রাউটার লাগবে
    """
    # /23 সাবনেট মানে 512টা আইপি - 500 রাউটারের জন্য যথেষ্ট
    network = ipaddress.IPv4Network(f"{start_ip}/23", strict=False)
    ip_list = list(network.hosts())[:total_routers]
    
    # রাউটার লিস্ট তৈরি
    routers = {
        'routers': []
    }
    
    # প্রতিটা রাউটারের জন্য কনফিগ তৈরি
    for i, ip in enumerate(ip_list, 1):
        router = {
            'name': f'router_{i}',      # ইউনিক নাম
            'ip': str(ip),              # ইউনিক আইপি
            'username': 'admin',         # লগইন ইউজারনেম
            'password': 'secure123',     # লগইন পাসওয়ার্ড
            'type': 'cisco_ios'         # রাউটার টাইপ
        }
        routers['routers'].append(router)
    
    return routers
```

### ৪. মেইন প্রোগ্রাম:
```python
def main():
    """
    মূল প্রোগ্রাম - তিনটা ধাপে কাজ করে:
    
    ১. প্রথম ধাপ: কনফিগারেশন জেনারেট
       - ৫০০টা রাউটারের ম্যানেজমেন্ট আইপি (192.168.0.0/23 থেকে)
       
    ২. দ্বিতীয় ধাপ: টেমপ্লেট প্রস্তুত
       - templates/loopback.j2 ফাইল থেকে টেমপ্লেট লোড
       
    ৩. তৃতীয় ধাপ: কনফিগারেশন পাঠানো
       - প্রতি রাউটারে SSH কানেকশন
       - ইউনিক লুপব্যাক কনফিগ (10.0.x.y রেঞ্জ থেকে)
       - কানেকশন বন্ধ
    """
    # ১. রাউটার কনফিগ জেনারেট
    routers = generate_routers_config('192.168.0.0', 500)
    
    # ২. জিনজা টেমপ্লেট সেটআপ
    env = Environment(loader=FileSystemLoader('templates'))
    template = env.get_template('loopback.j2')
    
    # ৩. প্রতিটা রাউটারে কাজ করি
    for i, router in enumerate(routers['routers'], 1):
        # এই রাউটারের জন্য ইউনিক লুপব্যাক আইপি নিই
        interfaces = generate_interface_config(i)
        
        # টেমপ্লেট থেকে কনফিগ কমান্ড বানাই
        config = template.render(interface=interfaces['interfaces']['loopback'])
        
        print(f"\n{router['name']} ({router['ip']}) এ কাজ শুরু...")
        try:
            # রাউটারে SSH কানেকশন করি
            connection = ConnectHandler(**router)
            
            # কনফিগারেশন কমান্ড পাঠাই
            output = connection.send_config_set(config.split('\n'))
            print(f"{router['name']} এ কাজ সফল!")
            
            # কানেকশন বন্ধ করি
            connection.disconnect()
        except Exception as e:
            print(f"সমস্যা হয়েছে {router['name']} এ: {str(e)}")

# যখন স্ক্রিপ্ট সরাসরি রান করা হবে (এটা নিয়ে আলাপ করা হয়েছে)
if __name__ == "__main__":
    main()
```

এভাবে উজ্জল একটা স্মার্ট সলিউশন বের করলো। সে বুঝলো যে, ৫০০টা রাউটারের কনফিগারেশন হাতে করা বোকামি হবে। তার বদলে পাইথনের শক্তি ব্যবহার করে অটোমেশন করে ফেললো।

!!! tips "ইচ্ছেকৃত ভুল"
    কাজটা করতে গিয়ে ছোট একটা ভুল করে রেখেছি, কাজ করলেই বুঝতে পারবেন। তবে, সেটা গিটহাবে দেয়া আছে।