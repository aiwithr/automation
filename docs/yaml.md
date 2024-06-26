#### ওয়াইএএমএল (YAML)

যে কোন সিস্টেমের কনফিগারেশন ফাইল ঠিকমত লিখতে গেলে YAML আমার লিস্টে প্রথম দিকেই থাকে। এই ফরমেটটা এতটাই হিউম্যান রিডেবল, যে সবাই এটাকে পছন্দ করে। যেকোনো মার্কআপ ল্যাঙ্গুয়েজ হিসেবে মার্ক ডাউন, ‘ইয়েট এনাদার মার্কআপ ল্যাঙ্গুয়েজ’ হিসেবে আসলেও এর ফলাফল খুবই ভালো। এই ফরম্যাট দিয়ে একদম জিরো থেকে একটা ফাংশনাল অটোমেশন ওয়ার্ক ফ্লু তৈরি করা যায় যার মাধ্যমে আপনি ডিফাইন করতে পারবেন কি ধরনের ডাটা আপনি ডিভাইসে পুশ করতে চাইছেন। দিন শেষে এই ধরনের ফরম্যাট আমাদেরকে সাহায্য করে কত সহজে বিভিন্ন কনফিগারেশন ডাটা ডিভাইসে দরকার মতো পুশ করতে। এটাকে অনেক সময় সিরিয়ালাইজেশন ল্যাঙ্গুয়েজ বলা হয় যাতে একটা ফাইল বা ডাটা স্ট্রিমে কিভাবে একটার পর একটা কনফিগারেশন পুশ করতে হয় সেটা দেখব সামনেই। 

আমি কয়েকটা ছোট উদাহরণ নিয়ে আসি যাতে আপনাদের বুঝতে সুবিধা হয়। ওয়াইএএমএল ফাইলের শুরুতে তিনটা হাইফেন দিয়ে বোঝানো হয় যে ডকুমেন্ট শুরু হচ্ছে। তিনটা ডট দিয়ে একটা ডকুমেন্ট শেষ করার ইন্ডিকেশন দেওয়া হয় যদিও এখানে সেটা আমরা দেখাইনি কারণ এটা একটাই ডকুমেন্ট। ওয়াইএমএল ফাইল এর মধ্যে একটা বড় সুবিধা হচ্ছে এখানে কোটেশন বা ডাবল কোটেশন ব্যবহার নেই বললেই চলে। স্ট্রিং হিসেবে ডিক্লেয়ার করার জন্য কোন কোটেশন দরকার হয় না। পাই-ওয়াইএমএল পার্সার সুন্দরভাবে এগুলোকে পার্স করতে পারে। আমাদের এখানে একটা লিস্ট নিয়ে এসেছি, কয়েকটা আইটেম তবে কিছুটা ভিন্ন ধরনের। আইটেম শুরু করতে হয় হাইফেন দিয়ে, যাতে লিস্ট হিসেবে বোঝা যায়। নেম, ডেসক্রিপশন হচ্ছে স্ট্রিং টাইপ। সংখ্যাগুলো আসলে ইনটেজার টাইপ। এনাবলড একটা বুলিয়ান, যেটা ট্রু ফলস হতে পারে। তবে, আইটেমের একটা লিস্ট এর মধ্যে আরেকটা লিস্ট এটা নেস্টেড, কারণ একটার মধ্যে আর একটা লিস্ট ঢুকে যেতে পারে।

```python
---
# example.yml file
interface:
  name: GigabitEthernet2
  description: Wide Area Network
  enabled: true
  ipv4:
    address:
    - ip: 172.16.0.2
      netmask: 255.255.255.0
```

### পাইথনের কি ভ্যালু পেয়ারের মত করে ডিকশনারি

এখানে আমাদের ‘কি’ গুলোকে স্ট্রিং হিসেবে দেখানো আছে কোলনের বা পাশে। ডান পাশে তার করেসপন্ডিং ভ্যালু দেখানো আছে। আমরা যদি পাইথনে এই ভ্যালুগুলোকে লুকআপ করতে যেতাম তাহলে এটার সাথে করেসপন্ডিং কি কে রেফারেন্স করতাম। এই একই yaml dictionary যেটা লিস্ট এর মত দেখা যায় সেটাকে বেশ কয়েক ভাবে লেখা যেতে পারে। ডাটা রিপেজেনসন স্ট্যান্ড পয়েন্ট থেকে আগের উদাহরণ আমরা বেশ কয়েকভাবে লিখতে পারি। ইন্টারনেটে উদাহরণে সেটা দেখবেন।

সব পার্সার সবগুলোকে উদাহরণকেই ঠিক মতো রিড করবে, তবে উপরেরটা বেশি হিউম্যান রিডেবল। আমাদেরকে হিউম্যান রিডেবল অংশটাকে প্রায়োরিটি দিতে হয় কারণ নিজের কোড অন্যদেরকেও তো বুঝতে হবে। অন্য প্রোগ্রামিং ল্যাঙ্গুয়েজ এর মত এখানে হ্যাশ ব্যবহার করা হয় যদি কিছু কমেন্ট আকারে দিতে চাই। আমি যেহেতু বই লেখার সময় প্রচুর মার্ক ডাউন ব্যবহার করি সে কারণে ওয়াইমেল এটা একটা ফ্রেন্ডলি ওয়ে হতে পারে যাতে মানুষ বিভিন্ন অটোমেশন সিস্টেম ঠিকমতো বুঝতে পারে।

#### পাইথনের ভিতরে ওয়াইএমএলের ব্যবহার

```python
import yaml

# Open the sample yaml file and read it into variable
with open("yaml_example.yaml") as f:
    yaml_example = f.read()
```

পাইথন খুব সহজে ওয়াইএএমএল ফাইলকে পাইথন অবজেক্টে কনভার্ট করতে পারে। তখন ওয়াইমেল অবজেক্টগুলো ডিকশনারি হয়ে যায়। ওয়াইম েল লিস্টগুলো তখন পাইথনের লিস্ট হয়ে যায়। যেমন, yaml.load(yaml_data), yaml.dump(object)। সবচেয়ে নিচের লাইনটা দেখুন।

```python
# Import the yamltodict library
import yaml

# Open the sample yaml file and read it into variable
with open("yaml_example.yaml") as f:
    yaml_example = f.read()

# Parse the yaml into a Python dictionary
yaml_dict = yaml.load(yaml_example)

# Save the interface name into a variable
int_name = yaml_dict["interface"]["name"]

# Change the IP address of the interface
yaml_dict["interface"]["ipv4"]["address"][0]["ip"] = "192.168.0.2"

# Revert to the yaml string version of the dictionary
print(yaml.dump(yaml_dict, default_flow_style=False))
```
