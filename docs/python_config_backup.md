নেটওয়ার্কের কনফিগারেশন ব্যাকআপের প্রক্রিয়া বুঝতে আমরা একটি উদাহরণ দিয়ে শুরু করব। এখানে আমরা পাইথন এবং নেটমিকো লাইব্রেরি ব্যবহার করে কিভাবে নেটওয়ার্ক ডিভাইসের কনফিগারেশন ব্যাকআপ করা যায় তা দেখাব।

### পাইথন দিয়ে নেটওয়ার্কের কনফিগারেশন ব্যাকআপ

আগের চ্যাপ্টারে আমরা পাইথন নিয়ে কিছুটা আলাপ করেছি। আমি চেষ্টা করেছি এখানে যতটুকু পাইথন লাগবে সেটাই কাভার করা, এর থেকে বেশী নয়। পাইথন স্ক্রিপ্ট দিয়ে আমাদের নেটওয়ার্ক ডিভাইসে এসএসএইচ (SSH) দিয়ে ঢুকে সেখান থেকে কনফিগারেশন ফাইলগুলোকে কপি করে ব্যাকআপ করব আমাদের নির্দিষ্ট কম্পিউটারে।

শুরুতে এটাকে খুবই সাধারণ একটা বেসিক স্ক্রিপটে রাখছি ইচ্ছে করে যার মধ্যে কোন ডেট-টাইম স্টাম্পিং থাকছে না অথবা বিভিন্ন ম্যানুফ্যাকচারার যেমন, সিসকো, জুনিপার, মাইক্রোটিক অথবা হুয়াওয়ে এতগুলো ভেন্ডর না রেখে একটা ম্যানুফ্যাকচারার সিলেক্ট করেছি। এর পাশাপাশি, স্ক্রিপ্টটাকে ইন্টেলিজেন্ট করার জন্য বিভিন্ন এর হ্যান্ডলিং অথবা প্যারালাল ভাবে আরো কিছু অপারেশন চালানোর মত দক্ষতা নিয়ে আলাপ করব সামনে। এখানে এডভান্সড লেভেলের স্ক্রিপ্ট তৈরি করা যায় তবে আমরা যদি বেসিক ফাউন্ডেশনাল টুল হিসাবে এটা না বুঝি তাহলে সামনে আরো ভেতরে ঢোকা দুষ্কর হবে।

আমরা আগেই বলেছি পাইথনের বিউটি হচ্ছে এর প্রচুর হেল্পার লাইব্রেরী আছে যার মাধ্যমে বড় বড় অপারেশনের অনেক কমপ্লেক্স সিটি হাইড করে রাখে আমাদের মত নতুন অটোমেশন এক্সপার্টদের কাছ থেকে। সেরকম একটা হেল্পার লাইব্রেরী হচ্ছে নেটমিকো। অনেকে প্যারামিকোর কথা বলবেন, তবে নেটমিকো প্যারামিকোর উপরের ভিত্তি করে তৈরি করা, এবং নেটমিকোতে অনেক এডভান্সড ফিচার আছে যেটা প্যারামিকোর অনেক কমপ্লেক্সসিটি লুকিয়ে রাখতে পারে।

### নেটমিকো কি?

আমাদের ল্যাপটপ বা পিসি থেকে নেটমিকো লাইব্রেরী ব্যবহার করে কয়েকটা ডিভাইসে কানেক্ট করব, একটা ফর লুপ ব্যবহার করে। নেটমিকো হচ্ছে একটা পাইথন লাইব্রেরী যেটা একটা নেটওয়ার্কের ভিতরে ব্যবহৃত বিভিন্ন ম্যানুফ্যাকচারারের ডিভাইসে কিভাবে সহজে অটমেশন করা যায় সেটাই দেখাবো এখানে। মোদ্দা কথায় এর কাজ হচ্ছে একটা ইউনিফাইড ইন্টারফেস তৈরি করা যাতে আমরা সরাসরি বিভিন্ন ধরনের ডিভাইসের সাথে ইন্টারেক্ট করতে পারি। চলুন শুরুতে নেটমিকো ইনস্টল করে নেই, পাইথনের পিপ প্যাকেজ ম্যানেজার দিয়ে।

**ইনস্টল করি:**
```
pip install netmiko
```

### কোড উদাহরণ: কনফিগারেশন ব্যাকআপ

আমাদের এই স্ক্রিপ্ট শুরু হচ্ছে দুটি মডিউলকে ইমপোর্ট করে। একটি হচ্ছে নেটমিকো, যার কাজ হচ্ছে এসএসএইচ কানেকশন তৈরি করে দেওয়া, এবং অন্যটি হচ্ছে গেটপাস, যা পাসওয়ার্ড প্রম্পটকে নিরাপদ ভাবে ইনপুট নেওয়ার জন্য ব্যবহৃত হবে।

```python
from netmiko import ConnectHandler
import getpass

# পাসওয়ার্ড ইনপুট নিন
passwd = getpass.getpass('Please enter the password: ')

# কনফিগারেশন ব্যাকআপ স্টোর করার স্থান
file_dir = '/Users/nabeel/Documents/config-backup/backups'
switch_list = ['192.168.10.10', 'core-switch-01', 'test_switch']
device_list = []

# ডিভাইসের তথ্য সংরক্ষণ
for ip in switch_list:
    device = {
        "device_type": "cisco_ios",
        "host": ip,
        "username": "nabeel",
        "password": passwd,
        "secret": passwd  # Enable password
    }
    device_list.append(device)

# প্রতিটি ডিভাইসে সংযোগ স্থাপন এবং কনফিগারেশন ব্যাকআপ
for device in device_list:
    host_name = device['host']
    try:
        connection = ConnectHandler(**device)
        show_run = connection.send_command('show run')
        
        # কনফিগারেশন ফাইল সংরক্ষণ
        with open(f"{file_dir}/show_run_{host_name}.txt", 'w') as f:
            f.write(show_run)
        
        connection.disconnect()
        print(f"{host_name} থেকে কনফিগারেশন ব্যাকআপ সফল হয়েছে")
    except Exception as e:
        print(f"{host_name} তে সমস্যা ঘটেছে: {str(e)}")
```

### কোড ব্যাখ্যা:

1. **পাসওয়ার্ড ইনপুট**: প্রথমে, আমরা `getpass.getpass` ফাংশনের মাধ্যমে নিরাপদভাবে পাসওয়ার্ড ইনপুট নিচ্ছি যাতে পাসওয়ার্ডটি ইনপুট করার সময় স্ক্রিনে প্রদর্শিত না হয়।
2. **কনফিগারেশন স্টোর করার স্থান নির্ধারণ**: যেখানে কনফিগারেশন ব্যাকআপ ফাইলগুলো সংরক্ষণ করা হবে সেই স্থানটি নির্ধারণ করছি।
3. **ডিভাইসের তথ্য সংরক্ষণ**: প্রতিটি ডিভাইসের তথ্য একটি লুপের মাধ্যমে ডিকশনারিতে সংরক্ষণ করা হচ্ছে এবং তা `device_list` এ যোগ করা হচ্ছে।
4. **ডিভাইসে সংযোগ এবং কনফিগারেশন ব্যাকআপ**: প্রতিটি ডিভাইসের জন্য, আমরা নেটমিকো `ConnectHandler` ক্লাস ব্যবহার করে সংযোগ স্থাপন করছি এবং `send_command` মেথড ব্যবহার করে `show run` কমান্ডটি পাঠাচ্ছি। এরপর সেই কনফিগারেশন ফাইলটি নির্দিষ্ট স্থানে সংরক্ষণ করছি।
5. **সংযোগ বিচ্ছিন্ন এবং সমস্যা হ্যান্ডলিং**: কাজ শেষ হলে সংযোগ বিচ্ছিন্ন করা হচ্ছে এবং কোন সমস্যা হলে তা প্রদর্শিত হচ্ছে।

### কোড কিভাবে ডেভেলপ করবো?

1. **কনকারেন্সি**: বর্তমান স্ক্রিপ্টটি প্রতিটি ডিভাইসে সিরিয়ালি সংযোগ স্থাপন করে কাজ সম্পন্ন করে। এতে গতি বাড়ানোর জন্য কনকারেন্সি ফিচার যোগ করা যেতে পারে।
2. **এরর হ্যান্ডলিং**: আরও উন্নত এরর হ্যান্ডলিং যোগ করা যেতে পারে যাতে স্ক্রিপ্টটি কোন ডিভাইসে ত্রুটি ঘটলে পরবর্তী ডিভাইসে কাজ চালিয়ে যেতে পারে।
3. **গিট ইন্টিগ্রেশন**: কনফিগারেশন ব্যাকআপ রাখতে গিট এর মত একটি ভার্সন কন্ট্রোল সিস্টেম যোগ করা যেতে পারে যাতে প্রতিটি কনফিগারেশনের পরিবর্তন ট্র্যাক করা যায়।

### কোড উদাহরণ: কনফিগারেশন ব্যাকআপে কনকারেন্সি যোগ করা

কনকারেন্সি যোগ করার জন্য পাইথনের `concurrent.futures` মডিউল ব্যবহার করা যেতে পারে। এটি আমাদেরকে থ্রেড বা প্রসেস পুল ব্যবহার করে বিভিন্ন কাজ প্যারালালি সম্পাদন করার সুযোগ দেয়। নিচে একটি উদাহরণ দেওয়া হলো যেখানে আমরা আমাদের আগের স্ক্রিপ্টকে কনকারেন্টভাবে চালাব।

প্রথমে `concurrent.futures` মডিউল ইমপোর্ট করুন এবং তারপর আমাদের পূর্বের স্ক্রিপ্টকে মডিফাই করুন:

```python
from netmiko import ConnectHandler
import getpass
import concurrent.futures

# পাসওয়ার্ড ইনপুট নিন
passwd = getpass.getpass('Please enter the password: ')

# কনফিগারেশন ব্যাকআপ স্টোর করার স্থান
file_dir = '/Users/nabeel/Documents/config-backup/backups'
switch_list = ['192.168.10.10', 'core-switch-01', 'test_switch']
device_list = []

# ডিভাইসের তথ্য সংরক্ষণ
for ip in switch_list:
    device = {
        "device_type": "cisco_ios",
        "host": ip,
        "username": "nabeel",
        "password": passwd,
        "secret": passwd  # Enable password
    }
    device_list.append(device)

# ফাংশন যা একটি ডিভাইসে সংযোগ স্থাপন করে কনফিগারেশন ব্যাকআপ করে
def backup_config(device):
    host_name = device['host']
    try:
        connection = ConnectHandler(**device)
        show_run = connection.send_command('show run')
        
        # কনফিগারেশন ফাইল সংরক্ষণ
        with open(f"{file_dir}/show_run_{host_name}.txt", 'w') as f:
            f.write(show_run)
        
        connection.disconnect()
        print(f"{host_name} থেকে কনফিগারেশন ব্যাকআপ সফল হয়েছে")
    except Exception as e:
        print(f"{host_name} তে ত্রুটি ঘটেছে: {str(e)}")

# কনকারেন্টলি ব্যাকআপ করার জন্য থ্রেড পুল ব্যবহার
with concurrent.futures.ThreadPoolExecutor() as executor:
    # প্রতিটি ডিভাইসের জন্য ফাংশনটি সাবমিট করুন
    futures = [executor.submit(backup_config, device) for device in device_list]
    
    # প্রতিটি ফিউচারের জন্য ফলাফল অপেক্ষা করুন
    for future in concurrent.futures.as_completed(futures):
        future.result()  # কোন ত্রুটি থাকলে তা এখানে উঠবে
```

### কোড ব্যাখ্যা:

1. **`concurrent.futures` মডিউল ইমপোর্ট**: প্রথমে আমরা কনকারেন্ট ফিউচারের মডিউল ইমপোর্ট করছি যা আমাদের থ্রেড বা প্রসেস পুল তৈরি করতে সাহায্য করবে।
2. **`backup_config` ফাংশন তৈরি**: এই ফাংশনটি প্রতিটি ডিভাইসে সংযোগ স্থাপন করে কনফিগারেশন ব্যাকআপ করবে।
3. **থ্রেড পুল তৈরি করা**: `ThreadPoolExecutor` ব্যবহার করে একটি থ্রেড পুল তৈরি করা হয়েছে।
4. **ফাংশন সাবমিট করা**: প্রতিটি ডিভাইসের জন্য `backup_config` ফাংশনটি সাবমিট করা হয়েছে।
5. **ফলাফল অপেক্ষা করা**: `concurrent.futures.as_completed` ব্যবহার করে প্রতিটি থ্রেডের ফলাফল অপেক্ষা করা হয়েছে এবং কোন ত্রুটি থাকলে তা প্রদর্শন করা হয়েছে।

এই ভাবে আমরা আমাদের স্ক্রিপ্টকে কনকারেন্টলি চালাতে পারি, যা আমাদের কাজের গতি বাড়াতে সাহায্য করবে।

### পাইথন দিয়ে নেটওয়ার্ক কনফিগারেশন ব্যাকআপ এবং গিট ইন্টিগ্রেশন

এই স্ক্রিপ্টটি দেখাবে কিভাবে পাইথন এবং `Netmiko` ব্যবহার করে নেটওয়ার্ক ডিভাইস থেকে কনফিগারেশন ব্যাকআপ করা যায় এবং `GitPython` ব্যবহার করে সেই ব্যাকআপ ফাইল গিট রিপোজিটরিতে আপলোড করা যায়।

#### ধাপ ১: প্রয়োজনীয় লাইব্রেরি ইনস্টল এবং ইমপোর্ট করা

প্রথমে আমাদের প্রয়োজনীয় লাইব্রেরি ইনস্টল করতে হবে। টার্মিনালে নিচের কমান্ডগুলি চালান:

```bash
pip install netmiko gitpython
```

এরপর, আমাদের স্ক্রিপ্টের শুরুতে এই লাইব্রেরিগুলো ইমপোর্ট করি:

```python
from netmiko import ConnectHandler
import getpass
import concurrent.futures
import os
from git import Repo
```

#### ধাপ ২: পাসওয়ার্ড ইনপুট নেয়া

নিরাপত্তার জন্য ব্যবহারকারীর কাছ থেকে পাসওয়ার্ড ইনপুট নিয়ে সেটি সংরক্ষণ করা হবে:

```python
passwd = getpass.getpass('Please enter the password: ')
```

#### ধাপ ৩: কনফিগারেশন ব্যাকআপ সংরক্ষণের স্থান এবং গিট রিপোজিটরির ঠিকানা নির্ধারণ

কনফিগারেশন ব্যাকআপ ফাইল কোথায় সংরক্ষণ করা হবে এবং গিট রিপোজিটরির ঠিকানা নির্ধারণ করা হবে:

```python
file_dir = '/Users/nabeel/Documents/config-backup/backups'
repo_url = 'https://github.com/raqueeb/network_automation.git'
local_repo_dir = '/Users/nabeel/Documents/config-backup'
```

#### ধাপ ৪: গিট রিপোজিটরি ক্লোন করা

যদি স্থানীয় ডিরেক্টরিতে রিপোজিটরি না থাকে, তবে এটি ক্লোন করা হবে:

```python
if not os.path.exists(local_repo_dir):
    Repo.clone_from(repo_url, local_repo_dir)

repo = Repo(local_repo_dir)
```

#### ধাপ ৫: নেটওয়ার্ক ডিভাইসের তালিকা প্রস্তুত করা

নেটওয়ার্ক ডিভাইসের তথ্য একটি তালিকায় সংরক্ষণ করা হবে:

```python
switch_list = ['192.168.10.10', 'core-switch-01', 'test_switch']
device_list = []

for ip in switch_list:
    device = {
        "device_type": "cisco_ios",
        "host": ip,
        "username": "nabeel",
        "password": passwd,
        "secret": passwd  # Enable password
    }
    device_list.append(device)
```

#### ধাপ ৬: কনফিগারেশন ব্যাকআপ এবং গিটে আপলোড করার ফাংশন

এই ফাংশনটি প্রতিটি ডিভাইসে সংযোগ স্থাপন করে কনফিগারেশন ব্যাকআপ করবে এবং সেটি গিটে আপলোড করবে:

```python
def backup_and_commit(device):
    host_name = device['host']
    try:
        connection = ConnectHandler(**device)
        show_run = connection.send_command('show run')
        
        # কনফিগারেশন ফাইল সংরক্ষণ
        file_path = f"{file_dir}/show_run_{host_name}.txt"
        with open(file_path, 'w') as f:
            f.write(show_run)
        
        connection.disconnect()
        print(f"{host_name} থেকে কনফিগারেশন ব্যাকআপ সফল হয়েছে")

        # গিটে ফাইলটি কমিট এবং পুশ করুন
        repo.index.add([file_path])
        repo.index.commit(f"Backup configuration for {host_name}")
        origin = repo.remote(name='origin')
        origin.push()
        print(f"{host_name} এর কনফিগারেশন গিটে আপলোড হয়েছে")

    except Exception as e:
        print(f"{host_name} তে ত্রুটি ঘটেছে: {str(e)}")
```

#### ধাপ ৭: থ্রেড পুল ব্যবহার করে কনকারেন্টলি ব্যাকআপ এবং গিট কমিট করা

কনকারেন্টলি প্রতিটি ডিভাইসের কনফিগারেশন ব্যাকআপ করতে এবং গিটে আপলোড করতে থ্রেড পুল ব্যবহার করা হবে:

```python
with concurrent.futures.ThreadPoolExecutor() as executor:
    # প্রতিটি ডিভাইসের জন্য ফাংশনটি সাবমিট করুন
    futures = [executor.submit(backup_and_commit, device) for device in device_list]
    
    # প্রতিটি ফিউচারের জন্য ফলাফল অপেক্ষা করুন
    for future in concurrent.futures.as_completed(futures):
        future.result()  # কোন ত্রুটি থাকলে তা এখানে উঠবে
```

### সম্পূর্ণ কোড

```python
from netmiko import ConnectHandler
import getpass
import concurrent.futures
import os
from git import Repo

# পাসওয়ার্ড ইনপুট নিন
passwd = getpass.getpass('Please enter the password: ')

# কনফিগারেশন ব্যাকআপ স্টোর করার স্থান
file_dir = '/Users/nabeel/Documents/config-backup/backups'
repo_url = 'https://github.com/raqueeb/network_automation.git'
local_repo_dir = '/Users/nabeel/Documents/config-backup'

# গিট রিপোজিটরি ক্লোন করুন যদি না থাকে
if not os.path.exists(local_repo_dir):
    Repo.clone_from(repo_url, local_repo_dir)

repo = Repo(local_repo_dir)
switch_list = ['192.168.10.10', 'core-switch-01', 'test_switch']
device_list = []

# ডিভাইসের তথ্য সংরক্ষণ
for ip in switch_list:
    device = {
        "device_type": "cisco_ios",
        "host": ip,
        "username": "nabeel",
        "password": passwd,
        "secret": passwd  # Enable password
    }
    device_list.append(device)

# ফাংশন যা একটি ডিভাইসে সংযোগ স্থাপন করে কনফিগারেশন ব্যাকআপ করে এবং গিটে আপলোড করে
def backup_and_commit(device):
    host_name = device['host']
    try:
        connection = ConnectHandler(**device)
        show_run = connection.send_command('show run')
        
        # কনফিগারেশন ফাইল সংরক্ষণ
        file_path = f"{file_dir}/show_run_{host_name}.txt"
        with open(file_path, 'w') as f:
            f.write(show_run)
        
        connection.disconnect()
        print(f"{host_name} থেকে কনফিগারেশন ব্যাকআপ সফল হয়েছে")

        # গিটে ফাইলটি কমিট এবং পুশ করুন
        repo.index.add([file_path])
        repo.index.commit(f"Backup configuration for {host_name}")
        origin = repo.remote(name='origin')
        origin.push()
        print(f"{host_name} এর কনফিগারেশন গিটে আপলোড হয়েছে")

    except Exception as e:
        print(f"{host_name} তে ত্রুটি ঘটেছে: {str(e)}")

# কনকারেন্টলি ব্যাকআপ এবং গিট কমিট করার জন্য থ্রেড পুল ব্যবহার
with concurrent.futures.ThreadPoolExecutor() as executor:
    # প্রতিটি ডিভাইসের জন্য ফাংশনটি সাবমিট করুন
    futures = [executor.submit(backup_and_commit, device) for device in device_list]
    
    # প্রতিটি ফিউচারের জন্য ফলাফল অপেক্ষা করুন
    for future in concurrent.futures.as_completed(futures):
        future.result()  # কোন ত্রুটি থাকলে তা এখানে উঠবে
```

এই স্ক্রিপ্টটি আপনার নেটওয়ার্ক ডিভাইসগুলির কনফিগারেশন ব্যাকআপ করবে এবং গিট রিপোজিটরিতে আপলোড করবে, যা আপনার কনফিগারেশনগুলির একটি সুরক্ষিত ব্যাকআপ রাখবে।