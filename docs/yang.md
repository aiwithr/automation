### ইয়াং ডাটা মডেলিং ল্যাংগুয়েজ (YANG Data Modeling Language)

নেটওয়ার্ক অটোমেশনের কাজে ইয়াং (YANG) ডাটা মডেল একটা অসাধারণ এবং সেন্ট্রালাইজড পদ্ধতি প্রদান করে, যার মাধ্যমে নেটওয়ার্ক ডিভাইসগুলোকে এক জায়গা থেকে কনফিগার এবং বিভিন্ন অপারেশনাল ডাটা সংগ্রহ করা যায়। আগে যে কাজগুলো কমান্ড লাইন ইন্টারফেস বা এসএনএমপি দিয়ে করা হত, সেগুলো এখন ডাটা মডেলের মাধ্যমে সরাসরি নেটওয়ার্কের ডিভাইসের ম্যানেজমেন্ট প্লেনের সাথে যোগাযোগ করে সহজেই সম্পন্ন করা যায়। 

এ ধরনের ডাটা মডেল স্ট্যান্ডার্ড হওয়ায় বিভিন্ন ধরনের নেটওয়ার্কিং ডিভাইসের সাথে একই ধরনের প্রসিডিউর ব্যবহার করা সম্ভব। এটি নেটওয়ার্ক সার্ভিস প্রোভাইডারদের জন্য বিশেষভাবে উপকারী, কারণ তারা বিভিন্ন কোম্পানির নেটওয়ার্কিং ডিভাইসকে এক ডাটা মডেলের মধ্যে এনে কন্ট্রোল করতে পারে।

এই উদ্দেশ্যে একটা ল্যাপটপ, কম্পিউটার বা সার্ভার ব্যবহার করে নেটওয়ার্কের সব ডিভাইসকে কনফিগার করা বা ডাটা সংগ্রহ করা যায়। বিশেষ করে অপারেশনাল ডাটা কালেকশন করে কাজগুলো সহজ করা সম্ভব। এখানে ডাটা মডেল বিভিন্ন সফটওয়্যার সিস্টেম এবং পাইথনের বিভিন্ন লাইব্রেরি ব্যবহার করে নেটওয়ার্ককে সহজে অটোমেট করতে পারে।

### ইয়েট অ্যানাদার নেক্সট জেনারেশন (YANG) ডাটা মডেলিং ল্যাংগুয়েজ (RFC 6020)

ইয়াং মডেল এখন ডাটা ল্যাংগুয়েজ মডেলের মধ্যে স্ট্যান্ডার্ড হয়ে গেছে। বিভিন্ন ভেন্ডরের নেটওয়ার্কিং ইকুইপমেন্টের মধ্যে বিভিন্ন ধরনের ডিভাইস কনফিগারেশন রিকোয়েস্ট এবং অপারেশনাল ডাটা, বিশেষ করে শো কমান্ড, সেন্ট্রালাইজড ভাবে সম্পন্ন করা যায়। এর মধ্যে যে স্ট্রাকচার তৈরি হয়েছে, কম্পিউটার প্রোগ্রামগুলো একে অপরের সাথে এক্সএমএল ফর্ম্যাটে ডাটা আদান-প্রদান করতে পারে, যা যন্ত্র থেকে যন্ত্রের মধ্যে যোগাযোগকে অনেক সহজ করে তোলে।

যদিও মানুষ এক্সএমএল ফরম্যাট বুঝতে কিছুটা কষ্ট পায়, তবুও এটি প্রয়োজনীয় কারণ যন্ত্রগুলোর মধ্যে যোগাযোগকে বোঝা গুরুত্বপূর্ণ। অনেক ধরনের অ্যাপ্লিকেশন এখন তৈরি হয়েছে, যেগুলো একটা সেন্ট্রালাইজড ম্যানেজমেন্ট প্লাটফর্ম থেকে পরিচালিত হতে পারে, যার মাধ্যমে বিভিন্ন ধরনের কনফিগারেশন এবং অপারেশনাল ডাটা রিকোয়েস্ট করা যায়।

আমরা আগেও ইয়াং মডেলের মধ্যে নেটকনফ এবং রেস্টকনফ নিয়ে আলোচনা করেছি, যেগুলো প্রায় সকল ভেন্ডরের মধ্যে চমৎকারভাবে কাজ করে। এখানে আমরা দেখব কিভাবে নিজস্ব মডেল এবং এর পাশাপাশি আইইটিএফ মডেল সাপোর্ট করে। এই দুই ধরনের ডাটা মডেলই কনফিগারেশন এবং অপারেশনাল ডাটা সংগ্রহের জন্য চমৎকারভাবে কাজ করে। যেহেতু এই মডেলগুলো যন্ত্রগুলোর মধ্যে যোগাযোগের জন্য ব্যবহৃত হয়, তাই এদের মধ্যে রিমোট প্রসিডিউর কল অথবা এইচটিটিপি ওয়েব সার্ভিস যা-ই ব্যবহার করা হোক না কেন, আমরা দেখব কিভাবে এক্সএমএল নিয়ে কাজ করা যায়।

### ইয়াং (YANG) মডেল এবং রিমোট প্রোসিজিউর কল

এদিকে, ইয়াং (YANG) মডেলগুলো আবার API এর মত নয়। আপনি YANG ব্যবহার করে ডেটা মডেল তৈরি করতে পারেন যা API দিয়ে ব্যবহার করা যায়। আগেও বলেছি, YANG এর ফুল নেম হল Yet Another Next Generation Model। এটা কনফিগারেশন ম্যানেজমেন্টের জন্য বিশেষভাবে তৈরি একটা ডেটা মডেল। আরেকটি উপায় হল gRPC রিমোট প্রোসিজার কল। গুগল প্রথম এই ফ্রেমওয়ার্কটি বানিয়েছিল বলে এই নামের g গুগলকে পয়েন্ট করে। যেহেতু gRPC একটা ওপেন-সোর্স প্রকল্প, এটি অনেক কোম্পানির দ্বারা নেটওয়ার্ক অটোমেশনের জন্য ব্যবহৃত হয়। নেটওয়ার্ক অটোমেশন টুলগুলো এই API এবং ডেটা ফ্রেমওয়ার্ক ব্যবহার করে নেটওয়ার্ক ডিভাইস এবং সার্ভিসগুলোর সাথে কথা বলে। সিসকো থেকে শুরু করে পৃথিবীর সব বড় বড় ডিভাইস ম্যানুফ্যাকচারার এবং মাইক্রোটিক তাদের ডিভাইসগুলো API সক্ষম করার জন্য কাজ করছে যাতে অটোমেশন আরও সহজে করা যায়। 

APIগুলো প্রোগ্রামেবিলিটির একটা গুরুত্বপূর্ণ অংশ, তবে এটি শুধুমাত্র কয়েকটা দিক কাভার করে। এগুলো ডিভাইসের প্রোগ্রামেবিলিটিকে এনাবল করে। এর অর্থ হচ্ছে, যেকোনো সফটওয়্যার অথবা কন্ট্রোলার তার নেটওয়ার্কের বাকি সব ডিভাইস গুলোকে এপিআই এর মাধ্যমে কন্ট্রোল করতে পারে। আমরা ধরে নিতে পারি সফটওয়্যার নিজে থেকেই অন্যান্য ডিভাইসের সাথে খুব সহজেই এই এপিআইগুলো দিয়ে কল করে নিজ নিজ কাজ শেষ করতে পারে। তবে এখানে আরও অনেক উপাদান রয়েছে যা এন্ড টু এন্ড প্রয়োজনীয় একটা কাজ সম্পাদন করার জন্য ব্যবহার হয়। এদিকে সফটওয়্যার এর ধারণায় ডেভঅপস, যা ডেভেলপমেন্ট এবং অপারেশনসের ছোট রূপ, এটা সিস্টেম এবং ইনফ্রাস্ট্রাকচার অর্থাৎ অবকাঠামোতে ডেভেলপার ধারণাগুলো নিয়ে আসার জন্য কাজ করে। এই ধারণাগুলোর কারণ হল যে তারা সফটওয়্যার এর মতো করে  কন্ট্রোলার ধরে ধরে প্রতিটা ডিভাইসে কিভাবে  কনফিগারেশনগুলোকে কার্যকর এবং দ্রুত প্রসেসে চালু করে ডেপ্লয়মেন্ট, রক্ষণাবেক্ষণ এবং ক্রমাগত উন্নতির জন্য কাজ করতে থাকে।

### অ্যাপ্লিকেশন প্রোগ্রামিং ইন্টারফেস, REST কনফিগারেশন প্রোটোকল (RESTCONF)

নেটওয়ার্ক প্রোগ্রামিং এখন অ্যাপ্লিকেশন প্রোগ্রামিং ইন্টারফেস (API) ব্যবহার করার দিকে হাঁটছে এবং কমান্ড লাইন ইন্টারফেস (CLI) থেকে দূরে সরে যাচ্ছে। তবে, আমরা CLI ভুলে যাবো না, কারণএটা অনেক সময়ই দরকার হয়। বিশেষ করে গুরুতর সমস্যার সমাধান করার প্রক্রিয়ায় CLI দরকার হতে পারে। তবে যেগুলো পুরনো ডিভাইস যার মধ্যে সিএলআই আছে, সেখানে secure socket অর্থাৎ এস এস এইচ দিয়ে অটোমেশনের জন্য অনেক লাইব্রেরী চলে এসেছে যার মধ্যে কন্ট্রোলার সরাসরি যোগাযোগ করতে পারে এ ধরনের পুরনো ডিভাইসে, যেগুলো শুধুমাত্র সি এল আই সাপোর্ট করতো। এর অর্থ হচ্ছে সিএলআই সাপোর্টেড ডিভাইস অটোমেশনে যোগদান করতে পারবে না এটা সত্য নয়। গত ১০ বছরের অভিজ্ঞতায়, বরং এ ধরনের ডিভাইস গুলোকেই সবচেয়ে বেশি যুক্ত করেছি অটোমেশনে। তবে, API ছাড়া নেটওয়ার্ক অটোমেশন বিশেষ করে সফটওয়্যার দিয়ে নেটওয়ার্ক চালানো না গেলে আমাদের বিশাল নেটওয়ার্ক এখন চালানো সম্ভব হচ্ছে না শুধুমাত্র মানুষের সাহায্য নিয়ে।

নেটওয়ার্ক ডিভাইস এবং কন্ট্রোলারের (যেটা একটা সফটওয়্যার) সাথে বিভিন্ন ধরনের API ব্যবহার করা হয়। RESTful বা REST API, যা রিপ্রেজেন্টেশনাল স্টেট ট্রান্সফার নামে পরিচিত, নেটওয়ার্কিং গিয়ার এবং অন্যান্য অ্যাপ্লিকেশনে বিশালভাবে ব্যবহৃত হয়। এটা ওয়েব সার্ভিস ব্যবহার করে বিভিন্ন ধরনের সার্ভিস প্রোভিশন, অ্যাকশন এবং স্ট্যাটাস জানতে ব্যবহার করা হয়। REST API অসাধারণভাবে দ্রুত, সহজ এবং নিরাপদ। এর পাশাপাশি, NETCONF নামে আরেকটা নেটওয়ার্ক কনফিগারেশন প্রোটোকল আছে যা আপনি একসাথে অনেক নেটওয়ার্ক ডিভাইস কনফিগার করতে ব্যবহার করতে পারেন। NETCONF অপারেশনগুলো একটা রিমোট প্রোসিজার কল (RPC) লেয়ারকে নিচে রেখে তার উপরে XML এনকোডিং ব্যবহার করে। আমরা যারা উইন্ডোজ ব্যবহার করি তারা এ ধরনের আরপিসি কলের সাথে পরিচিত। আমরা এটা নিয়ে কাজ করার সময় আরো ভেতরে ঢুকবো।

REST কনফিগারেশন প্রোটোকল (RESTCONF) হল NETCONF প্রোটোকলের একটা সাবসেট, তবে এটি REST এপিআই স্ট্রাকচার ব্যবহার করে কাজগুলোকে সহজ করে নিয়ে এসেছে। REST API এর সিম্পিসিটি এবং গতি এবং নেটওয়ার্ক ডিভাইসগুলোর উপর এক জায়গায় আনা API এর সুবিধা পেতে আপনি RESTCONF ব্যবহার করতে পারেন।

### NETCONF প্রোটোকল

যদিও এটা সম্প্রতি জনপ্রিয়তা পাচ্ছে (আমার পছন্দ RESTCONF), NETCONF আসলে অনেক আগে থেকেই ব্যবহৃত হচ্ছে। এর RFC প্রথম প্রকাশিত হয়ে আসে ২০০৬ সালে। NETCONF হলো একটা XML-ভিত্তিক ইন্টারফেস যা নেটওয়ার্ক ডিভাইসগুলোর কনফিগার এবং মনিটর করতে ব্যবহৃত হয়। এর প্রধান উদ্দেশ্যগুলোর মধ্যে একটা হলো SNMP (Simple Network Management Protocol)কে সাপ্লিমেন্ট করা। 

প্রাথমিকভাবে SNMP-কে 'রিড' এবং 'রাইট' উভয় কাজের জন্য তৈরি করা হয়েছিল, কিন্তু "রাইট" ব্যাপারটা কখনোই খুব বেশি জনপ্রিয়তা পায়নি। এর কারণ হল MIBs (Management Information Bases) নেভিগেট করা এবং সঠিক রেজাল্ট পাওয়ার জন্য প্রয়োজনীয় কমান্ডগুলো বোঝা খুবই জটিল ছিল। 

NETCONF সাধারণত TCP পোর্ট ৮৩০-এ একটা SSH (Secure Shell) সেশন এর মাধ্যমে কাজ করে, যা এটিকে আরও নিরাপদ এবং কার্যকরী করে তোলে। এটি একটা আধুনিক এবং উন্নত পদ্ধতি যা SNMP-র সীমাবদ্ধতাগুলো দূর করতে সাহায্য করে এবং নেটওয়ার্ক ডিভাইসগুলোর আরও কার্যকরী এবং সুনির্দিষ্ট ব্যবস্থাপনা প্রদান করে। তবে, REST কনফিগারেশন প্রোটোকল (RESTCONF) এখন অন্য লেভেলে গিয়েছে।

NETCONFকে অনেক সময় অনানুষ্ঠানিকভাবে SNMPv4 হিসেবে ভাবা হয় কারণ এটি মূলত SNMP-র চতুর্থ সংস্করণের মত কাজ করে। এটা নেটওয়ার্ক ম্যানেজমেন্টর ক্ষেত্রে আরও ভাল, দ্রুত এবং নির্ভরযোগ্য পদ্ধতি দিয়েছে, যা নেটওয়ার্ক ইঞ্জিনিয়ারদের কাজকে আরও সহজ এবং কার্যকরী করে তুলেছে। আমার কথা হচ্ছে, ব্যাপারটা জেনে রাখেন, তবে REST কনফিগারেশন প্রোটোকল (RESTCONF) হচ্ছে আমাদের খেলার জায়গা।

তাহলে, রেস্টকন্ফ কেন ব্যবহার করবো?

রেস্টকন্ফ হলো একটা REST-ভিত্তিক API যা নেটকন্ফ-এর ব্যবহৃত SSH সেশনকে প্রতিস্থাপন করে। নেটকন্ফ এবং রেস্টকন্ফ উভয়ের জন্য ব্যবহৃত YANG মডেলগুলো অভিন্ন। সহজভাবে বললে, রেস্টকন্ফ হলো একটা ওয়েব API যা দীর্ঘদিনের নেটকন্ফ ফ্রেমওয়ার্কের উপর ভিত্তি করে তৈরি। রেস্টকন্ফ, নেটকন্ফ-এর XML ইন্টারফেসের উপরে JSON ডেটা ফরম্যাটের বিকল্পও প্রদান করে (যদিও XML ব্যবহার করা যেতে পারে)। আমি ব্যক্তিগতভাবে রেস্টকন্ফ ব্যবহার করতে পছন্দ করি কারণ আমি REST API-এর সাথে পরিচিত এবং এই ইন্টারফেসটি আমার কাছে খুবই পরিচিত।

নেটকন্ফ এবং রেস্টকন্ফ-এর মধ্যে পার্থক্য কী?

নেটকন্ফ প্রযুক্তিগতভাবে রেস্টকন্ফ-এর চেয়ে কিছু বেশি কার্যকরী সুবিধা প্রদান করে। সবচেয়ে স্পষ্ট পার্থক্য হল যে স্ট্রিমিং টেলিমেট্রি (উদাহরণস্বরূপ: প্রতিটি X সেকেন্ডে CPU ব্যবহার পরিমাপ করা) একটা সেশন খোলা রাখার প্রয়োজন। এটি একটা SSH সেশনের মাধ্যমে সম্ভব, কিন্তু REST-এর ক্ষেত্রে, প্রতিটি কমান্ড লেনদেনভিত্তিক এবং সেখানে কোনো সেশন নেই যা এই ধরনের ডেটা প্রবাহিত রাখতে পারে। এছাড়াও কয়েকটি সুবিধা রয়েছে যা এই নথির পরিধির বাইরে।

### আমি কেন এই দুটির একটা ব্যবহার করতে চাইব?

মূল ব্যবহার ক্ষেত্রটি বেশ স্পষ্ট। আপনি যদি হাজারো ডিভাইস ম্যানেজ করেন, তবে প্রতিটি ডিভাইসে ম্যানুয়ালি SSH করে, কী পরিবর্তন প্রয়োজন তা নির্ধারণ করা এবং তারপর সেই পরিবর্তন করা খুবই সময়সাপেক্ষ। একটা ভাল লেখা স্ক্রিপ্ট এবং একটা API কয়েক মিনিটে করতে পারে যা একজন মানুষের কয়েক ঘণ্টা লাগবে এবং তাও মানুষ-ঘণ্টার খরচের হিসেব ছাড়াই।

আরেকটি উন্নত ব্যবহার ক্ষেত্র হলো ইনফ্রাস্ট্রাকচার-এজ-কোড। এর ধারণা হলো যে উদ্দেশ্যটি নেটওয়ার্ক কনফিগারেশনকে সংজ্ঞায়িত করবে, যা পরে সফ্টওয়্যারের মাধ্যমে প্রয়োগ করা হবে। উদাহরণস্বরূপ, যদি আপনার নেটওয়ার্কে একটা নির্দিষ্ট পরিবর্তন প্রয়োজন হয়, আপনি সেটি ম্যানুয়ালি না করে একটা স্ক্রিপ্টের মাধ্যমে দ্রুত করতে পারেন। এটি শুধুমাত্র সময় সাশ্রয় করে না, বরং ভুলের সম্ভাবনাও কমিয়ে দেয়।

নেটকন্ফ এবং রেস্টকন্ফ উভয়ই আপনাকে নেটওয়ার্ক অটোমেশন করতে সাহায্য করে, যা আপনার নেটওয়ার্ক ম্যানেজমেন্টকে আরও সহজ এবং কার্যকর করে তোলে। তাই, আপনি যদি নেটওয়ার্ক ম্যানেজমেন্টর ক্ষেত্রে সময় এবং পরিশ্রম বাঁচাতে চান, তাহলে এই প্রযুক্তিগুলো ব্যবহার করতে পারেন।