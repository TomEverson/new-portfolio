---
layout: ../../layout/BlogLayout.astro
title: SQLite Deep Dive (Part-0) -- Introduction
date: 2024-03-15
---

ဒါကို hobby project လေးတွေအတွက် သုံးတဲ့ database လေးတစ်ခုလို့ မင်းထင်နေမှာပါ။ တကယ်တမ်းကတော့ — မင်းထင်တာထက် အများကြီး ပိုစိတ်ဝင်စားစရာကောင်းတယ်။

![SQLite Deep Dive.png](/public/sqlite-deep-dive/p-0.png)


မကြာသေးမီက ကျွန်တော် **Data Storage Class** ကိုတက်ဖြစ်ခဲ့တယ်။ အဲဒီ class မှာ topic တော်တော်များများ ရှိပေမဲ့ ကျွန်တော့်အာရုံကို **စိတ်အဝင်စားဆုံး ဖြစ်ခဲ့တာကတော့ SQLite** ပဲ။

ဘာကြောင့်လဲဆိုတော့ လူတော်တော်များများက SQLite ကို “ဪ... ဒါလေးက side project လေးတွေမှာပဲ သုံးတဲ့ file database လေးပေါ့” လို့ပဲ မြင်ကြလို့ပါ။ ကျွန်တော်လည်း အစပိုင်းမှာ အဲဒီလိုပဲ ထင်ခဲ့တာ **အမှန်အတိုင်း ဝန်ခံရရင်** ပေါ့။

ဒါပေမဲ့ ပိုပြီး နက်နက်နဲနဲ လေ့လာကြည့်လေ — ဒီ database ရဲ့ အတွင်းပိုင်းမှာ တကယ်ဘာတွေ ရှိနေတယ်ဆိုတာ သိလာရတဲ့အခါမှာတော့ ကျွန်တော် တကယ်ပဲ အံ့ဩသွားမိတယ်။

---

# **SQLite ဆိုတာ စစ်တပ်အဆင့် software တစ်ခုပါ။**

ဒါကြားတာနဲ့ “ဟာ overstatement ပါပဲ” လို့ တွေးမိနိုင်တယ်။ ကျွန်တော်လည်း နားမလည်သေးတဲ့အချိန်မှာ အဲဒီလိုပဲ တွေးမိတယ်။ ဒါပေမဲ့ တကယ်ပါပဲ။

၂၀၀၀ ပြည့်နှစ်မှာ D. Richard Hipp ဆိုတဲ့ developer တစ်ယောက်က ဒီ software ကို တည်ဆောက်ခဲ့တယ် — startup တစ်ခုအတွက်မဟုတ်ဘူး၊ funding ရဖို့မဟုတ်ဘူး — တိုက်ရိုက်ပြောရရင် **အမေရိကန်ရေတပ်** အတွက်ပဲ။ ဒုံးကျည်တင်ဆောင် တိုက်စစ်သင်္ဘောတွေပေါ်မှာ **data** တွေကို စီမံဖို့ ဖန်တီးခဲ့တာ။

ခဏစဉ်းစားကြည့် — ဒီ environment မှာ internet ဆိုတာ မရှိဘူး။ System crash ဖြစ်ရင် ပြင်ပေးနိုင်မယ့် engineer ရှိမှာမဟုတ်ဘူး။ Data ပျောက်သွားရင် — အဆင်ပြေနိုင်မယ့် အနေအထားမဟုတ်ဘူး။ ဒါကြောင့် ဒီ software ဟာ မမှားရဘူး၊ မပျက်ရဘူး — ဒါဟာ **option** တစ်ခုမဟုတ်ဘဲ requirement ပဲ။

မင်းဖုန်းထဲမှာ တိတ်တဆိတ် run နေတဲ့ database engine လေးဟာ တစ်ချိန်က စစ်သင်္ဘောပေါ်မှာ survive ဖို့ ဖန်တီးခဲ့တာ — ဒါ cinematic မဟုတ်ဘဲ တကယ်ဖြစ်ပျက်ခဲ့တဲ့ အကြောင်းအရာပါ။

---

# **ဘာကြောင့် ထူးခြားသလဲ?**

SQLite ကို အခြား database တွေနဲ့ ကွာခြားစေတာကတော့ သူ့ရဲ့ အခြေခံ **philosophy** ပဲ — **serverless ၊ self-contained နဲ့ zero-configuration။**

PostgreSQL၊ MySQL လိုမျိုး database တော်တော်များများဟာ **client-server model** ကို သုံးကြတယ်။ Server ကို run ရတယ်၊ port configure လုပ်ရတယ်၊ connection တွေ စီမံရတယ်၊ authentication ပြင်ဆင်ရတယ်။ ကြမ်းတမ်းသော workload တွေအတွက်တော့ အသုံးဝင်ပေမဲ့ — complexity ပါလာတယ်။ SQLite ကတော့ ဒါတွေအကုန် ချပစ်လိုက်တယ်။ Database တစ်ခုလုံး — disk ပေါ်က file တစ်ခုတည်းထဲမှာ နေတယ်။ Server မလိုဘူး။ Setup မလိုဘူး။ Background မှာ daemon မပြေးဘူး။ File ဖွင့်လိုက်ရုံနဲ့ — အဆင်သင့်ဖြစ်နေပြီ။

ဒါပေမဲ့ ရိုးရှင်းတယ်ဆိုပြီး လွဲမှားမနားနဲ့။ အဲဒီ **file** တစ်ခုတည်းဟာ အလွန်တိကျသော engineering နဲ့ ဖန်တီးထားတာ။

---

# **Core: SQLite တကယ်ဆိုတာ ဘာလဲ?**

ဒီနေရာမှာ လူတော်တော်များများ ဖတ်တာရပ်ကြပြီး — အပြင်မှာမှ အကောင်းဆုံးအရာတွေ ရှိနေတာ။

SQLite ဆိုတာ “အစစ်အမှန်” database တစ်ခုရဲ့ ချုံ့ထားတဲ့ version မဟုတ်ဘူး။ Feature တွေဖြုတ်ထားတဲ့ PostgreSQL မဟုတ်ဘူး။ ဒါဟာ အစကတည်းက လုံးဝကွဲပြားတဲ့ priority တွေနဲ့ ဒီဇိုင်းဆွဲထားတဲ့ software အမျိုးအစားတစ်ခုပဲ။

SQLite ကို နည်းပညာအရ ဖော်ပြရရင် — **embedded, self-contained, serverless, transactional SQL database engine** တစ်ခုပါ။ စကားတစ်လုံးချင်းစီမှာ အဓိပ္ပါယ်ရှိတယ်၊ ဒါကြောင့် တစ်ခုချင်းစီ ဆင်ခြင်ကြည့်ရအောင်။

**Embedded** ဆိုတာ SQLite ဟာ သီးခြား process တစ်ခုအနေနဲ့ မပြေးဘူး။ မင်းရဲ့ application *ထဲမှာပဲ* ပြေးတယ်။ မင်း app စ run တာနဲ့ SQLite ပါ စတယ်။ မင်း app ပိတ်တာနဲ့ SQLite ပါ ပိတ်တယ်။ Connection တွေကို စောင့်နေတဲ့ background service မရှိဘူး — database engine ဟာ မင်းရဲ့ code နဲ့ memory space တစ်ခုတည်းထဲမှာ နေတယ်။ ဒါကြောင့်မို့ local read/write တွေ ဆိုရင်အလွန်မြန်တာ။

**Self-contained** ဆိုတာ SQL parse လုပ်တာ၊ storage စီမံတာ၊ transaction ကိုင်တွယ်တာ၊ constraint enforce လုပ်တာ — engine တစ်ခုလုံးဟာ C library တစ်ခုထဲမှာ ထည့်သွင်းထားတယ်။ External dependency မရှိဘူး။ Runtime requirement မရှိဘူး။ SQLite library ဟာ system အများစုမှာ 600 KB နဲ့ 1 MB ကြားလောက်ပဲ ရှိတယ်။ မင်းရဲ့ application ထဲ compile ထည့်ပြီး ဘယ်နေရာမဆို — smartphone တစ်လုံး၊ လေယာဉ်တစ်စီး၊ ဆက်တလိုက်တစ်ခု — ship လုပ်လို့ရတယ်။

**Serverless** ဆိုတာ ဆိုလိုချင်တဲ့အတိုင်းပဲ။ Access ကို စီမံတဲ့၊ query တွေကို route လုပ်တဲ့၊ connection တွေကို ကိုင်တွယ်တဲ့ server process မရှိဘူး။ Application ကိုယ်တိုင်က library call တွေမှတဆင့် database file နဲ့ တိုက်ရိုက်ပြောတယ်။ ဒါဟာ infrastructure complexity တစ်ရပ်လုံးကို ဖြေဖျောက်ပေးတယ် — ဒါပေမဲ့ SQLite ဟာ network ကတဆင့် တပြိုင်နက် connect နေတဲ့ client ရာနဲ့ချီတဲ့ workload အတွက်မဟုတ်ဘဲ၊ application တစ်ခုတည်းက local data ကို access လုပ်ဖို့ ဒီဇိုင်းဆွဲထားတာပါ။

**Transactional** ဆိုတဲ့နေရာမှာ လူတော်တော်များများ SQLite ကို အနိမ့်ဆုံး ခန့်မှန်းကြတယ်။ SQLite ဟာ ACID compliance အပြည့်အဝ ပေးနိုင်တယ် — Atomicity, Consistency, Isolation, နဲ့ Durability။ ဒါတွေဟာ buzzword တွေမဟုတ်ဘူး။ ဆိုလိုတာကတော့ — data write နေစဉ် program crash ရင်တောင် database ပျက်စီးမှာမဟုတ်ဘူး၊ operation နှစ်ခု conflict ဖြစ်ရင် တစ်ခုကို cleanly rollback လုပ်မယ်၊ transaction commit ဖြစ်ပြီဆိုရင် ဓာတ်အားပြတ်တောက်မှုပင် ဖြစ်ပေါ်ရင်တောင် ဒေတာ ရှင်သန်နေမယ်။ စစ်သင်္ဘောပေါ်မှာ အရေးကြီးတဲ့ guarantee တစ်ခုဟာ မင်းရဲ့ mobile app ညအချိန် မမျှော်လင့်ဘဲ crash သွားတဲ့အချိန်မှာလည်း အတူတူပဲ အရေးကြီးနေတယ်။

---

# **အံ့သြဖို့ကောင်းတဲ့ ကိန်းဂဏာန်းများ**

SQLite ဟာ ရှိဖူးတဲ့ software တွေထဲမှာ အများဆုံး test လုပ်ထားတဲ့ software တစ်ခုပါ။ Source code ကတော့ လိုင်း ၁၅၀,၀၀၀ ရှိတယ် — ဒါပေမဲ့ test suite ကတော့? Test code လိုင်း ၉၂ သန်းကျော်ရှိတယ်။ ဒါဟာ enterprise software developer တော်တော်များများ အိမ်မက်မက်မိနိုင်မယ့် 600-to-1 testing ratio ပါ။ Edge case တိုင်း၊ failure path တိုင်း၊ ရှားပါးတဲ့ error condition တိုင်း အသေးစိတ် covered လုပ်ထားတယ်။ Developer တွေဟာ mutation testing ပင် သုံးကြတယ် — ဆိုလိုတာ test suite က တကယ်ကောင်းမကောင်း စစ်ဆေးဖို့ source ထဲကို ဆောင်ရွက်ချက် bug တွေ တမင်ထည့်ပြီး စစ်တယ်။

ဒီ rigor ဟာ အကြောင်းရင်းရှိတယ် — SQLite ဟာ bug ခွင့်မပြုနိုင်တဲ့ နေရာတွေမှာ run နေတယ်။ 38,000 ပေအမြင့်မှာ ပြေးနေတဲ့ Airbus ကို patch push လုပ်လို့မရဘူး။ လေကြာင်းယာဉ်ပေါ်က database ကို hotfix လုပ်လို့မရဘူး။ Software ဟာ ပထမဆုံးကတည်းက မှန်ကန်ရမယ်၊ အမြဲမှန်ကန်နေရမယ်။

---

# **တကယ်တမ်း ဘယ်နေရာတွေမှာ ရှိနေလဲ**

SQLite ဟာ ကမ္ဘာပေါ်မှာ အများဆုံး deploy ဖြစ်ထားတဲ့ database engine ပါ။ ယနေ့ use ဖြစ်နေတဲ့ active SQLite database အရေအတွက် ခန့်မှန်းချက်က **trillion တစ်ထရီလီယံ** ကျော်ပါ။ Typo မဟုတ်ဘူး။

Android နဲ့ iOS device တိုင်းထဲမှာ၊ Chrome, Firefox, Safari browser တိုင်းထဲမှာ၊ Airbus A350 လေယာဉ်တွေရဲ့ avionics system ထဲမှာ၊ Adobe Photoshop ထဲမှာ၊ Spotify ရဲ့ offline caching layer ထဲမှာ၊ အချို့သော အာကာသ mission တွေမှာပင် — SQLite run နေတယ်။ Apple ရဲ့ Core Data framework — iOS နဲ့ macOS app သန်းနဲ့ချီတဲ့အတွက် persistence ကို ကိုင်တွယ်တဲ့ framework — ဟာ SQLite ကို default storage backend အဖြစ် သုံးတယ်။ iPhone ကို Mac မှာ backup လုပ်တဲ့အချိန် — ဒီ backup ရဲ့ တစ်ပိုင်းဟာ SQLite file တစ်ခုပဲ။

Engineer တွေဟာ reliable၊ portable၊ operational maintenance မလိုအပ်တဲ့ storage လိုချင်တဲ့အခါ — SQLite ကို ဆက်တိုက် ရွေးချယ်ကြတယ်။ အလွယ်ကူဆုံးဆိုလို့မဟုတ်ဘဲ — ဒီ specific class of problem အတွက် တခြား ဘာမှ မတင်နိုင်ဘူးလို့ ယုံကြည်ကြလို့ပဲ။

---

# **Deep Dive မသွားခင်**

SQLite ဟာ “သေးငယ်တဲ့ database” မဟုတ်ဘူး။ ဒါဟာ သေးငယ်တဲ့ **footprint** ရှိတဲ့ ကြီးမားတဲ့ software တစ်ခုပါ။ ကွာခြားချက်က ကြီးတယ်။

ကျွန်တော် ဒီ series တစ်ခုလုံးမှာ ဖော်ပြသွားမှာကတော့ — SQLite ကို ဘာကြောင့် ဒီလို ဒီဇိုင်းဆွဲခဲ့တယ်ဆိုတာ နားလည်ထားရင်၊ နောက်ပိုင်း technical detail တွေက “ဟာ ဒါကြောင့်ကိုး” ဆိုပြီး သဘာဝကျကျ ပေါ်ထွက်လာမယ်။ မှတ်ဖို့ ကြိုးစားနေစရာ မလိုတော့ဘဲ — နားလည်သွားလိမ့်မယ်။

Part 1 မှာတော့ — .db file တစ်ခုထဲမှာ ဘာတွေ ရှိနေတယ်ဆိုတာ၊ page structure ဆိုတာ ဘာလဲ၊ query တစ်ကြောင်း run လိုက်တဲ့အချိန် SQLite ရဲ့ အတွင်းမှာ တကယ်ဘာဖြစ်သွားတယ်ဆိုတာကြည့်ရအောင်။
