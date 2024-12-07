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

## দ্বিতীয় ধাপ: ডাটা সাজানো 

এবার আমাদের রাউটারের ডাটা সাজিয়ে রাখব একটা ফাইলে (routers.yml):

```yaml
routers:
  - name: dhaka_router_1
    ip: 192.168.1.1
    username: admin
    password: secure123
    type: cisco_ios
```

আর নতুন কনফিগারেশন লিখব আরেক ফাইলে (interfaces.yml):

```yaml
interfaces:
  loopback:
    number: 0
    description: Management Interface
    ip: 10.0.0.1
    mask: 255.255.255.0
```

## তৃতীয় ধাপ: টেমপ্লেট তৈরি

এবার কমান্ডের জিনজা টেম্পলেট বানাব (template.j2):

```jinja2
interface Loopback{{ interface.number }}
 description {{ interface.description }}
 ip address {{ interface.ip }} {{ interface.mask }}
 no shutdown
```

## চতুর্থ ধাপ: স্ক্রিপ্ট লেখা

এবার মূল স্ক্রিপ্ট (deploy.py):

```python
import yaml
from jinja2 import Environment, FileSystemLoader
from netmiko import ConnectHandler

def main():
    # রাউটার এবং কনফিগ ডাটা লোড করি
    with open('configs/routers.yml') as f:
        routers = yaml.safe_load(f)
    with open('configS/interfaces.yml') as f:
        configs = yaml.safe_load(f)
    
    # টেমপ্লেট থেকে কমান্ড তৈরি করি
    env = Environment(loader=FileSystemLoader('templates'))
    template = env.get_template('template.j2')
    config = template.render(interface=configs['interfaces']['loopback'])
    
    # প্রতি রাউটারে কনফিগ পাঠাই
    for router in routers['routers']:
        print(f"\n{router['name']} এ কাজ শুরু...")
        connection = ConnectHandler(**router)
        output = connection.send_config_set(config.split('\n'))
        print(f"{router['name']} এ কাজ শেষ!")
        connection.disconnect()

if __name__ == "__main__":
    main()
```

## শেষ ধাপ: স্ক্রিপ্ট চালানো

এবার শুধু কমান্ড দিলেই কাজ শেষ:

```bash
python deploy.py
```

এভাবে উজ্জল ৮৩ ঘণ্টার কাজ ১৫ মিনিটে শেষ করল। কিন্তু এর আগে সে কিছু সতর্কতা অবলম্বন করল:

১. টেস্টিং এনভায়রনমেন্টে প্রথমে পরীক্ষা করল
২. সব রাউটারের ব্যাকআপ নিয়ে রাখল
৩. যে কমান্ড পাঠাবে তা আগে ভাল করে চেক করল

এভাবে সে নিশ্চিত হল যে কোন ভুল হবে না। আপনিও চাইলে এই পদ্ধতি ব্যবহার করে আপনার কাজ সহজ করে ফেলতে পারেন!

!!! tips "ইচ্ছেকৃত ভুল"
    কাজটা করতে গিয়ে ছোট একটা ভুল করে রেখেছি, কাজ করলেই বুঝতে পারবেন। তবে, সেটা গিটহাবে দেয়া আছে।