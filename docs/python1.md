# নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য পাইথন: একদম শুরু থেকে

আমি নিজেও একজন নেটওয়ার্ক ইঞ্জিনিয়ার ছিলাম - আমরা শুরুতে `show ip route`, `show interface` এসব কমান্ড নিয়ে কাজ করতাম। প্রোগ্রামিং আমাদের কাছে একটা ভয়ের জিনিস, কারণ এটা আমাদের কম্ফোর্ট জোন না। কিন্তু ভাবুন, প্রতিদিন একই কমান্ড বারবার টাইপ করতে করতে ক্লান্ত লাগে না? অথবা একসাথে ১০০টা রাউটার চেক করতে হলে কী করবেন? এই গাইড আমাদেরকে দেখাবে কীভাবে CLI কমান্ডের মতোই সহজভাবে পাইথন শেখা যায়। যেমন `show` কমান্ড দিয়ে আমরা ইনফরমেশন দেখি, তেমনি `print()` দিয়ে পাইথনে ইনফরমেশন দেখবো। একদম গোড়া থেকে শুরু করব, যাতে আমরা কম্ফোর্টেবলি নিজের গতিতে শিখতে পারি। মনে রাখবেন, নেটওয়ার্কিং যেমন একদিনে শেখেননি, প্রোগ্রামিংও তেমনি ধীরে ধীরে শিখবেন।

## লেসন ১: প্রথম পরিচয়

### প্রোগ্রামিং কী?
প্রোগ্রামিং হলো কম্পিউটারকে ইন্সট্রাকশন দেওয়া। ঠিক যেমন আপনি CLI-তে কমান্ড দিয়ে রাউটার-সুইচকে ইন্সট্রাকশন দেন, তেমনি প্রোগ্রামিংয়ে লাইন বাই লাইন ইন্সট্রাকশন দিয়ে কাজ করানো হয়।

### পাইথন শুরু করা
প্রথমে Visual Studio Code (VS Code) ইনস্টল করে নিন। এটা হলো আপনার নতুন CLI। তারপর একটা নতুন ফাইল খুলুন, নাম দিন `first.py`

এখন প্রথম কমান্ড:
```python
# এটা হলো প্রিন্ট কমান্ড, CLI-তে show কমান্ডের মতো
print("Hello")
```

### CLI vs পাইথন তুলনা:
```bash
# CLI-তে যেভাবে লেখেন:
Router> show version

# পাইথনে সেভাবে লিখবেন:
print("Checking version...")
```

## লেসন ২: ভেরিয়েবল - মেমোরিতে জিনিস রাখা

### ভেরিয়েবল কী?
CLI-তে যখন `hostname R1` লেখেন, রাউটার সেই নাম মনে রাখে। পাইথনে ভেরিয়েবল ঠিক তেমনি কাজ করে - এটা একটা নাম, যেখানে কিছু স্টোর করা থাকে।

```python
# রাউটারের নাম স্টোর করা
# যেভাবে পড়বেন: router_name নামক জায়গায় "R1" রাখলাম
router_name = "R1"

# এখন এই নাম দেখতে চাইলে
print(router_name)    # আউটপুট: R1
```

### ভেরিয়েবলের টাইপ:
```python
# স্ট্রিং: টেক্সট টাইপের ডাটা (উদাহরণ: নাম, IP অ্যাড্রেস)
hostname = "CoreRouter"

# ইন্টিজার: পূর্ণসংখ্যা (উদাহরণ: VLAN নাম্বার, পোর্ট নাম্বার)
vlan_number = 10

# বুলিয়ান: হ্যাঁ/না টাইপের ডাটা (উদাহরণ: পোর্ট আপ/ডাউন)
is_port_up = True

# ফ্লোট: দশমিক সংখ্যা (উদাহরণ: লিংক স্পীড গিগাবিটে)
link_speed = 1.5
```

### মজার ব্যাপার:
```python
# একাধিক ভেরিয়েবলকে একসাথে ব্যবহার করা যায়
device_name = "CoreSwitch"
location = "Dhaka"

# কমা দিয়ে একসাথে প্রিন্ট করা
print(device_name, location)    # আউটপুট: CoreSwitch Dhaka

# দুইটা স্ট্রিং যোগ করা
full_name = device_name + "-" + location
print(full_name)    # আউটপুট: CoreSwitch-Dhaka
```

### ভেরিয়েবলের নাম দেওয়ার নিয়ম:
1. শুধু ইংরেজি অক্ষর, নাম্বার আর আন্ডারস্কোর (_) ব্যবহার করা যাবে
2. নাম্বার দিয়ে শুরু করা যাবে না
3. স্পেস দেওয়া যাবে না
4. বাংলা বা অন্য ভাষা ব্যবহার করা যাবে না

সঠিক নাম:
```python
router_name = "R1"
vlan10 = "Management"
management_ip = "192.168.1.1"
```

ভুল নাম:
```python
1router = "R1"    # নাম্বার দিয়ে শুরু করা যাবে না
router name = "R1"    # স্পেস দেওয়া যাবে না
রাউটার = "R1"    # বাংলা ব্যবহার করা যাবে না
```

## লেসন ৩: print() ফাংশন - ডাটা দেখানো

CLI-তে আমরা `show` কমান্ড দিয়ে তথ্য দেখি। পাইথনে `print()` ফাংশন একই কাজ করে।

```python
# সাধারণ প্রিন্ট
print("Testing connection")

# ভেরিয়েবল প্রিন্ট
device = "Router-1"
print(device)

# একাধিক জিনিস প্রিন্ট
ip = "192.168.1.1"
status = "up"
print(device, ip, status)    # কমা দিয়ে আলাদা করলে স্পেস দিয়ে প্রিন্ট হয়
```

# নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য পাইথন: লেসন ৪

## লেসন ৪: f-string - ডাটা সুন্দরভাবে দেখানো

### f-string কী?
CLI-তে যখন `show ip interface brief` দেন, সুন্দর ফরম্যাট করা আউটপুট পান। পাইথনে f-string দিয়ে তেমনি সুন্দর আউটপুট বানানো যায়।

### সাধারণ স্ট্রিং vs f-string:

```python
# প্রথমে দুইটা ভেরিয়েবল নিই
device = "Router-1"
ip = "192.168.1.1"

# সাধারণ স্ট্রিং দিয়ে প্রিন্ট (পুরনো পদ্ধতি, জটিল)
print("Device " + device + " has IP " + ip)

# f-string দিয়ে প্রিন্ট (নতুন পদ্ধতি, সহজ)
print(f"Device {device} has IP {ip}")
```

### f-string এর মজার ব্যাপার:

১. **ব্রাকেটের {} ভিতরে সরাসরি ভেরিয়েবল বসানো যায়:**
```python
# ভেরিয়েবলগুলো আগে ডিক্লেয়ার করি 
hostname = "Core-SW"
location = "Dhaka"
rack_number = 5

# f-string দিয়ে সব একসাথে প্রিন্ট
print(f"Device {hostname} is in {location} at rack {rack_number}")
```

২. **মাল্টিলাইন আউটপুট বানানো যায় (টিপল কোট দিয়ে):**
```python
device_info = f"""
Device Information:
------------------
Name: {hostname}
Location: {location}
Rack: {rack_number}
"""
print(device_info)
```

৩. **ব্রাকেটের {} ভিতরে ক্যালকুলেশন করা যায়:**
```python
# পোর্ট এবং VLAN এর হিসাব
total_ports = 24
used_ports = 18

print(f"""
Port Statistics:
---------------
Total Ports: {total_ports}
Used Ports: {used_ports}
Free Ports: {total_ports - used_ports}
Usage: {(used_ports/total_ports) * 100}%
""")
```

### বাস্তব উদাহরণ - ইন্টারফেস স্ট্যাটাস রিপোর্ট:

```python
# প্রথমে ডাটা নিয়ে নিই
interface_name = "GigabitEthernet0/1"
interface_ip = "10.0.0.1"
interface_status = "up"
duplex = "full"
speed = 1000
last_update = "5 minutes ago"

# এখন f-string দিয়ে সুন্দর রিপোর্ট বানাই
interface_report = f"""
Interface Status Report
----------------------
Name: {interface_name}
IP Address: {interface_ip}
Status: {interface_status}
Speed: {speed} Mbps
Duplex: {duplex}
Last Updated: {last_update}
"""

print(interface_report)
```

### f-string টিপস: একটু বাংলায় দেখি

১. **f-string শুরু করতে স্ট্রিং এর আগে f বা F দিতে হয়:**
```python
print(f"এটা f-string")      # ঠিক আছে
print(F"এটাও f-string")     # ঠিক আছে
print("এটা f-string নয়")    # এটা সাধারণ স্ট্রিং
```

২. **কারলি ব্রেস {} ভিতরে যেকোনো ভেরিয়েবল বা এক্সপ্রেশন লেখা যায়:**
```python
# ভেরিয়েবল
port = 22
print(f"SSH Port: {port}")

# এক্সপ্রেশন
print(f"Double Port: {port * 2}")
```

৩. **মাল্টিলাইন f-string এ টিপল কোট (""") ব্যবহার করুন:**
```python
status = "active"
uptime = "5 days"

report = f"""
System Status:
-------------
Status: {status}
Uptime: {uptime}
"""
print(report)
```

### গুরুত্বপূর্ণ নোট:
- f-string পাইথন ৩.৬+ এ কাজ করে
- f-string ব্যবহার করলে কোড পড়তে এবং বুঝতে সহজ হয়
- সব জায়গায় f-string ব্যবহার করার দরকার নেই, শুধু যেখানে ভেরিয়েবল মিক্স করে প্রিন্ট করবেন সেখানে ব্যবহার করুন

# নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য পাইথন: লেসন ৫

## লেসন ৫: লিস্ট - একাধিক জিনিস একসাথে রাখা

### লিস্ট কেন দরকার?
মনে করুন, আপনার ১০টা রাউটার আছে। প্রতিটার নাম কি আলাদা আলাদা ভেরিয়েবলে রাখবেন?
```python
router1 = "R1"
router2 = "R2"
router3 = "R3"
# ... এভাবে ১০টা লাইন?
```
এর বদলে একটা লিস্টে সব রাখা যায়!

### লিস্ট তৈরি করা:
```python
# স্কয়ার ব্র্যাকেট [] দিয়ে লিস্ট তৈরি হয়
routers = ["R1", "R2", "R3", "R4", "R5"]

# এই রাউটার লিস্ট প্রিন্ট করি
print(routers)    # আউটপুট: ['R1', 'R2', 'R3', 'R4', 'R5']
```

### লিস্ট থেকে আইটেম অ্যাক্সেস করা:
```python
# পজিশন/ইনডেক্স নাম্বার দিয়ে অ্যাক্সেস করা যায়
# মনে রাখবেন, পজিশন 0 থেকে শুরু হয়!
routers = ["R1", "R2", "R3", "R4", "R5"]

print(routers[0])     # প্রথম রাউটার: R1
print(routers[1])     # দ্বিতীয় রাউটার: R2
print(routers[-1])    # শেষ রাউটার: R5
```

### লিস্টে নতুন আইটেম যোগ করা:
```python
# append() ফাংশন দিয়ে শেষে নতুন আইটেম যোগ করা যায়
routers = ["R1", "R2", "R3"]
routers.append("R4")
print(routers)    # আউটপুট: ['R1', 'R2', 'R3', 'R4']

# insert() ফাংশন দিয়ে নির্দিষ্ট পজিশনে যোগ করা যায়
routers.insert(1, "New-R2")    # পজিশন 1 এ যোগ হবে
print(routers)    # আউটপুট: ['R1', 'New-R2', 'R2', 'R3', 'R4']
```

### লিস্ট থেকে আইটেম বাদ দেওয়া:
```python
# remove() ফাংশন দিয়ে নির্দিষ্ট আইটেম মুছে ফেলা যায়
routers = ["R1", "R2", "R3", "R4"]
routers.remove("R2")
print(routers)    # আউটপুট: ['R1', 'R3', 'R4']

# pop() ফাংশন দিয়ে পজিশন দিয়ে মুছে ফেলা যায়
removed_router = routers.pop(1)    # পজিশন 1 থেকে মুছে ফেলবে
print(f"Removed: {removed_router}")    # আউটপুট: Removed: R3
print(routers)    # আউটপুট: ['R1', 'R4']
```

### বাস্তব উদাহরণ - নেটওয়ার্ক ডিভাইস ম্যানেজমেন্ট:
```python
# অফিসের সব রাউটারের লিস্ট
office_routers = ["Core-R1", "Edge-R1", "Edge-R2"]

# সব সুইচের লিস্ট
office_switches = ["SW1", "SW2", "SW3", "SW4"]

# নতুন ডিভাইস এলো
office_routers.append("New-R1")
office_switches.append("New-SW1")

# সব ডিভাইস দেখাই
print("\nRouter List:")
print("-" * 20)    # 20টা হাইফেন প্রিন্ট করবে
for router in office_routers:
    print(router)

print("\nSwitch List:")
print("-" * 20)
for switch in office_switches:
    print(switch)
```

### মজার টিপস:

১. **লিস্টের সাইজ বের করা:**
```python
devices = ["R1", "R2", "R3"]
count = len(devices)    # len() ফাংশন দিয়ে সাইজ বের করা
print(f"Total devices: {count}")    # আউটপুট: Total devices: 3
```

২. **লিস্টে আইটেম আছে কিনা চেক করা:**
```python
devices = ["R1", "R2", "R3"]
if "R1" in devices:
    print("R1 found in the list!")
```

৩. **লিস্ট সর্ট করা:**
```python
devices = ["R3", "R1", "R2"]
devices.sort()    # অক্ষরের ক্রম অনুযায়ী সাজাবে
print(devices)    # আউটপুট: ['R1', 'R2', 'R3']
```

### গুরুত্বপূর্ণ নোট:
- লিস্ট ইনডেক্স 0 থেকে শুরু হয়
- লিস্টে যেকোনো টাইপের ডাটা রাখা যায় (স্ট্রিং, নাম্বার, বুলিয়ান)
- লিস্ট পরিবর্তনযোগ্য (mutable) - এর মানে আপনি যেকোনো সময় আইটেম যোগ বা বাদ দিতে পারেন

# নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য পাইথন: লেসন ৬

## লেসন ৬: লুপ - একই কাজ বারবার করা

### লুপ কেন দরকার?
মনে করুন, আপনার ২০টা রাউটারের স্ট্যাটাস চেক করতে হবে। CLI-তে আপনাকে ২০ বার কমান্ড টাইপ করতে হত। পাইথনে লুপ দিয়ে এক লাইনে এই কাজ করা যায়!

### for লুপ - লিস্টের সাথে কাজ করা:

```python
# প্রথমে একটা রাউটার লিস্ট নিই
routers = ["R1", "R2", "R3", "R4"]

# এখন প্রতিটা রাউটার নিয়ে কাজ করি
for router in routers:
    print(f"Checking status for {router}")

# আউটপুট:
# Checking status for R1
# Checking status for R2
# Checking status for R3
# Checking status for R4
```

### for লুপ কীভাবে কাজ করে?
- `for router in routers:` মানে: "routers লিস্টের প্রতিটা আইটেম একে একে নিয়ে router নামে রাখো"
- প্রতিবার লুপ ঘুরবে, router ভেরিয়েবলে নতুন ভ্যালু আসবে
- ইনডেন্টেশন (স্পেস দিয়ে ডান পাশে সরানো) দিয়ে বোঝায় কোন কোন লাইন লুপের মধ্যে আছে

### বাস্তব উদাহরণ - সব রাউটার চেক করা:

```python
# রাউটার এবং তাদের IP লিস্ট
routers = ["Core-1", "Edge-1", "Edge-2"]
ip_addresses = ["10.0.0.1", "10.0.0.2", "10.0.0.3"]

# প্রতিটা রাউটার চেক করি
print("Starting router check...")
print("-" * 30)    # ৩০টা হাইফেন দিয়ে লাইন আঁকি

for i in range(len(routers)):    # range(3) মানে 0,1,2
    print(f"""
    Checking router: {routers[i]}
    IP Address: {ip_addresses[i]}
    Status: Connected
    """)
```

### লুপের ভেতরে যদি আইটেম নাম্বার দরকার হয়:
```python
devices = ["R1", "R2", "R3"]

# enumerate() ফাংশন দিয়ে পজিশনসহ লুপ চালানো যায়
for index, device in enumerate(devices):
    print(f"{index+1}. {device}")

# আউটপুট:
# 1. R1
# 2. R2
# 3. R3
```

### লুপের ভেতরে কন্ডিশন (if-else):
```python
routers = ["R1", "R2", "R3", "R4"]
status = ["up", "down", "up", "up"]

for i in range(len(routers)):
    if status[i] == "up":
        print(f"{routers[i]} is working fine!")
    else:
        print(f"WARNING: {routers[i]} is down!")

# আউটপুট:
# R1 is working fine!
# WARNING: R2 is down!
# R3 is working fine!
# R4 is working fine!
```

### প্র্যাকটিক্যাল উদাহরণ - নেটওয়ার্ক স্ট্যাটাস রিপোর্ট:
```python
# নেটওয়ার্ক ডিভাইসের তথ্য
devices = ["Core-SW", "Edge-R1", "Edge-R2"]
types = ["switch", "router", "router"]
ips = ["192.168.1.1", "192.168.1.2", "192.168.1.3"]
ports_active = [10, 5, 5]
ports_total = [24, 8, 8]

print("Network Status Report")
print("=" * 50)

for i in range(len(devices)):
    # পোর্ট ইউটিলাইজেশন ক্যালকুলেট করি
    utilization = (ports_active[i] / ports_total[i]) * 100
    
    # স্ট্যাটাস রিপোর্ট প্রিন্ট করি
    print(f"""
    Device: {devices[i]}
    Type: {types[i]}
    IP: {ips[i]}
    Ports: {ports_active[i]} active out of {ports_total[i]}
    Utilization: {utilization}%
    {"-" * 30}
    """)
```

আসুন একটু একটু করে বুঝি এই কোডটা কী করছে।

প্রথমে ভাবুন আপনার অফিসে তিনটা ডিভাইস আছে। প্রতিটা ডিভাইসের জন্য কিছু তথ্য সংরক্ষণ করেছেন আলাদা আলাদা লিস্টে:
- `devices` লিস্টে ডিভাইসের নাম রাখা আছে
- `types` লিস্টে কোনটা রাউটার, কোনটা সুইচ সেই তথ্য আছে
- `ips` লিস্টে প্রতিটা ডিভাইসের আইপি আছে
- `ports_active` লিস্টে কয়টা পোর্ট এখন ব্যবহার হচ্ছে সেই সংখ্যা আছে
- `ports_total` লিস্টে মোট কয়টা পোর্ট আছে সেই সংখ্যা আছে

এরপর একটা লুপ চালিয়ে প্রতিটা ডিভাইসের রিপোর্ট বানাচ্ছে:
- ক্যালকুলেট করছে কত শতাংশ পোর্ট ব্যবহার হচ্ছে
- প্রতিটা ডিভাইসের জন্য একটা সুন্দর ফরম্যাটেড রিপোর্ট প্রিন্ট করছে
- আউটপুটে দেখাচ্ছে ডিভাইসের নাম, টাইপ, আইপি, কয়টা পোর্ট অ্যাক্টিভ, এবং পোর্ট ইউটিলাইজেশন

এটা সেই কাজই করছে যা আপনি মানুয়ালি করেন - প্রতিটা ডিভাইসে লগইন করে স্ট্যাটাস চেক করা। কিন্তু এখানে সব কাজটা অটোমেটিক হচ্ছে, মাত্র কয়েক সেকেন্ডে।

### গুরুত্বপূর্ণ টিপস:

১. **ইনডেন্টেশন খুব গুরুত্বপূর্ণ:**
```python
# ঠিক
for device in devices:
    print(device)    # স্পেস দিয়ে ডানে সরানো
    
# ভুল
for device in devices:
print(device)    # ইনডেন্টেশন নেই
```

২. **লুপ বন্ধ করতে ভুলবেন না:**
```python
for device in devices:
    print(f"Checking {device}")
print("All checks complete!")    # লুপের বাইরে, ইনডেন্টেশন নেই
```

৩. **range() ফাংশন বোঝা:**
```python
range(5)      # 0,1,2,3,4
range(2,5)    # 2,3,4
range(0,10,2) # 0,2,4,6,8 (২ করে বাড়বে)
```

# নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য পাইথন: লেসন ৭

## ডিকশনারি - একটা জিনিসের সব তথ্য একসাথে রাখা

আচ্ছা মনে করেন, আপনার অফিসে একটা নতুন রাউটার এসেছে। এই রাউটারের জন্য আপনার কি কি ইনফরমেশন দরকার? নাম লাগবে, আইপি এড্রেস লাগবে, কোন জায়গায় ইন্সটল করবেন সেটা লাগবে, আর মডেল নাম্বার লাগবে। 

আমরা আগে যেভাবে শিখেছি, সেভাবে লিখলে:
```python
# এভাবে আলাদা আলাদা ভেরিয়েবল নেয়া ঝামেলার
router_name = "Core-Router"
router_ip = "192.168.1.1"
router_model = "Cisco 4500"
router_location = "HQ Server Room"
```

এখন একই ডিভাইসের এত তথ্য আলাদা আলাদা রাখা বিরক্তিকর। তার চেয়ে সব তথ্য একসাথে রাখি:

```python
# একটা রাউটারের সব ইনফরমেশন একসাথে
router = {
    "name": "Core-Router",
    "ip": "192.168.1.1",
    "model": "Cisco 4500",
    "location": "HQ Server Room",
    "ports_active": 8,
    "is_configured": True
}

# তথ্য দেখার পদ্ধতি
print(router["name"])      # রাউটারের নাম দেখব
print(router.get("ip"))    # রাউটারের আইপি দেখব

# নতুন তথ্য যোগ করা
router["firmware"] = "15.1"

# তথ্য আপডেট করা
router["location"] = "New Server Room"
```

এবার একটা বাস্তব উদাহরণ দেখি। মনে করুন আপনার অফিসের সব নেটওয়ার্ক ডিভাইসের হিসাব রাখতে হবে:

```python
# অফিসের সব নেটওয়ার্ক ডিভাইসের ইনফরমেশন
network_devices = [
    {
        "name": "Core-Switch",
        "ip": "10.0.0.1",
        "type": "switch",
        "ports": 24,
        "location": "Main DC",
        "vlans": [10, 20, 30]
    },
    {
        "name": "Edge-Router",
        "ip": "10.0.0.2",
        "type": "router",
        "ports": 8,
        "location": "Branch Office",
        "routing_protocols": ["OSPF", "BGP"]
    }
]

# সব ডিভাইসের রিপোর্ট দেখাই
print("Network Device Status Report")
print("-" * 40)

for device in network_devices:
    print(f"""
    Device Name: {device['name']}
    Type: {device['type']}
    IP Address: {device['ip']}
    Location: {device['location']}
    Total Ports: {device['ports']}
    """)
```

ডিকশনারি ব্যবহারে কিছু উপকারী মেথড:

```python
# এই কী আছে কিনা চেক করা
if "ip" in router:
    print("IP information exists")

# সব কী একসাথে দেখা
print(router.keys())

# সব ভ্যালু একসাথে দেখা
print(router.values())

# ডিকশনারি খালি করা
router.clear()
```

বাস্তব কাজে ডিকশনারি মূলত ব্যবহার করা হয় কনফিগারেশন, ইনভেন্টরি ম্যানেজমেন্ট এবং স্ট্যাটাস ট্র্যাকিং এর জন্য। এতে করে সব ডাটা অর্গানাইজড থাকে এবং সহজে অ্যাক্সেস করা যায়।

# নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য পাইথন: লেসন ৮

## ফাংশন - নিজের কমান্ড বানানো

একজন নেটওয়ার্ক ইঞ্জিনিয়ার হিসেবে আপনি CLI-তে `show ip route` বা `show ip interface brief` এই কমান্ডগুলো রোজই ব্যবহার করেন। এগুলো আসলে সিস্কোর তৈরি করা কমান্ড। 

পাইথনেও আমরা নিজেদের মতো কমান্ড বানাতে পারি। এগুলোকে বলে ফাংশন।

### ফাংশন বানানোর সবচেয়ে সহজ উদাহরণ:

```python
# সিম্পল ফাংশন - শুধু প্রিন্ট করবে
def say_hello():
    print("Hello Network Engineer!")

# ফাংশন কল করা
say_hello()    # আউটপুট: Hello Network Engineer!
```

এখানে কী হলো:
- `def` দিয়ে ফাংশন শুরু করতে হয়
- `say_hello` হলো ফাংশনের নাম
- `()` চিহ্ন দিয়ে ফাংশন শেষ হবে
- `:` কোলন দিয়ে ফাংশনের কাজ শুরু
- ইনডেন্টেশন (৪ স্পেস) দিয়ে ফাংশনের কাজ লিখতে হয়

### একটু জটিল উদাহরণ - প্যারামিটার নিয়ে কাজ:

```python
# রাউটারের স্ট্যাটাস চেক করার ফাংশন
def check_router_status(router_name, ip_address):
    print(f"""
    Checking Router Status:
    ----------------------
    Router Name: {router_name}
    IP Address: {ip_address}
    Status: Connected
    """)

# ফাংশন ব্যবহার
check_router_status("Core-Router", "192.168.1.1")
```

### বাস্তব উদাহরণ - পিং চেক:

```python
# সিম্পল পিং চেকার ফাংশন
def check_connection(ip):
    # এখানে আসলে পিং করার কোড থাকবে
    print(f"Checking connection to {ip}")
    return True    # মনে করি পিং সাকসেসফুল

# ফাংশন ব্যবহার
result = check_connection("8.8.8.8")
if result:
    print("Connection successful!")
else:
    print("Connection failed!")
```

### কিছু মজার উদাহরণ:

```python
# ভিলান কালকুলেটর
def calculate_total_vlans(*vlan_lists):
    total = 0
    for vlans in vlan_lists:
        total += len(vlans)
    return total

# ডিভাইস চেকার
def verify_device(name, device_type="router"):
    if device_type == "router":
        print(f"Verifying router: {name}")
    elif device_type == "switch":
        print(f"Verifying switch: {name}")

# ফাংশন ব্যবহার
vlans1 = [10, 20, 30]
vlans2 = [40, 50]
total = calculate_total_vlans(vlans1, vlans2)
print(f"Total VLANs: {total}")

verify_device("R1")                    # টাইপ না দিলে router ধরে নেবে
verify_device("SW1", "switch")         # টাইপ এখানে switch দেয়া
```

### মনে রাখার বিষয়:

1. **ফাংশনের নাম অর্থপূর্ণ হওয়া উচিত:**
```python
# ভালো নাম
def check_interface_status():
    pass

# খারাপ নাম
def xyz123():
    pass
```

2. **ফাংশন ছোট হওয়া ভালো** - একটা ফাংশন একটা কাজ করবে

3. **ফাংশনের ডকুমেন্টেশন লেখা ভালো অভ্যাস:**
```python
def ping_check(ip):
    """
    This function checks if an IP is reachable
    Input: IP address as string
    Output: Returns True if ping successful, False otherwise
    """
    return True
```

## নেটওয়ার্ক ইঞ্জিনিয়ারদের জন্য পাইথন: আমরা এখন পর্যন্ত যা শিখলাম

আমরা শুরু করেছিলাম একদম বেসিক থেকে - প্রথমে শিখেছি কীভাবে `print()` ব্যবহার করে আউটপুট দেখানো যায়। তারপর শিখেছি ভেরিয়েবল নিয়ে কাজ করা, যেখানে ডাটা স্টোর করে রাখা যায়। এরপর দেখেছি `f-string`, যা দিয়ে সুন্দর করে ডাটা ফরম্যাট করা যায়। অনেক ডাটা নিয়ে কাজ করার জন্য শিখেছি লিস্ট, যেখানে অনেকগুলো আইটেম একসাথে রাখা যায়। লিস্টের সব আইটেম নিয়ে কাজ করার জন্য দেখেছি for লুপ। একটা ডিভাইসের সব ইনফরমেশন একসাথে রাখার জন্য শিখেছি ডিকশনারি। সবশেষে দেখেছি কীভাবে নিজেদের কাস্টম কমান্ড বা ফাংশন বানানো যায়। এই সব টুল ব্যবহার করে একজন নেটওয়ার্ক ইঞ্জিনিয়ার তার দৈনন্দিন কাজগুলোকে অটোমেট করতে পারেন - যেমন কনফিগারেশন ব্যাকআপ নেওয়া, মাল্টিপল ডিভাইস মনিটর করা, বা স্ট্যাটাস রিপোর্ট জেনারেট করা।