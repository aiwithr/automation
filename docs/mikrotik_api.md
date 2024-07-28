### ড্রাফট (অনেক এডিট করতে হবে)

!!! list "মাইক্রোটিক ডিভাইস"
   মাইক্রোটিক রাউটারগুলো বেশ কার্যকরী এবং বহুমুখী নেটওয়ার্কিং ডিভাইস। এগুলোর প্রধান ফিচারগুলো হল;

      ১. রাউটিং: এগুলো ইন্টারনেট ট্রাফিক ম্যানেজমেন্ট করে এবং বিভিন্ন নেটওয়ার্কের মধ্যে সহজ কানেক্টিভিটি দেয়।
      ২. ফায়ারওয়াল: নেটওয়ার্ক নিরাপত্তা নিশ্চিত করতে অনুমোদিত ট্রাফিক ম্যানেজ করে।
      ৩. ব্যান্ডউইথ ম্যানেজমেন্ট: ইন্টারনেট গতি নিয়ন্ত্রণ এবং ব্যবহারকারীদের মধ্যে ব্যান্ডউইথ বণ্টন করে।
      ৪. ভিপিএন: বাইরের অফিস বা ব্যবহারকারীদের সাথে নিরাপদ সংযোগ তৈরি করে।
      ৫. হটস্পট: ওয়াই-ফাই হটস্পট ম্যানেজমেন্ট করে, যা ক্যাফে বা হোটেলের জন্য উপযোগী।
      ৬. লোড ব্যালান্সিং: একাধিক ইন্টারনেট সংযোগ ব্যবহার করে নেটওয়ার্কের কার্যক্ষমতা বাড়ায়।
      ৭. কনফিগারেশন: এর নিজস্ব অপারেটিং সিস্টেম RouterOS এর মাধ্যমে বিস্তৃত কনফিগারেশনের সুযোগ দেয়।
      
   এই ক্ষমতাগুলো মাইক্রোটিক রাউটারকে ছোট থেকে বড় ব্যবসা এবং ইন্টারনেট সেবা প্রদানকারীদের জন্য একটি জনপ্রিয় পছন্দ করে তুলেছে।

১. REST API-এর সুবিধা:
      - স্ক্রিপ্টিং এবং অটোমেশনের জন্য চমত্কার ইন্টারফেস দেয়।
      - বহু প্রোগ্রামিং ভাষায় ব্যবহার করা যায়।
      - JSON ফরম্যাটে ডেটা দেয়, যা সহজে পার্স করা যায়।

২. API এন্ডপয়েন্ট:
      - `/ip/address`: IP অ্যাড্রেস ম্যানেজমেন্ট
      - `/interface`: ইন্টারফেস তথ্য এবং কনফিগারেশন
      - `/ip/firewall/filter`: ফায়ারওয়াল ফিল্টার ম্যানেজমেন্ট
      - `/system/resource`: সিস্টেম রিসোর্স তথ্য (CPU, মেমরি ব্যবহার)

৩. HTTP মেথড: (আগেও আলাপ হয়েছে, মাইক্রোটিকের জন্য আলাদা কিছু নয়)
      - GET: তথ্য পড়তে
      - PUT: বিদ্যমান রেকর্ড আপডেট করতে
      - POST: নতুন রেকর্ড তৈরি করতে
      - DELETE: রেকর্ড মুছে ফেলতে

৪. উদাহরণ API কল:
      - নতুন IP অ্যাড্রেস যোগ করা:
     ```python
     response = requests.post(url+'/ip/address', 
                              json={"address":"192.168.1.1/24", "interface":"ether1"},
                              auth=HTTPBasicAuth(username, password), verify=False)
     ```

৫. নিরাপত্তা প্ল্যানিং:
      - API অ্যাক্সেসের জন্য পৃথক ব্যবহারকারী তৈরি করুন সীমিত 'রাইট' সহ।
      - HTTPS ব্যবহার করুন এবং সঠিক SSL সার্টিফিকেট কনফিগার করুন।
      - API অ্যাক্সেস শুধুমাত্র নির্দিষ্ট IP অ্যাড্রেস থেকে পারমিট করুন।

আপনাকে MikroTik RouterOS-এর কিছু বিশেষ বৈশিষ্ট্য এবং REST API-এর আরও কয়েকটি গুরুত্বপূর্ণ API কল সম্পর্কে কিছু ধারণা দিচ্ছি:

১. ফায়ারওয়াল ম্যানেজমেন্ট:
   API কল: `/ip/firewall/filter`
   উদাহরণ:
   ```python
   # নতুন ফায়ারওয়াল নিয়ম যোগ করা
   new_rule = {
       "chain": "input",
       "protocol": "tcp",
       "dst-port": "80",
       "action": "accept",
       "comment": "Allow HTTP"
   }
   response = requests.post(url+'/ip/firewall/filter', json=new_rule, auth=HTTPBasicAuth(username, password), verify=False)
   ```
   এই API কল ব্যবহার করে আপনি ফায়ারওয়াল নিয়ম যোগ, পরিবর্তন বা মুছে ফেলতে পারেন।

২. ব্যান্ডউইথ ম্যানেজমেন্ট:
   API কল: `/queue/simple`
   উদাহরণ:
   ```python
   # নতুন ব্যান্ডউইথ সীমা সেট করা
   new_queue = {
       "name": "Client1",
       "target": "192.168.1.100/32",
       "max-limit": "5M/5M"
   }
   response = requests.post(url+'/queue/simple', json=new_queue, auth=HTTPBasicAuth(username, password), verify=False)
   ```
   এই API ব্যবহার করে আপনি ক্লায়েন্টদের জন্য ব্যান্ডউইথ সীমা নির্ধারণ করতে পারেন।

৩. ডাইনামিক DNS আপডেট:
   API কল: `/ip/cloud`
   উদাহরণ:
   ```python
   # MikroTik Cloud DDNS স্ট্যাটাস পরীক্ষা
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
       "name": "newadmin",
       "password": "securepass123",
       "group": "full"
   }
   response = requests.post(url+'/user', json=new_user, auth=HTTPBasicAuth(username, password), verify=False)
   ```
   এই API ব্যবহার করে আপনি নতুন ব্যবহারকারী তৈরি, বিদ্যমান ব্যবহারকারী পরিবর্তন বা মুছে ফেলতে পারেন।

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
   # ওয়্যারলেস নেটওয়ার্কের তথ্য পাওয়া
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
   # DHCP লিজ তথ্য পাওয়া
   response = requests.get(url+'/ip/dhcp-server/lease', auth=HTTPBasicAuth(username, password), verify=False)
   leases = response.json()
   for lease in leases:
       print(f"IP: {lease['address']}, MAC: {lease['mac-address']}")
   ```
   এই API ব্যবহার করে আপনি DHCP সার্ভার কনফিগার করতে এবং লিজ তথ্য ম্যানেজমেন্ট করতে পারেন।

এই API কলগুলি ব্যবহার করে আপনি MikroTik রাউটারের প্রায় সব ফাংশন প্রোগ্রাম্যাটিকভাবে নিয়ন্ত্রণ করতে পারেন। এটি বড় নেটওয়ার্ক ম্যানেজমেন্ট, অটোমেশন, এবং কাস্টম মনিটরিং সিস্টেম তৈরির জন্য অত্যন্ত উপযোগী।

বেশ, আমি আপনার জন্য কোড এবং আউটপুটের একটি আরও প্রযুক্তিগত ব্যাখ্যা দেব, আউটপুটের বিস্তারিত বোঝার উপর জোর দিয়ে:

কোডের ব্যাখ্যা:

1. লাইব্রেরি ইমপোর্ট:
   ```python
   from requests.auth import HTTPBasicAuth
   import requests
   import json
   ```
   এই লাইনগুলি HTTP অনুরোধ পাঠানো, সিকিউরিটি এবং JSON ডেটা ম্যানেজমেন্টর জন্য প্রয়োজনীয় টুল ইমপোর্ট করে।

2. সিকিউরিটি তথ্য:
   ```python
   username = 'rakib'
   password = '1234'
   ```
   এগুলি MikroTik রাউটারে প্রবেশের জন্য ব্যবহৃত হয়।

3. রাউটারের URL:
   ```python
   url = 'http://10.248.27.226/rest'
   ```
   এটি MikroTik রাউটারের REST API এন্ডপয়েন্ট।

4. API অনুরোধ:
   ```python
   response = requests.get(url+'/interface', auth=HTTPBasicAuth(username, password), verify=False)
   ```
   এই লাইনটি রাউটারের '/interface' এন্ডপয়েন্টে একটি GET অনুরোধ পাঠায়, যা সমস্ত নেটওয়ার্ক ইন্টারফেসের তথ্য ফেরত দেয়।

5. রেজাল্ট প্রদর্শন:
   ```python
   print(json.dumps(response.json(), indent=4))
   ```
   এটি API থেকে প্রাপ্ত JSON ডেটা সুন্দরভাবে ফরম্যাট করে প্রিন্ট করে।

   ```python
   {
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
    },
   ```

আউটপুট মানে কী:

আউটপুটে দেখা যাচ্ছে যে রাউটারে পাঁচটি ইন্টারফেস রয়েছে: ether1, ether2, ether3, ether4, এবং lo। প্রতিটি ইন্টারফেসের জন্য বিস্তারিত তথ্য দেওয়া আছে। আসুন প্রতিটি ইন্টারফেস সম্পর্কে গুরুত্বপূর্ণ তথ্য বিশ্লেষণ করি:

1. ether1:
   - টাইপ: ইথারনেট
   - অবস্থা: চালু ("running": "true")
   - MAC অ্যাড্রেস: 50:01:00:05:00:00
   - সর্বশেষ লিঙ্ক-আপ সময়: 2024-07-26 18:19:19
   - প্রেরিত ডেটা: 9,303,961 বাইট
   - গৃহীত ডেটা: 0 বাইট

   অর্থ কী: এই ইন্টারফেসটি সক্রিয় আছে এবং শুধুমাত্র ডেটা পাঠাচ্ছে, কিন্তু কোনো ডেটা নিচ্ছে না।


