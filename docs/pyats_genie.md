### pyATS এবং Genie কি?

**পাইএটিএস (পাইথন অটোমেশন টেস্ট সিস্টেম)** একটি শক্তিশালী ফ্রেমওয়ার্ক যা নেটওয়ার্ক ডিভাইস এবং সিস্টেমগুলির টেস্টিং, ভেরিফিকেশন এবং অপারেশন করার জন্য তৈরি করা হয়েছে। এটি একটি ওপেন সোর্স প্রজেক্ট যা সিস্কো মেইনটেইন এবং ডেভেলপ করে।

**জেনি** হল পাইএটিএস এর একটি লাইব্রেরি যা নেটওয়ার্ক ডিভাইসগুলির সাথে অটোমেটিকভাবে কানেক্ট, কনফিগারেশন কালেক্ট, এবং বিভিন্ন নেটওয়ার্ক অপারেশন পারফর্ম করতে সাহায্য করে। জেনি ব্যবহার করে, আপনি সহজেই আপনার নেটওয়ার্কের বিভিন্ন ডিভাইসের থেকে ডেটা কালেক্ট এবং অ্যানালাইজ করতে পারেন।

#### কেন পাইএটিএস এবং জেনি ব্যবহার করা প্রয়োজন?

১. **অটোমেশন**: পাইএটিএস এবং জেনি ব্যবহার করে আপনি নেটওয়ার্ক ডিভাইসগুলির টেস্টিং এবং কনফিগারেশন অটোমেট করতে পারেন, যা টাইম এবং এফোর্ট সেভ করে।

২. **অ্যানালাইসিস**: জেনি এর সাহায্যে আপনি নেটওয়ার্ক ডিভাইসগুলির স্টেট এবং পারফরম্যান্স অ্যানালাইজ করতে পারেন, যা ট্রাবলশুটিং-এ হেল্প করে।

৩. **অ্যাকুরেসি**: অটোমেটেড স্ক্রিপ্টগুলি হিউম্যান এরর কমাতে হেল্প করে, যার ফলে আপনার নেটওয়ার্ক ম্যানেজমেন্ট আরো অ্যাকুরেট হয়।

৪. **মনিটরিং**: পাইএটিএস এবং জেনি দিয়ে আপনি রেগুলার নেটওয়ার্ক মনিটরিং করতে পারেন এবং ইস্যুগুলি আগে থেকেই আইডেন্টিফাই করতে পারেন।

### উদাহরণ: এসএলএ (সার্ভিস লেভেল এগ্রিমেন্ট) মনিটরিং

একটি রিয়েল-লাইফ এক্সাম্পল হিসাবে, আমরা দেখবো কীভাবে পাইএটিএস এবং জেনি ব্যবহার করে একটি ইন্টারনেট সার্ভিস প্রোভাইডার (আইএসপি) কাস্টমারের এসএলএ মনিটর করতে পারে।

#### এসএলএ কি?

এসএলএ হল একটি এগ্রিমেন্ট যা সার্ভিস প্রোভাইডার এবং কাস্টমারের মধ্যে করা হয়, যেখানে স্পেসিফিক সার্ভিস স্ট্যান্ডার্ড গ্যারান্টি করা হয়। উদাহরণস্বরূপ, একটি আইএসপি তার কাস্টমারদের স্পেসিফিক টাইমের মধ্যে স্পেসিফিক অ্যামাউন্টে আপটাইম, লেটেন্সি, জিটার, প্যাকেট লস, এবং থ্রুপুট প্রোভাইড করার কমিটমেন্ট দেয়।

#### এসএলএ মনিটরিং কেন ইম্পর্ট্যান্ট?

এসএলএ মনিটরিং করার মাধ্যমে আইএসপি এনশিওর করতে পারে যে তারা তাদের কাস্টমারদের কমিটমেন্ট অনুযায়ী সার্ভিস প্রোভাইড করছে। এটি কাস্টমার স্যাটিসফ্যাকশন মেইনটেইন করতে এবং পোটেনশিয়াল প্রবলেম আগে থেকেই আইডেন্টিফাই করতে হেল্প করে।

### উদাহরণ: টেস্টবেড সেটআপ করা

এই অধ্যায়ে আমরা শিখবো কীভাবে টেস্টবেড ফাইল ক্রিয়েট করতে হয় এবং এটি আমাদের নেটওয়ার্ক ডিভাইসগুলোর সাথে কানেক্ট করতে কীভাবে হেল্প করে।

#### টেস্টবেড ফাইল কি?

টেস্টবেড ফাইল একটি ইয়ামল ফাইল যা আপনার নেটওয়ার্ক ডিভাইসগুলোর ডিটেইলস হোল্ড করে। এতে ডিভাইসের নেম, আইপি অ্যাড্রেস, প্ল্যাটফর্ম টাইপ, এবং লগইন ইনফরমেশন থাকে। পাইএটিএস এই ফাইলের মাধ্যমে ডিভাইসগুলোর সাথে কানেক্ট করে।

#### টেস্টবেড ফাইলের এক্সাম্পল

নীচে একটি টেস্টবেড ফাইলের এক্সাম্পল দেওয়া হল:

```yaml
# isp_testbed.yaml এর একটি উদাহরণ
testbed:
  name: ISP Testbed
  tacacs:
    username: admin
    password: C1sco12345
  devices:
    router1:
      alias: Router1
      os: iosxe
      type: router
      connections:
        defaults:
          class: unicon.Unicon
        vty:
          protocol: ssh
          ip: 192.168.0.1
    router2:
      alias: Router2
      os: iosxe
      type: router
      connections:
        defaults:
          class: unicon.Unicon
        vty:
          protocol: ssh
          ip: 192.168.0.2
    switch1:
      alias: Switch1
      os: iosxe
      type: switch
      connections:
        defaults:
          class: unicon.Unicon
        vty:
          protocol: ssh
          ip: 192.168.0.3
    switch2:
      alias: Switch2
      os: iosxe
      type: switch
      connections:
        defaults:
          class: unicon.Unicon
        vty:
          protocol: ssh
          ip: 192.168.0.4
  credentials:
    default:
      username: admin
      password: C1sco12345
```

### এক্সপ্লেনেশন

- **testbed**: টেস্টবেডের মূল চাবি।
- **name**: টেস্টবেডের নাম।
- **tacacs**: ডিফল্ট ব্যবহারকারীর নাম এবং পাসওয়ার্ড যা ডিভাইসের সাথে সংযোগ স্থাপনের জন্য ব্যবহৃত হয়।
- **devices**: টেস্টবেডের মধ্যে থাকা ডিভাইসগুলোর তালিকা।
  - **alias**: ডিভাইসের জন্য একটি সহজবোধ্য নাম।
  - **os**: ডিভাইসের অপারেটিং সিস্টেম (যেমন, iosxe)।
  - **type**: ডিভাইসের প্রকার (যেমন, router, switch)।
  - **connections**: ডিভাইসের সংযোগের বিবরণ।
    - **defaults**: ডিফল্ট সংযোগ শ্রেণী (`unicon.Unicon` সিস্কো ডিভাইসের জন্য)।
    - **vty**: সংযোগের প্রোটোকল এবং আইপি ঠিকানা (SSH সংযোগের জন্য)।
- **credentials**: ডিফল্ট প্রমাণপত্র যা ডিভাইসের সাথে সংযোগ স্থাপনের জন্য ব্যবহৃত হয়।

### ব্যবহার

এই ফাইলটি তৈরি করে `isp_testbed.yaml` নামে সেভ করুন আপনার কাজে। এরপর, আপনার পাইথন স্ক্রিপ্টে এই ফাইলটি `genie.testbed` মডিউলের `load` ফাংশন দিয়ে লোড করুন।

```python
from genie.testbed import load

# টেস্টবেড ফাইল লোড করা
testbed = load('isp_testbed.yaml')

# টেস্টবেডের সব ডিভাইসের সাথে সংযোগ স্থাপন করা
testbed.connect()
```

এই টেস্টবেড ফাইলটি pyATS এবং Genie কে আপনার নেটওয়ার্কের ডিভাইসগুলোর সাথে সংযোগ স্থাপন এবং পরিচালনা করতে সাহায্য করে।

### একটা উদাহরন: SLA মনিটরিং করা (বিস্তারিত)

এই অধ্যায়ে আমরা আরো বিস্তারিতভাবে জানবো কীভাবে pyATS এবং Genie ব্যবহার করে একটি ইন্টারনেট সার্ভিস প্রোভাইডার (ISP) গ্রাহকের SLA মনিটরিং করতে পারে এবং বিভিন্ন মেট্রিক্স পরিমাপ করতে পারে।

#### SLA কি?

SLA (Service Level Agreement) হল একটি চুক্তি যা সার্ভিস প্রদানকারী এবং গ্রাহকের মধ্যে করা হয়, যেখানে নির্দিষ্ট সার্ভিস মান নিশ্চিত করা হয়। উদাহরণস্বরূপ, একটি ISP তার গ্রাহকদের নির্দিষ্ট সময়ের মধ্যে নির্দিষ্ট পরিমাণে আপটাইম, ল্যাটেন্সি, জিটার, প্যাকেট লস, এবং থ্রুপুট প্রদান করার প্রতিশ্রুতি দেয়।

#### SLA মনিটরিং কেন গুরুত্বপূর্ণ?

SLA মনিটরিং করার মাধ্যমে ISP নিশ্চিত করতে পারে যে তারা তাদের গ্রাহকদের প্রতিশ্রুতি অনুযায়ী সার্ভিস প্রদান করছে। এটি গ্রাহকের সন্তুষ্টি বজায় রাখতে এবং সম্ভাব্য সমস্যা আগে থেকেই চিহ্নিত করতে সাহায্য করে।

#### pyATS এবং Genie ব্যবহার করে SLA মনিটরিং

SLA মনিটরিংের জন্য pyATS এবং Genie ব্যবহার করে নেটওয়ার্ক ডিভাইস থেকে বিভিন্ন মেট্রিক্স সংগ্রহ করা যায়। উদাহরণস্বরূপ, ল্যাটেন্সি, জিটার, প্যাকেট লস, থ্রুপুট, আপটাইম, এবং ব্যান্ডউইথ ব্যবহার টেস্ট করা যেতে পারে।

#### উদাহরণ: ল্যাটেন্সি টেস্ট করা

নীচে একটি উদাহরণ দেওয়া হল যেখানে আমরা ল্যাটেন্সি টেস্ট করছি:

```python
from genie.testbed import load
from genie.utils import Dq

# টেস্টবেড ফাইল লোড করা
testbed = load('isp_testbed.yaml')

# ডিভাইসের সাথে সংযোগ স্থাপন করা
device = testbed.devices['Router1']
device.connect()

# অথবা অনেকগুলো ডিভাইসের সাথে সংযোগ স্থাপন করা
# for device in testbed.devices.values():
#    print(f"Connecting to {device.name}")
#    device.connect()

# পিং কমান্ড চালানো
ping_output = device.ping('8.8.8.8', count=5)

# ল্যাটেন্সি বিশ্লেষণ করা
latency = Dq(ping_output).contains('time=').reconstruct()

# ল্যাটেন্সি প্রদর্শন করা
print('Latency:', latency)
```

### কি বুঝলাম?

- **ping**: পিং কমান্ড চালানো হয় একটি নির্দিষ্ট গন্তব্যে (এখানে 8.8.8.8)।
- **Dq**: Genie এর Dq টুল ব্যবহার করে আমরা পিং আউটপুট থেকে ল্যাটেন্সি সংগ্রহ করি। Dq (Data Query) একটি টুল যা ডাটা থেকে নির্দিষ্ট ডাটা বের করতে সাহায্য করে।
- **Latency**: ল্যাটেন্সি মান বিশ্লেষণ এবং প্রদর্শন করা হয়।

#### অন্যান্য মেট্রিক্স টেস্ট করা

pyATS এবং Genie ব্যবহার করে আমরা অন্যান্য মেট্রিক্স যেমন জিটার, প্যাকেট লস, থ্রুপুট, আপটাইম, এবং ব্যান্ডউইথ ব্যবহার টেস্ট করতে পারি। প্রতিটি মেট্রিক্সের জন্য বিভিন্ন কমান্ড ব্যবহার করা হয়:

1. **জিটার টেস্ট**: 

```python
jitter_output = device.execute('show ip sla statistics')
# জিটার ডাটা বিশ্লেষণ করা
jitter = Dq(jitter_output).contains('Jitter:').reconstruct()
print('Jitter:', jitter)
```

2. **প্যাকেট লস টেস্ট**:

```python
packet_loss_output = device.execute('show interface GigabitEthernet0/0')
# প্যাকেট লস ডাটা বিশ্লেষণ করা
packet_loss = Dq(packet_loss_output).contains('Total output drops:').reconstruct()
print('Packet Loss:', packet_loss)
```

3. **থ্রুপুট টেস্ট**:

```python
throughput_output = device.execute('show policy-map interface GigabitEthernet0/0')
# থ্রুপুট ডাটা বিশ্লেষণ করা
throughput = Dq(throughput_output).contains('offered rate').reconstruct()
print('Throughput:', throughput)
```

4. **আপটাইম টেস্ট**:

```python
uptime_output = device.execute('show version')
# আপটাইম ডাটা বিশ্লেষণ করা
uptime = Dq(uptime_output).contains('uptime is').reconstruct()
print('Uptime:', uptime)
```

5. **ব্যান্ডউইথ ব্যবহার টেস্ট**:

```python
bandwidth_output = device.execute('show interfaces')
# ব্যান্ডউইথ ব্যবহার ডাটা বিশ্লেষণ করা
bandwidth = Dq(bandwidth_output).contains('BW').reconstruct()
print('Bandwidth Usage:', bandwidth)
```

### কি শিখলাম?

আমরা শিখলাম কীভাবে pyATS এবং Genie ব্যবহার করে SLA মনিটরিং করা যায়। এই জিনিস দিয়ে ISP তাদের গ্রাহকদের প্রতিশ্রুত মানের সার্ভিস দিতে পারে এবং সমস্যা আগে থেকেই চিহ্নিত করতে পারে। SLA মনিটরিং করে, ISP তাদের সার্ভিসের মান উন্নত এবং গ্রাহকের সন্তুষ্টি দিতে পারে। আর সেটাই তো টেকনোলজির মানুষ হিসেবে আমরা করতে পারি, তাই না?

### BGP ম্যানেজমেন্ট নিয়ে একটা উদাহরন

BGP প্রোটোকল ব্যবহার করে নেটওয়ার্ক পরিচালনা করতে হলে কিছু সর্বোত্তম পদ্ধতি অনুসরণ করা গুরুত্বপূর্ণ, বিশেষ করে যখন pyATS এবং Genie ব্যবহার করে অটোমেশন এবং মনিটরিং করা হয়। এখানে কিছু সেরা চর্চা (best practices) উল্লেখ করা হলো যা BGP পরিচালনায় pyATS এবং Genie ব্যবহারে সহায়ক হতে পারে:

#### 1. BGP হেলথ চেক (bgp_health_check)

```python
def bgp_health_check(device):
    bgp_summary = device.parse('show ip bgp summary')
    for neighbor in bgp_summary['vrf']['default']['neighbor']:
        state = bgp_summary['vrf']['default']['neighbor'][neighbor]['session_state']
        if state != 'Established':
            print(f"BGP neighbor {neighbor} on {device.name} is not in Established state.")
    return bgp_summary
```

**ব্লক-ব্লক ব্যাখ্যা:**

1. **ফাংশন ঘোষণা:**
    ```python
    def bgp_health_check(device):
    ```
    - `bgp_health_check` নামের ফাংশনটি `device` নামের একটি প্যারামিটার নেয় যা Genie ডিভাইস অবজেক্ট।

2. **BGP সারাংশ পার্স করা:**
    ```python
    bgp_summary = device.parse('show ip bgp summary')
    ```
    - `device.parse` মেথডটি ব্যবহার করে 'show ip bgp summary' কমান্ড চালানো হয় এবং ফলাফলটি `bgp_summary` ভেরিয়েবলে সংরক্ষণ করা হয়।

3. **প্রতিটি নেইবার পার্স করা:**
    ```python
    for neighbor in bgp_summary['vrf']['default']['neighbor']:
        state = bgp_summary['vrf']['default']['neighbor'][neighbor]['session_state']
        if state != 'Established':
            print(f"BGP neighbor {neighbor} on {device.name} is not in Established state.")
    ```
    - `bgp_summary` এর মধ্যে প্রতিটি নেইবার (পিয়ার) লুপ করে পার্স করা হয়।
    - প্রতিটি নেইবারের `session_state` চেক করা হয়।
    - যদি `session_state` 'Established' না হয়, তাহলে একটি বার্তা প্রিন্ট করা হয় যা নেইবার এবং ডিভাইসের নাম দেখায়।

4. **আউটপুট রিটার্ন করা:**
    ```python
    return bgp_summary
    ```
    - পুরো `bgp_summary` আউটপুটটা রিটার্ন করা হয়।

#### 2. BGP কনফিগারেশন যাচাই (verify_bgp_config)

```python
def verify_bgp_config(device):
    bgp_config = device.parse('show run | section bgp')
    expected_config = """
    router bgp 65000
      bgp log-neighbor-changes
      neighbor 192.168.1.2 remote-as 65001
      !
      address-family ipv4
      neighbor 192.168.1.2 activate
      exit-address-family
    """
    if bgp_config != expected_config:
        print(f"BGP configuration on {device.name} does not match the expected configuration.")
```

**ব্লক-ব্লক ব্যাখ্যা:**

1. **ফাংশন ঘোষণা:**
    ```python
    def verify_bgp_config(device):
    ```
    - `verify_bgp_config` ফাংশনটি একটি ডিভাইস অবজেক্ট নেয় যা Genie দ্বারা উপস্থাপিত।

2. **BGP কনফিগারেশন পার্স করা:**
    ```python
    bgp_config = device.parse('show run | section bgp')
    ```
    - `device.parse` মেথডটি ব্যবহার করে 'show run | section bgp' কমান্ড চালানো হয় এবং ফলাফলটি `bgp_config` ভেরিয়েবলে সংরক্ষণ করা হয়।

3. **প্রত্যাশিত কনফিগারেশন সংজ্ঞায়িত করা:**
    ```python
    expected_config = """
    router bgp 65000
      bgp log-neighbor-changes
      neighbor 192.168.1.2 remote-as 65001
      !
      address-family ipv4
      neighbor 192.168.1.2 activate
      exit-address-family
    """
    ```
    - `expected_config` ভেরিয়েবলে একটি স্ট্রিং হিসেবে প্রত্যাশিত BGP কনফিগারেশন সংরক্ষণ করা হয়।

4. **কনফিগারেশন মিলিয়ে দেখা:**
    ```python
    if bgp_config != expected_config:
        print(f"BGP configuration on {device.name} does not match the expected configuration.")
    ```
    - যদি `bgp_config` এবং `expected_config` এর মধ্যে মিল না থাকে, তাহলে একটি বার্তা প্রিন্ট করা হয় যা কনফিগারেশন মিসম্যাচ দেখায়।

#### 3. BGP পিয়ার মনিটরিং (monitor_bgp_peers)

```python
def monitor_bgp_peers(device):
    bgp_neighbors = device.parse('show ip bgp neighbors')
    for neighbor, details in bgp_neighbors['vrf']['default']['neighbor'].items():
        if details['session_state'] != 'Established':
            print(f"BGP peer {neighbor} on {device.name} is not Established.")
            print(f"State: {details['session_state']}")
            print(f"Received Prefixes: {details['bgp_negotiated_capabilities']['bgp_received_prefixes']}")
```

**ব্লক-ব্লক ব্যাখ্যা:**

1. **ফাংশন ঘোষণা:**
    ```python
    def monitor_bgp_peers(device):
    ```
    - `monitor_bgp_peers` ফাংশনটি একটি ডিভাইস অবজেক্ট নেয়।

2. **BGP নেবার ডাটা পার্স করা:**
    ```python
    bgp_neighbors = device.parse('show ip bgp neighbors')
    ```
    - `device.parse` মেথডটি ব্যবহার করে 'show ip bgp neighbors' কমান্ড চালানো হয় এবং ফলাফলটি `bgp_neighbors` ভেরিয়েবলে সংরক্ষণ করা হয়।

3. **প্রতিটি নেবার চেক করা:**
    ```python
    for neighbor, details in bgp_neighbors['vrf']['default']['neighbor'].items():
        if details['session_state'] != 'Established':
            print(f"BGP peer {neighbor} on {device.name} is not Established.")
            print(f"State: {details['session_state']}")
            print(f"Received Prefixes: {details['bgp_negotiated_capabilities']['bgp_received_prefixes']}")
    ```
    - `bgp_neighbors` এর প্রতিটি নেবার লুপ করে পার্স করা হয়।
    - যদি `session_state` 'Established' না হয়, তাহলে একটি বার্তা প্রিন্ট করা হয় যা নেবার, তার স্থিতি এবং পাওয়া প্রিফিক্স দেখায়।

#### 4. অ্যালার্ম এবং রিপোর্টিং (bgp_alert_check)

```python
import smtplib
from email.mime.text import MIMEText

def send_email_alert(subject, message):
    msg = MIMEText(message)
    msg['Subject'] = subject
    msg['From'] = 'alert@link3.net'
    msg['To'] = 'admin@link3.net'
    
    with smtplib.SMTP('smtp.link3.net') as server:
        server.sendmail(msg['From'], [msg['To']], msg.as_string())

def bgp_alert_check(device):
    bgp_summary = device.parse('show ip bgp summary')
    for neighbor in bgp_summary['vrf']['default']['neighbor']:
        state = bgp_summary['vrf']['default']['neighbor'][neighbor]['session_state']
        if state != 'Established':
            send_email_alert(f"BGP Alert on {device.name}", f"BGP neighbor {neighbor} is in state {state}")
```

**ব্লক-ব্লক ব্যাখ্যা:**

1. **ইমেল পাঠানোর ফাংশন:**
    ```python
    import smtplib
    from email.mime.text import MIMEText

    def send_email_alert(subject, message):
        msg = MIMEText(message)
        msg['Subject'] = subject
        msg['From'] = 'alert@link3.net'
        msg['To'] = 'admin@link3.net'
        
        with smtplib.SMTP('smtp.link3.net') as server:
            server.sendmail(msg['From'], [msg['To']], msg.as_string())
    ```
    - `smtplib` এবং `MIMEText` মডিউলগুলো ইমেল পাঠানোর জন্য আমদানি করা হয়।
    - `send_email_alert` ফাংশনটি ইমেলের সাবজেক্ট এবং মেসেজ নেয় এবং ইমেল পাঠায়।
    - `MIMEText` ব্যবহার করে মেসেজ তৈরি করা হয় এবং SMTP সার্ভার ব্যবহার করে ইমেল পাঠানো হয়।

2. **BGP অ্যালার্ম চেক:**
    ```python
    def bgp_alert_check(device):
        bgp_summary = device.parse('show ip bgp summary')
        for neighbor in bgp_summary['vrf']['default']['neighbor']:
            state = bgp_summary['vrf']['default']['neighbor'][neighbor]['session_state']
            if state != 'Established':
                send_email_alert(f"BGP Alert on {device.name}", f"BGP neighbor {neighbor} is in state {state}")
    ```
    - `bgp_alert_check` ফাংশনটি একটি ডিভাইস অবজেক্ট নেয়।
    - `device.parse` মেথডটি ব্যবহার করে 'show ip bgp summary' কমান্ড চালানো হয় এবং ফলাফলটি `bgp_summary` ভেরিয়েবলে সংরক্ষণ করা হয়।
    - প্রতিটি নেবারের `session_state` চেক করা হয়।
    - যদি `session_state` 'Established' না হয়, তাহলে `send_email_alert

` ফাংশনটি ব্যবহার করে ইমেইলে\সিগন্যালে (আপনারা করবেন) অ্যালার্ম পাঠানো হয়।

#### 5. নিয়মিত ব্যাকআপ এবং রোলব্যাক (backup_bgp_config এবং restore_bgp_config)

```python
def backup_bgp_config(device):
    bgp_config = device.execute('show run | section bgp')
    with open(f"{device.name}_bgp_backup.cfg", 'w') as f:
        f.write(bgp_config)

def restore_bgp_config(device):
    with open(f"{device.name}_bgp_backup.cfg", 'r') as f:
        bgp_config = f.read()
    device.configure(bgp_config)
```

**ব্লক-ব্লক ব্যাখ্যা:**

1. **ব্যাকআপ ফাংশন:**
    ```python
    def backup_bgp_config(device):
        bgp_config = device.execute('show run | section bgp')
        with open(f"{device.name}_bgp_backup.cfg", 'w') as f:
            f.write(bgp_config)
    ```
    - `backup_bgp_config` ফাংশনটি একটি ডিভাইস অবজেক্ট নেয়।
    - `device.execute` মেথডটি ব্যবহার করে 'show run | section bgp' কমান্ড চালানো হয় এবং ফলাফলটি `bgp_config` ভেরিয়েবলে সংরক্ষণ করা হয়।
    - ব্যাকআপ ফাইল `device.name_bgp_backup.cfg` নামে তৈরি করা হয় এবং এতে কনফিগারেশন লেখা হয়।

2. **রোলব্যাক ফাংশন:**
    ```python
    def restore_bgp_config(device):
        with open(f"{device.name}_bgp_backup.cfg", 'r') as f:
            bgp_config = f.read()
        device.configure(bgp_config)
    ```
    - `restore_bgp_config` ফাংশনটি একটি ডিভাইস অবজেক্ট নেয়।
    - ব্যাকআপ ফাইল `device.name_bgp_backup.cfg` থেকে পড়া হয়।
    - `device.configure` মেথডটি ব্যবহার করে ব্যাকআপ কনফিগারেশন ডিভাইসে প্রয়োগ করা হয়।

### উপসংহার

pyATS এবং Genie ব্যবহার করে BGP পরিচালনার এই সেরা চর্চাগুলি নেটওয়ার্কের স্থিতিশীলতা এবং কার্যকারিতা বজায় রাখতে সাহায্য করে। নিয়মিত স্বাস্থ্য পরীক্ষা, কনফিগারেশন যাচাই, পিয়ার মনিটরিং, অ্যালার্ম এবং রিপোর্টিং এবং নিয়মিত ব্যাকআপ ও রোলব্যাকের মাধ্যমে একটি সুস্থ ও কার্যকরী BGP নেটওয়ার্ক পরিচালনা করা সম্ভব।















