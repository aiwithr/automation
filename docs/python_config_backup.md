### পাইথন দিয়ে নেটওয়ার্কের কনফিগারেশন ব্যাকআপ

আমাদের এই স্ক্রিপ্ট শুরু হচ্ছে দুটা মডিউলকে ইমপোর্ট করে। একটা হচ্ছে নেটমিকো, যার কাজ হচ্ছে এসএসএইচ কানেকশন তৈরি করে দেওয়া, এবং অন্যটা হচ্ছে গেটপাস, যা পাসওয়ার্ড প্রম্পটকে নিরাপদ ভাবে ইনপুট নেওয়ার জন্য ব্যবহৃত হবে।

```python
# প্রথমে লাগবে আমাদের দুটো লাইব্রেরি
from netmiko import ConnectHandler
import getpass

# এবার পাসওয়ার্ড নিই - সিকিওর ভাবে!
password = getpass.getpass('বন্ধু, পাসওয়ার্ডটা দিয়ে দাও: ')

# কোথায় ব্যাকআপ রাখবো সেটা বলে দিই
backup_folder = 'C:/network_backups'  # উইন্ডোজের জন্য
# backup_folder = '/home/user/network_backups'  # লিনাক্সের জন্য

# এই যে আমাদের ডিভাইসের লিস্ট
switch_list = [
    '192.168.1.10',     # কোর সুইচ
    '192.168.1.20',     # ডিস্ট্রিবিউশন সুইচ
    '192.168.1.30'      # এক্সেস সুইচ
]
```

### উপরের কোডটা কি করলো? দেখি:

    1. netmiko আর getpass নামে দুটো লাইব্রেরি নিয়ে এলাম
    2. পাসওয়ার্ড চাইলাম - তবে সিকিওর ভাবে, যাতে স্ক্রিনে না দেখা যায়
    3. ব্যাকআপ ফোল্ডারের লোকেশন ঠিক করলাম
    4. আমাদের টার্গেট সুইচগুলোর আইপি লিস্ট বানালাম

## এবার আসল কাজে নামি!

```python
# প্রতিটা সুইচের জন্য ডিভাইস ইনফো বানাই
device_list = []

for switch_ip in switch_list:
    device = {
        "device_type": "cisco_ios",      # ধরে নিলাম সিসকো সুইচ
        "host": switch_ip,               # সুইচের আইপি
        "username": "admin",             # ইউজারনেম
        "password": password,            # লগইন পাসওয়ার্ড
        "secret": password               # এনেবল পাসওয়ার্ড
    }
    device_list.append(device)

print("হুররে! সব ডিভাইসের ইনফো রেডি!")
```

### এই পার্টে কি হলো?

    - প্রতিটা সুইচের জন্য একটা ডিকশনারি বানালাম
    - সেখানে সুইচে লগইন করার সব ডাটা রাখলাম
    - সব ডিকশনারি একটা লিস্টে জমা করলাম

## এখন আসল ম্যাজিক!

```python
# একে একে সব সুইচে ঢুকে কাজ করি
for device in device_list:
    current_switch = device['host']
    print(f"\n{current_switch} এ কানেক্ট করার চেষ্টা করছি...")
    
    try:
        # সুইচে কানেক্ট করি
        connection = ConnectHandler(**device)
        print(f"{current_switch} এ কানেক্ট করা সফল!")
        
        # কনফিগ নিয়ে আসি
        print("কনফিগারেশন পড়ছি...")
        config = connection.send_command('show running-config')
        
        # ফাইলে সেভ করি
        backup_file = f"{backup_folder}/backup_{current_switch}.txt"
        with open(backup_file, 'w') as f:
            f.write(config)
        print(f"{current_switch} এর কনফিগ সেভ করা হয়েছে!")
        
        # কানেকশন বন্ধ করি
        connection.disconnect()
        print(f"{current_switch} থেকে ডিসকানেক্ট করা হয়েছে।")
        
    except Exception as e:
        print(f"আরে দুঃখিত! {current_switch} এ একটা সমস্যা হয়েছে: {str(e)}")

print("\nসব কাজ শেষ! সবার ব্যাকআপ নেওয়া হয়ে গেছে!")
```

#### এখানে কি কি করলাম?

    1. লিস্টের প্রতিটা ডিভাইসে একে একে কানেক্ট করলাম
    2. প্রতিটা স্টেপে কি হচ্ছে তা প্রিন্ট করে দেখালাম
    3. কনফিগারেশন নিয়ে এসে ফাইলে সেভ করলাম
    4. কোন সমস্যা হলে সেটা ধরে ফেললাম এবং মেসেজ দেখালাম

### প্রতিটা অংশ বুঝে নেই

### ১. লাইব্রেরি ইমপোর্ট
প্রথম দুই লাইনে আমরা যে টুলস লাগবে সেগুলো নিয়ে আসছি। `netmiko` দিয়ে সুইচে কানেক্ট করব, আর `getpass` দিয়ে পাসওয়ার্ড নিরাপদে নিব।

### ২. পাসওয়ার্ড ইনপুট
`getpass.getpass()` ফাংশন ব্যবহার করে পাসওয়ার্ড নিচ্ছি। এটা পাসওয়ার্ড টাইপ করার সময় স্ক্রিনে কিছু দেখায় না, যা সিকিউরিটির জন্য ভালো।

### ৩. ব্যাকআপের জায়গা
`backup_folder` ভেরিয়েবলে আমরা বলে দিচ্ছি কোথায় ব্যাকআপ ফাইলগুলো রাখা হবে। উইন্ডোজ এবং লিনাক্সের জন্য পাথ আলাদা হয়।

### ৪. সুইচের তালিকা
`switch_list` লিস্টে আমরা সব সুইচের আইপি রাখছি। কমেন্ট দিয়ে প্রতিটা সুইচের কাজ লিখে রাখছি।

### ৫. সুইচের লগইন ডাটা
প্রতিটা সুইচের জন্য একটা ডিকশনারি বানাচ্ছি যেখানে সব লগইন ইনফরমেশন থাকবে। এই ডিকশনারিগুলো `device_list` এ জমা করছি।

### ৬. প্রধান লুপ; এই অংশে আমরা প্রতিটা সুইচের জন্য:

    - SSH দিয়ে কানেক্ট করছি
    - running-config দেখছি
    - কনফিগ ফাইলে সেভ করছি
    - কানেকশন বন্ধ করছি

### ৭. এরর হ্যান্ডলিং
`try-except` ব্লক ব্যবহার করে যেকোনো সমস্যা ধরে ফেলছি। কোন সুইচে সমস্যা হলে বাকি সুইচগুলোর কাজ আটকে যাবে না।

### ব্যবহার করার টিপস:

    - স্ক্রিপ্ট চালানোর আগে নেটমিকো ইনস্টল করুন: `pip install netmiko`
    - ব্যাকআপ ফোল্ডার আগে থেকে তৈরি করে রাখুন
    - সুইচগুলোতে SSH এনাবল আছে কিনা চেক করুন
    - প্রতিটা সুইচের সঠিক আইপি, ইউজারনেম ব্যবহার করুন

এই স্ক্রিপ্টটা একদম বেসিক, কিন্তু এর উপর ভিত্তি করে আপনি আরও অনেক মজার ফিচার যোগ করতে পারেন। এখানে কিছু অ্যাডভান্সড ফিচার যোগ করে নেটওয়ার্ক কনফিগারেশন ব্যাকআপ সিস্টেম বানাবো এখন। প্রতিটি ফিচার লাইন বাই লাইন বুঝে নেই:

### ১. ডেট-টাইম স্ট্যাম্পিং

```python
from datetime import datetime

# বর্তমান সময় নিয়ে একটা ফরম্যাটেড স্ট্রিং তৈরি করি
timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")

# ফাইলের নামে টাইমস্ট্যাম্প যোগ করি
backup_file = f"{backup_folder}/backup_{current_switch}_{timestamp}.txt"
```

### এখানে:

    - `datetime` মডিউল থেকে বর্তমান সময় নিচ্ছি
    - `strftime()` ফাংশন দিয়ে সময়কে সুন্দর ফরম্যাটে রূপান্তর করছি
    - %Y = বছর (2024)
    - %m = মাস (01-12)
    - %d = দিন (01-31)
    - %H = ঘণ্টা (00-23)
    - %M = মিনিট (00-59)
    - %S = সেকেন্ড (00-59)
    - ফাইলের নামে এই টাইমস্ট্যাম্প যুক্ত করে রাখছি

### ২. মাল্টিভেন্ডর সাপোর্ট

```python
# বিভিন্ন ভেন্ডরের কমান্ড ম্যাপিং
vendor_commands = {
    "cisco_ios": "show running-config",        # সিসকো সুইচের জন্য
    "juniper": "show configuration | display set",  # জুনিপারের জন্য
    "mikrotik_routeros": "export",            # মাইক্রোটিকের জন্য
    "huawei": "display current-configuration"  # হুয়াওয়ের জন্য
}

# ডিভাইসের টাইপ অনুযায়ী সঠিক কমান্ড নির্বাচন করে চালানো
config = connection.send_command(vendor_commands[device['device_type']])
```

### এখানে:

    - একটা ডিকশনারি তৈরি করছি যেখানে প্রতিটা ভেন্ডরের জন্য আলাদা কমান্ড রাখা হয়েছে
    - `device['device_type']` দিয়ে ডিভাইসের টাইপ চেক করে সেই অনুযায়ী কমান্ড সিলেক্ট করছি
    - `send_command()` দিয়ে সিলেক্টেড কমান্ড চালাচ্ছি

### ৩. লগিং সিস্টেম

```python
import logging

# লগিং কনফিগারেশন সেটআপ
logging.basicConfig(
    filename=f"{backup_folder}/backup_operations.log",  # লগ ফাইলের লোকেশন
    level=logging.INFO,                                # লগ লেভেল সেট
    format='%(asctime)s - %(levelname)s - %(message)s' # লগ ফরম্যাট
)

# বিভিন্ন ধরনের লগ এন্ট্রি
logging.info(f"Starting backup for {current_switch}")   # সাধারণ ডাটা
logging.error(f"Error in {current_switch}: {str(e)}")  # এরর মেসেজ
```

#### এখানে:

  - `logging` মডিউল ইমপোর্ট করছি
  - লগ ফাইলের বেসিক সেটিংস করছি:
    - কোথায় লগ সেভ হবে
    - কোন লেভেলের লগ রাখব (INFO, ERROR, etc.)
    - লগের ফরম্যাট কেমন হবে
  - বিভিন্ন জায়গায় লগ করে রাখছি

### ৪. কনফিগারেশন ভ্যালিডেশন

```python
def validate_backup(config_file):
    try:
        # ফাইল খুলে কনটেন্ট চেক করি
        with open(config_file, 'r') as f:
            content = f.read()
            
            # কনফিগ ফাইল খুব ছোট কিনা চেক
            if len(content) < 100:  
                logging.warning(f"Suspicious backup size for {config_file}")
                return False
                
            return True
            
    except Exception as e:
        # কোন সমস্যা হলে লগ করি
        logging.error(f"Validation failed: {str(e)}")
        return False
```

#### এখানে:

  - একটা ফাংশন বানাচ্ছি যা ব্যাকআপ ফাইল চেক করে
  - ফাইল খোলা যায় কিনা চেক করে
  - কনটেন্টের সাইজ যুক্তিসংগত কিনা দেখে
  - কোন সমস্যা হলে এরর লগ করে

### ৫. ইমেইল নোটিফিকেশন

```python
import smtplib
from email.mime.text import MIMEText

def send_notification(status, details):
    # ইমেইল সেটিংস
    sender = "backup@link3.net"
    receiver = "admin@link3.net"
    
    # ইমেইল বডি তৈরি
    msg = MIMEText(f"Backup Status: {status}\n\nDetails:\n{details}")
    msg['Subject'] = f"Network Backup Report - {datetime.now().date()}"
    msg['From'] = sender
    msg['To'] = receiver
    
    # ইমেইল পাঠানো
    with smtplib.SMTP('smtp.link3.net') as server:
        server.send_message(msg)
```

#### এখানে:

  - ইমেইল পাঠানোর জন্য SMTP লাইব্রেরি ব্যবহার করছি
  - ইমেইলের বিষয়বস্তু তৈরি করছি
  - SMTP সার্ভার দিয়ে মেইল পাঠাচ্ছি

### ৬. সমান্তরাল প্রসেসিং (Parallel Processing)

```python
from concurrent.futures import ThreadPoolExecutor, as_completed

def backup_all_devices():
    # একসাথে সর্বোচ্চ ৫টা ডিভাইসের ব্যাকআপ নিতে থ্রেড পুল তৈরি
    with ThreadPoolExecutor(max_workers=5) as executor:
        # প্রতিটা ডিভাইসের জন্য একটা করে থ্রেড তৈরি করি
        future_to_device = {
            executor.submit(backup_single_device, device): device 
            for device in device_list
        }
        
        # যে যে থ্রেড শেষ হয়ে যাবে তাদের রেজাল্ট পর্যবেক্ষণ করি
        for future in as_completed(future_to_device):
            device = future_to_device[future]
            try:
                result = future.result()
            except Exception as e:
                # কোন থ্রেডে সমস্যা হলে লগ করি
                logging.error(f"Device {device['host']} failed: {str(e)}")
```

#### এই কোডটা কিভাবে কাজ করে:

  - `ThreadPoolExecutor` একটা পুল তৈরি করে যেখানে একসাথে ৫টা থ্রেড চলতে পারে
  - প্রতিটা ডিভাইসের ব্যাকআপ আলাদা থ্রেডে চলে
  - একটা থ্রেড শেষ হলেই নতুন ডিভাইসের ব্যাকআপ শুরু হয়
  - এভাবে অনেক দ্রুত সব ডিভাইসের ব্যাকআপ নেওয়া সম্ভব হয়

### ৭. ফাইল কমপ্রেশন

```python
import gzip
import shutil
import os

def compress_old_backups():
    # ব্যাকআপ ফোল্ডারের সব ফাইল চেক করি
    for file in os.listdir(backup_folder):
        # শুধু টেক্সট ফাইলগুলো নিয়ে কাজ করি
        if file.endswith('.txt'):
            file_path = os.path.join(backup_folder, file)
            
            # gzip দিয়ে ফাইল কমপ্রেস করি
            with open(file_path, 'rb') as f_in:
                with gzip.open(f"{file_path}.gz", 'wb') as f_out:
                    shutil.copyfileobj(f_in, f_out)
            
            # মূল ফাইল ডিলিট করি
            os.remove(file_path)
```

#### এই ফাংশনটা যা করে:

  - সব পুরনো টেক্সট ফাইল খুঁজে বের করে
  - প্রতিটা ফাইলকে gzip দিয়ে কমপ্রেস করে
  - কমপ্রেস করা ফাইল সেভ করে মূল ফাইল মুছে ফেলে
  - এতে ডিস্ক স্পেস বাঁচে এবং ফাইল ট্রান্সফার সহজ হয়

### ৮. গিট ইন্টিগ্রেশন

```python
from git import Repo

def commit_to_git():
    # গিট রিপোজিটরি অবজেক্ট তৈরি
    repo = Repo(backup_folder)
    
    # সব পরিবর্তন স্টেজ করি
    repo.index.add('*')
    
    # পরিবর্তনগুলো কমিট করি
    repo.index.commit(f"Backup taken on {datetime.now()}")
    
    # রিমোট রিপোতে পুশ করি
    origin = repo.remote(name='origin')
    origin.push()
```

#### এই কোডটা:

  - ব্যাকআপ ফোল্ডারকে একটা গিট রিপোজিটরি হিসেবে ব্যবহার করে
  - নতুন ব্যাকআপ ফাইলগুলো গিটে যোগ করে
  - একটা কমিট মেসেজ সহ পরিবর্তনগুলো কমিট করে
  - সবশেষে রিমোট রিপোজিটরিতে পুশ করে

### সব মিলিয়ে এই আপগ্রেডগুলোর সুবিধা:

  1. টাইমস্ট্যাম্প দিয়ে কখন কোন ব্যাকআপ নেওয়া হয়েছে সহজে বোঝা যায়
  2. বিভিন্ন ভেন্ডরের ডিভাইস একই স্ক্রিপ্ট দিয়ে হ্যান্ডেল করা যায়
  3. লগ ফাইল থেকে কোন সমস্যা হলে সহজে ডিবাগ করা যায়
  4. ভ্যালিডেশন দিয়ে নিশ্চিত করা যায় ব্যাকআপ সঠিকভাবে হয়েছে
  5. ইমেইল নোটিফিকেশন দিয়ে দূর থেকে মনিটর করা যায়
  6. প্যারালাল প্রসেসিং দিয়ে দ্রুত ব্যাকআপ নেওয়া যায়
  7. কমপ্রেশন দিয়ে স্টোরেজ স্পেস বাঁচানো যায়
  8. গিট ইন্টিগ্রেশন দিয়ে ভার্সন কন্ট্রোল করা যায়

এই সব ফিচার একসাথে যোগ করলে আপনার স্ক্রিপ্ট হয়ে যাবে একটা প্রফেশনাল-গ্রেড ব্যাকআপ সিস্টেম!