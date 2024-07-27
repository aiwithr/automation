### ড্রাফট (অনেক এডিট করতে হবে)

আমি আপনাকে MikroTik RouterOS এবং এর REST API সম্পর্কে আরও কিছু গুরুত্বপূর্ণ তথ্য দিচ্ছি:

১. MikroTik RouterOS সম্পর্কে:
   - এটি একটি লিনাক্স-ভিত্তিক অপারেটিং সিস্টেম যা বিশেষভাবে নেটওয়ার্ক ডিভাইসগুলির জন্য ডিজাইন করা হয়েছে।
   - এটি রাউটিং, ফায়ারওয়াল, ব্যান্ডউইথ পরিচালনা, ওয়্যারলেস অ্যাক্সেস পয়েন্ট, ব্যাকবোন লিংক, ইন্টারনেট গেটওয়ে, ভিপিএন সার্ভার, ইত্যাদি হিসেবে ব্যবহৃত হয়।

২. REST API-এর সুবিধা:
   - স্ক্রিপ্টিং এবং অটোমেশনের জন্য সহজ ইন্টারফেস প্রদান করে।
   - বহু প্রোগ্রামিং ভাষায় ব্যবহার করা যায়।
   - JSON ফরম্যাটে ডেটা প্রদান করে, যা সহজে পার্স করা যায়।

৩. API এন্ডপয়েন্ট:
   - `/ip/address`: IP ঠিকানা পরিচালনা
   - `/interface`: ইন্টারফেস তথ্য এবং কনফিগারেশন
   - `/ip/firewall/filter`: ফায়ারওয়াল নিয়ম পরিচালনা
   - `/system/resource`: সিস্টেম রিসোর্স তথ্য (CPU, মেমরি ব্যবহার)

৪. HTTP মেথড:
   - GET: তথ্য পড়তে
   - PUT: বিদ্যমান রেকর্ড আপডেট করতে
   - POST: নতুন রেকর্ড তৈরি করতে
   - DELETE: রেকর্ড মুছে ফেলতে

৫. উদাহরণ API কল:
   - নতুন IP ঠিকানা যোগ করা:
     ```python
     response = requests.post(url+'/ip/address', 
                              json={"address":"192.168.1.1/24", "interface":"ether1"},
                              auth=HTTPBasicAuth(username, password), verify=False)
     ```

৬. নিরাপত্তা বিবেচনা:
   - API অ্যাক্সেসের জন্য পৃথক ব্যবহারকারী তৈরি করুন সীমিত অধিকার সহ।
   - HTTPS ব্যবহার করুন এবং সঠিক SSL সার্টিফিকেট কনফিগার করুন।
   - API অ্যাক্সেস শুধুমাত্র নির্দিষ্ট IP ঠিকানা থেকে অনুমোদন করুন।

আমি আপনাকে MikroTik RouterOS-এর কিছু বিশেষ বৈশিষ্ট্য এবং REST API-এর আরও কয়েকটি গুরুত্বপূর্ণ API কল সম্পর্কে বিস্তারিত তথ্য দিচ্ছি:

১. ফায়ারওয়াল পরিচালনা:
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

২. ব্যান্ডউইথ পরিচালনা:
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
   এই API ব্যবহার করে আপনি MikroTik Cloud DDNS সেবা পরিচালনা করতে পারেন।

৪. ব্যবহারকারী পরিচালনা:
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

৬. ওয়্যারলেস নেটওয়ার্ক পরিচালনা:
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

৮. DHCP সার্ভার পরিচালনা:
   API কল: `/ip/dhcp-server`
   উদাহরণ:
   ```python
   # DHCP লিজ তথ্য পাওয়া
   response = requests.get(url+'/ip/dhcp-server/lease', auth=HTTPBasicAuth(username, password), verify=False)
   leases = response.json()
   for lease in leases:
       print(f"IP: {lease['address']}, MAC: {lease['mac-address']}")
   ```
   এই API ব্যবহার করে আপনি DHCP সার্ভার কনফিগার করতে এবং লিজ তথ্য পরিচালনা করতে পারেন।

এই API কলগুলি ব্যবহার করে আপনি MikroTik রাউটারের প্রায় সব ফাংশন প্রোগ্রাম্যাটিকভাবে নিয়ন্ত্রণ করতে পারেন। এটি বড় নেটওয়ার্ক পরিচালনা, অটোমেশন, এবং কাস্টম মনিটরিং সিস্টেম তৈরির জন্য অত্যন্ত উপযোগী।

বেশ, আমি আপনার জন্য কোড এবং আউটপুটের একটি আরও প্রযুক্তিগত ব্যাখ্যা দেব, আউটপুটের বিস্তারিত বোঝার উপর জোর দিয়ে:

কোডের ব্যাখ্যা:

1. লাইব্রেরি আমদানি:
   ```python
   from requests.auth import HTTPBasicAuth
   import requests
   import json
   ```
   এই লাইনগুলি HTTP অনুরোধ পাঠানো, প্রমাণীকরণ এবং JSON ডেটা পরিচালনার জন্য প্রয়োজনীয় টুল আমদানি করে।

2. প্রমাণীকরণ তথ্য:
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

5. ফলাফল প্রদর্শন:
   ```python
   print(json.dumps(response.json(), indent=4))
   ```
   এটি API থেকে প্রাপ্ত JSON ডেটা সুন্দরভাবে ফরম্যাট করে প্রিন্ট করে।

```
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

আউটপুটের ব্যাখ্যা:

আউটপুটে দেখা যাচ্ছে যে রাউটারে পাঁচটি ইন্টারফেস রয়েছে: ether1, ether2, ether3, ether4, এবং lo। প্রতিটি ইন্টারফেসের জন্য বিস্তারিত তথ্য দেওয়া আছে। আসুন প্রতিটি ইন্টারফেস সম্পর্কে গুরুত্বপূর্ণ তথ্য বিশ্লেষণ করি:

1. ether1:
   - টাইপ: ইথারনেট
   - অবস্থা: চালু ("running": "true")
   - MAC ঠিকানা: 50:01:00:05:00:00
   - সর্বশেষ লিঙ্ক-আপ সময়: 2024-07-26 18:19:19
   - প্রেরিত ডেটা: 9,303,961 বাইট
   - গৃহীত ডেটা: 0 বাইট

   বিশ্লেষণ: এই ইন্টারফেসটি সক্রিয় আছে এবং শুধুমাত্র ডেটা প্রেরণ করছে, কিন্তু কোনো ডেটা গ্রহণ করছে না।


