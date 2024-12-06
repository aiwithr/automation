!!! tldr "মাইক্রোটিক ডিভাইসের অটোমেশন"
      মাইক্রোটিক রাউটারগুলো বেশ কার্যকরী এবং বহুমুখী নেটওয়ার্কিং ডিভাইস। এগুলোর প্রধান ফিচারগুলো হল;

      ১. রাউটিং: এগুলো ইন্টারনেট ট্রাফিক ম্যানেজমেন্ট করে এবং বিভিন্ন নেটওয়ার্কের মধ্যে সহজ কানেক্টিভিটি দেয়।
      ২. ফায়ারওয়াল: নেটওয়ার্ক সিকিউরিটি নিশ্চিত করতে অনুমোদিত ট্রাফিক ম্যানেজ করে।
      ৩. ব্যান্ডউইথ ম্যানেজমেন্ট: ইন্টারনেট গতি নিয়ন্ত্রণ এবং ব্যবহারকারীদের মধ্যে ব্যান্ডউইথ বণ্টন করে।
      ৪. ভিপিএন: বাইরের অফিস বা ব্যবহারকারীদের সাথে নিরাপদ সংযোগ তৈরি করে।
      ৫. হটস্পট: ওয়াই-ফাই হটস্পট ম্যানেজমেন্ট করে, যা ক্যাফে বা হোটেলের জন্য উপযোগী।
      ৬. লোড ব্যালান্সিং: একাধিক ইন্টারনেট সংযোগ ব্যবহার করে নেটওয়ার্কের কার্যক্ষমতা বাড়ায়।
      ৭. কনফিগারেশন: এর নিজস্ব অপারেটিং সিস্টেম RouterOS এর মাধ্যমে বিস্তৃত কনফিগারেশনের সুযোগ দেয়।
      
   এই ক্ষমতাগুলো মাইক্রোটিক রাউটারকে ছোট থেকে বড় ব্যবসা এবং ইন্টারনেট সেবা প্রদানকারীদের জন্য একটি জনপ্রিয় পছন্দ করে তুলেছে। এখানে REST API-এর সুবিধা নিচ্ছি ক্রিপ্টিং এবং অটোমেশনের জন্য চমত্কার ইন্টারফেসের জন্য। JSON ফরম্যাটে ডেটা দেয়, যা সহজে পার্স করা যায়।

!!! info "কোডবেজ পাবেন গিটহাবে"
      বইয়ের পাতার লিমিটেশন থাকাতে নিচের কোডগুলো "স্নিপেট" (অল্প করে) আকারে দেয়া হলো। এই সব এবং বাড়তি সব কোড পাবেন এই https://github.com/raqueeb/network_automation এড্রেসে।

২. API এন্ডপয়েন্ট:

   - `/ip/address`: IP অ্যাড্রেস ম্যানেজমেন্ট
   - `/interface`: ইন্টারফেস ডাটা এবং কনফিগারেশন
   - `/ip/firewall/filter`: ফায়ারওয়াল ফিল্টার ম্যানেজমেন্ট
   - `/system/resource`: সিস্টেম রিসোর্স ডাটা (CPU, মেমরি ব্যবহার)

৩. উদাহরণ API কল: (পোস্টম্যান, কার্ল এবং পাইথনের রিকোয়েস্টস মড্যুল)

শুরুতে পোস্টম্যান দিয়ে এই কাজ করা গেলে অনেক কমপ্লেক্স জিনিস এড়ানো যাবে। তারপর আস্তে আস্তে curl ব্যবহার শুরু করবো। curl একটা 'বেয়ারবোন' কমান্ড লাইন ক্লায়েন্ট, যেটা ডিবাগিংয়ে হেল্প করে। এরপর রেস্ট এপিআই এর জন্য requests মডিউল ব্যবহার করব পাইথন থেকে।

- নতুন IP অ্যাড্রেস যোগ করা:(curl ব্যবহার করে, দেখার জন্য, বোঝার দরকার নেই এই মুহুর্তে)
  
```python
$ curl -k -u rakib:1234 -X PUT https://203.11.91.21/rest/ip/address \
--data '{"address": "192.168.0.11", "interface": "dummy"}' -H "content-type: application/json"
{".id":"*A","actual-interface":"dummy","address":"192.168.0.11/32","disabled":"false",
"dynamic":"false","interface":"dummy","invalid":"false","network":"192.168.0.11"}
```

- পিং টেস্ট করার জন্য:

```bash
$ curl -k -u admin:1234 -X POST https://10.155.101.214/rest/ping \
  --data '{"address":"10.155.101.1","count":"4"}' \
  -H "content-type: application/json"
```

এই কমান্ডগুলো ব্যবহার করার সময় কিছু জিনিস মনে রাখতে হবে:

১. -k অপশন শুধু টেস্টিংয়ের জন্য। প্রোডাকশনে SSL সার্টিফিকেট ঠিক করে সেটআপ করতে হবে।
২. -u admin: এর পরে আপনার পাসওয়ার্ড দিতে হবে। যেমন: -u admin:mypassword
৩. content-type: application/json হেডার দেয়া জরুরি যখন ডাটা পাঠাচ্ছেন।
৪. API কল করার আগে www-ssl সার্ভিস চালু করতে হবে রাউটারে।

এই কমান্ডগুলো দিয়ে আপনি রাউটারের প্রায় সব কিছু কন্ট্রোল করতে পারবেন। শুরুতে টেস্টিং এনভায়রনমেন্টে প্র্যাকটিস করা ভালো, যাতে কোন ভুল হলে প্রোডাকশন এনভায়রনমেন্টে সমস্যা না হয়।

### পাইথন দিয়ে 'রিকোয়েস্টস' মড্যুউল     
```python
response = requests.post(https://203.11.91.21/rest/ip/address', 
                        json={"address":"192.168.1.1/24", "interface":"ether1"},
                        auth=HTTPBasicAuth(username, password), verify=False)
```

৪. সিকিউরিটি প্ল্যানিং:

      - API অ্যাক্সেসের জন্য পৃথক ব্যবহারকারী তৈরি করুন সীমিত 'রাইট' সহ।
      - HTTPS ব্যবহার করুন এবং সঠিক SSL সার্টিফিকেট কনফিগার করুন।
      - API অ্যাক্সেস শুধুমাত্র নির্দিষ্ট IP অ্যাড্রেস থেকে পারমিট করুন।

আপনাকে MikroTik RouterOS-এর কিছু বিশেষ বৈশিষ্ট্য এবং REST API-এর আরও কয়েকটি গুরুত্বপূর্ণ API কল সম্পর্কে কিছু ধারণা দিচ্ছি:

১. ফায়ারওয়াল ম্যানেজমেন্ট:

   API কল: `/ip/firewall/filter`

   উদাহরণ:
   ```python
   # নতুন ফায়ারওয়াল নিয়ম যোগ করা
   web_rule = {
       "chain": "input",
       "protocol": "tcp",
       "dst-port": "80",
       "action": "accept",
       "comment": "Allow HTTP"
   }
   response = requests.post(url+'/ip/firewall/filter/add', json=web_rule, auth=HTTPBasicAuth(username, password), verify=False)
   ```
   এই API কল ব্যবহার করে আপনি ফায়ারওয়াল নিয়ম যোগ, পরিবর্তন বা মুছে ফেলতে পারেন।

২. ব্যান্ডউইথ ম্যানেজমেন্ট:

   API কল: `/queue/simple`

   উদাহরণ:

   ```python
      # নতুন ব্যান্ডউইথ সীমা সেট করা
      new_queue = {
      "name": "Home_Client_Banani",
      "target": "192.168.1.100/32",
      "max-limit": "5M/5M"
   }
   response = requests.post(url + '/queue/simple/add', json=new_queue, auth=HTTPBasicAuth(username, password), verify=False)
   ```

   এই API ব্যবহার করে আপনি ক্লায়েন্টদের জন্য ব্যান্ডউইথ সীমা নির্ধারণ করতে পারেন। এই কলগুলো নিচ্ছে আপনাদের বিলিং/প্যাকেজিং সফটওয়্যার। এখন আমরাও করতে পারবো।

৩. ডাইনামিক DNS আপডেট:
   API কল: `/ip/cloud`
   উদাহরণ:

   ```python
   # MikroTik Cloud DDNS স্ট্যাটাস টেস্ট
   response = requests.get(url+'/ip/cloud', auth=HTTPBasicAuth(username, password), verify=False)
   print(json.dumps(response.json(), indent=4))
   ```
   এই API ব্যবহার করে আপনি MikroTik Cloud DDNS সেবা ম্যানেজমেন্ট করতে পারেন।

৪. ব্যবহারকারী ম্যানেজমেন্ট:
   API কল: `/user`
   উদাহরণ:

   ```python
   # নতুন ব্যবহারকারী তৈরি
   new_user = {
      "name": "rhassan",
      "password": "securepass123",
      "group": "full"
   }
   response = requests.post(base_url+'/user/add', json=new_user, auth=HTTPBasicAuth(username, password), verify=False)
   print(response.text)
   ```
   এই API ব্যবহার করে আপনি নতুন ব্যবহারকারী তৈরি, এক্জিস্টিং ব্যবহারকারী পরিবর্তন বা মুছে ফেলতে পারেন।

৫. সিস্টেম রিসোর্স মনিটরিং:
   API কল: `/system/resource`
   উদাহরণ:

   ```python
   response = requests.get(url+'/system/resource', auth=HTTPBasicAuth(username, password), verify=False)
   resources = response.json()
   print(f"CPU Load: {resources[0]['cpu-load']}%")
   print(f"Free Memory: {resources[0]['free-memory']} bytes")
   ```
   এই API ব্যবহার করে আপনি রাউটারের CPU ব্যবহার, মেমরি ব্যবহার, আপটাইম ইত্যাদি মনিটর করতে পারেন।

৬. ওয়্যারলেস নেটওয়ার্ক ম্যানেজমেন্ট:
   API কল: `/interface/wireless`
   উদাহরণ:

   ```python
   # ওয়্যারলেস নেটওয়ার্কের ডাটা পাওয়া
   response = requests.get(url+'/interface/wireless', auth=HTTPBasicAuth(username, password), verify=False)
   wireless_info = response.json()
   for interface in wireless_info:
       print(f"SSID: {interface['ssid']}, Frequency: {interface['frequency']}")
   ```
   এই API ব্যবহার করে আপনি ওয়্যারলেস নেটওয়ার্ক কনফিগার এবং মনিটর করতে পারেন।

৭. ব্যাকআপ এবং রিস্টোর:
   API কল: `/system/backup`
   উদাহরণ:

   ```python
   # সিস্টেম ব্যাকআপ তৈরি
   response = requests.post(url+'/system/backup/save', json={"name": "daily_backup"}, auth=HTTPBasicAuth(username, password), verify=False)
   ```
   এই API ব্যবহার করে আপনি রাউটারের কনফিগারেশন ব্যাকআপ নিতে এবং পুনরুদ্ধার করতে পারেন।

৮. DHCP সার্ভার ম্যানেজমেন্ট:
   API কল: `/ip/dhcp-server`
   উদাহরণ:

   ```python
   # DHCP লিজ ডাটা পাওয়া
   response = requests.get(url+'/ip/dhcp-server/lease', auth=HTTPBasicAuth(username, password), verify=False)
   leases = response.json()
   for lease in leases:
       print(f"IP: {lease['address']}, MAC: {lease['mac-address']}")
   ```
   এই API ব্যবহার করে আপনি DHCP সার্ভার কনফিগার করতে এবং লিজ ডাটা ম্যানেজমেন্ট করতে পারেন।

এই API কলগুলি ব্যবহার করে আপনি MikroTik রাউটারের প্রায় সব ফাংশন প্রোগ্রাম্যাটিকভাবে নিয়ন্ত্রণ করতে পারেন। এটি বড় নেটওয়ার্ক ম্যানেজমেন্ট, অটোমেশন, এবং কাস্টম মনিটরিং সিস্টেম তৈরির জন্য অত্যন্ত উপযোগী।

### দুটো 'এন্ড-টু-এন্ড' কোড নিয়ে ধারণা

আপনার জন্য কোড এবং আউটপুটের আরও একটি প্রযুক্তিগত ধারণা দেব, আউটপুটের বিস্তারিত বোঝার উপর জোর দিয়ে:

কোডের ধারণা: সব ইন্টারফেস দেখবো ডিটেল লেভেলে

1. লাইব্রেরি ইমপোর্ট:
   ```python
   from requests.auth import HTTPBasicAuth
   import requests
   import json
   ```
   এই লাইনগুলি HTTP অনুরোধ পাঠানো, সিকিউরিটি এবং JSON ডেটা ম্যানেজমেন্টর জন্য প্রয়োজনীয় টুল ইমপোর্ট করে।

2. সিকিউরিটি ডাটা:
   ```python
   username = 'rakib'
   password = '1234'
   ```
   এগুলি MikroTik রাউটারে ঢোকার জন্য ব্যবহৃত হয়।

3. রাউটারের URL:
   ```python
   url = 'https://10.248.27.226/rest'
   ```
   এটি MikroTik রাউটারের REST API এন্ডপয়েন্ট।

4. API অনুরোধ:
   ```python
   response = requests.get(url+'/interface', auth=HTTPBasicAuth(username, password), verify=False)
   ```
   এই লাইনটি রাউটারের '/interface' এন্ডপয়েন্টে একটি GET অনুরোধ পাঠায়, যা সমস্ত নেটওয়ার্ক ইন্টারফেসের ডাটা ফেরত দেয়।

5. রেজাল্ট প্রদর্শন:
   ```python
   print(json.dumps(response.json(), indent=4))
   ```
   এটি API থেকে পাওয়া JSON ডেটা সুন্দরভাবে (ইনডেন্ট করে) ফরম্যাট করে প্রিন্ট করে।

   ```python
        ".id": "*2",
        "actual-mtu": "1500",
        "default-name": "ether1",
        "disabled": "false",
        "mac-address": "50:01:00:05:00:00",
        "mtu": "1500",
        "name": "ether1",
        "running": "true",
        "tx-byte": "9303961",
        "tx-packet": "30177",
        "tx-queue-drop": "0",
        "type": "ether"
   ```

আউটপুট মানে কী:

আউটপুটে দেখা যাচ্ছে যে রাউটারে পাঁচটি ইন্টারফেস রয়েছে: ether1, ether2, ether3, ether4, এবং lo। প্রতিটি ইন্টারফেসের জন্য বিস্তারিত ডাটা দেওয়া আছে। আসুন একটা ইন্টারফেস সম্পর্কে গুরুত্বপূর্ণ ডাটা বিশ্লেষণ করি:

1. ether1:
   - টাইপ: ইথারনেট
   - অবস্থা: চালু ("running": "true")
   - MAC অ্যাড্রেস: 50:01:00:05:00:00
   - সর্বশেষ লিঙ্ক-আপ সময়: 2024-07-26 18:19:19
   - প্রেরিত ডেটা: 9,303,961 বাইট
   - গৃহীত ডেটা: 0 বাইট

   অর্থ কী: এই ইন্টারফেসটি সক্রিয় আছে এবং শুধুমাত্র ডেটা পাঠাচ্ছে, কিন্তু কোনো ডেটা নিচ্ছে না।

### ওয়েব ফায়ারওয়াল রুল (এন্ড টু এন্ড)

উপরের মতো ১ম-৩য় ব্লক একই রকম। সেকারণে, ৪র্থ এবং ৫ম ব্লকগুলো আরও বিস্তারিতভাবে ব্যাখ্যা করছি:

৪র্থ ব্লক:
```
web_rule = {
    "chain": "input",
    "protocol": "tcp",
    "dst-port": "80",
    "action": "accept",
    "comment": "Allow HTTP"
}
```
এটি একটি নতুন ফায়ারওয়াল নিয়ম তৈরি করছে। এখানে প্রতিটি লাইন একটি নির্দিষ্ট কাজ করে:

- "chain": "input" - এর মানে হল এই নিয়মটি রাউটারে আসা সব ট্রাফিকের জন্য প্রযোজ্য।
- "protocol": "tcp" - এটি শুধুমাত্র TCP প্রোটোকল ব্যবহার করা ট্রাফিকের জন্য।
- "dst-port": "80" - এটি 80 নম্বর পোর্টে আসা ট্রাফিকের জন্য, যা সাধারণত ওয়েবসাইট দেখার জন্য ব্যবহৃত হয়।
- "action": "accept" - এর মানে হল এই ধরনের ট্রাফিক অনুমোদন করা হবে।
- "comment": "Allow HTTP" - এটি শুধু একটি মন্তব্য, যা নিয়মটির উদ্দেশ্য বোঝায়।

৫ম ব্লক:

```
response = requests.post(base_url+'/ip/firewall/filter/add', json=web_rule, auth=HTTPBasicAuth(username, password), verify=False)
print(response.text)
```

এই অংশটি কয়েকটি গুরুত্বপূর্ণ কাজ করে:

  1. `requests.post()` ফাংশনটি ব্যবহার করে রাউটারে একটি নতুন ডাটা (POST) পাঠানো হচ্ছে।
  2. `base_url+'/ip/firewall/filter/add'` এই অংশটি বলছে যে আমরা রাউটারের ফায়ারওয়াল ফিল্টারে একটি নতুন নিয়ম যোগ করতে চাই।
  3. `json=web_rule` দ্বারা আমরা আগে তৈরি করা নিয়মটি পাঠাচ্ছি।
  4. `auth=HTTPBasicAuth(username, password)` এর মাধ্যমে রাউটারে লগইন করা হচ্ছে।
  5. `verify=False` বলছে যে রাউটারের সার্টিফিকেট যাচাই করা হবে না (এটি সিকিউরিটির জন্য সাধারণত সুপারিশ করা হয় না)।
  6. `print(response.text)` রাউটার থেকে পাওয়া উত্তর প্রিন্ট করে দেখায়।

এই দুটি ব্লক মিলে, একটি নতুন ফায়ারওয়াল নিয়ম তৈরি করে সেটি রাউটারে পাঠানো হচ্ছে, যা ওয়েবসাইট ট্রাফিক পাস করতে দেবে।

গিটহাব লিঙ্কে অনেক উদাহরন দিয়েছি আমি। প্র্যাকটিস করবেন কিন্তু।


