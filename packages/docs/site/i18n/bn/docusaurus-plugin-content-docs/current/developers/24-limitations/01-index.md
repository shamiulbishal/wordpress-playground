---
title: সীমাবদ্ধতা (Limitations)
slug: /developers/limitations
description: Learn about the current limitations of WordPress Playground, including browser-specific behaviors, browser persistence and recovery constraints, iframe quirks, and WP-CLI support.
---

<!--
# Limitations
-->

# সীমাবদ্ধতা (Limitations)

<!--
WordPress Playground is under active development and has some limitations you should keep in mind when running it and developing with it.
-->

ওয়ার্ডপ্রেস প্লেগ্রাউন্ড সক্রিয়ভাবে উন্নয়নাধীন রয়েছে এবং এটি চালানো ও এর মাধ্যমে ডেভেলপমেন্ট করার সময় কিছু সীমাবদ্ধতা আপনার মনে রাখা উচিত।

<!--
You can track the status of these issues on the [Playground Project board](https://github.com/orgs/WordPress/projects/180).
-->

আপনি [Playground Project board](https://github.com/orgs/WordPress/projects/180)-এ এই সমস্যাগুলোর অগ্রগতি ও স্ট্যাটাস ট্র্যাক করতে পারেন।

<!--
## In the browser
-->

## ব্রাউজারে

<!--
### Browser storage and recovery
-->

### ব্রাউজার স্টোরেজ এবং পুনরুদ্ধার (Browser storage and recovery)

<!--
Playground runs WordPress in the browser. New Playgrounds are autosaved when
browser storage and saving are available, and they appear in **Your
Playgrounds**. Playground keeps up to five recent autosaves. After five exist,
creating another deletes the oldest one. Autosaves are recovery points, not
long-term backups. Store an autosave permanently or export a ZIP when you want
to keep it.
-->

প্লেগ্রাউন্ড ব্রাউজারের ভেতরে ওয়ার্ডপ্রেস চালায়। ব্রাউজার স্টোরেজ এবং সংরক্ষণ সুবিধা উপলব্ধ থাকলে নতুন প্লেগ্রাউন্ডগুলো স্বয়ংক্রিয়ভাবে সংরক্ষিত (autosaved) হয় এবং সেগুলো **Your Playgrounds**-এ প্রদর্শিত হয়। প্লেগ্রাউন্ড সাম্প্রতিক পাঁচটি স্বয়ংক্রিয় সংরক্ষণ বজায় রাখে। পাঁচটি পূর্ণ হয়ে গেলে নতুন একটি তৈরির সাথে সাথে সবচেয়ে পুরোনোটি মুছে যায়। স্বয়ংক্রিয় সংরক্ষণগুলো হলো পুনরুদ্ধারের পয়েন্ট (recovery points), দীর্ঘমেয়াদী ব্যাকআপ নয়। আপনি যদি কোনো ইনস্ট্যান্স স্থায়ীভাবে সংরক্ষণ করতে চান, তবে সেটি পার্মানেন্টলি স্টোর করুন অথবা একটি জিপ (ZIP) ফাইল হিসেবে এক্সপোর্ট করে রাখুন।

<!--
Use these storage modes deliberately:

- **Autosaved**: stored in browser storage and retained only while it is one of up to five recent autosaves.
- **Saved**: stored permanently in browser storage or saved to a local directory.
- **Temporary**: created with `?storage=temp` or when saving is unavailable. It is discarded when the tab closes or the browser page refreshes.
-->

এই স্টোরেজ মোডগুলো সচেতনভাবে ব্যবহার করুন:

- **স্বয়ংক্রিয়ভাবে সংরক্ষিত (Autosaved)**: ব্রাউজার স্টোরেজে সংরক্ষিত থাকে এবং সাম্প্রতিক পাঁচটি স্বয়ংক্রিয় সংরক্ষণের একটি থাকা পর্যন্ত অক্ষুণ্ণ থাকে।
- **সংরক্ষিত (Saved)**: ব্রাউজার স্টোরেজে স্থায়ীভাবে জমা থাকে অথবা একটি স্থানীয় ডিরেক্টরিতে সংরক্ষিত থাকে।
- **অস্থায়ী (Temporary)**: `?storage=temp` দিয়ে তৈরি করা হয় অথবা সংরক্ষণ সুবিধা অনুপলব্ধ থাকলে এটি ব্যবহৃত হয়। ট্যাব বন্ধ করলে কিংবা ব্রাউজার পেজ রিফ্রেশ করলেই এটি বাতিল (discard) হয়ে যায়।

<!--
The Playground **Refresh page** button reloads the WordPress page inside the current Playground. Browser refresh (Cmd+R or F5) reloads the whole Playground app. A stored or autosaved Playground can recover after that reload, but a temporary Playground cannot.
-->

প্লেগ্রাউন্ডের **Refresh page** বাটনটি বর্তমান প্লেগ্রাউন্ডের ভেতরের ওয়ার্ডপ্রেস পেজটিকে পুনরায় লোড করে। ব্রাউজার রিফ্রেশ (Cmd+R বা F5) পুরো প্লেগ্রাউন্ড অ্যাপটিকে পুনরায় লোড করে। সংরক্ষিত (stored) বা স্বয়ংক্রিয়ভাবে সংরক্ষিত (autosaved) প্লেগ্রাউন্ড সেই রিলোডের পরেও রিকভার হতে পারে, কিন্তু অস্থায়ী (temporary) প্লেগ্রাউন্ড তা পারে না।

<!--
![The Dock controls for refreshing WordPress, opening storage choices, and exporting the Playground](https://raw.githubusercontent.com/WordPress/wordpress-playground/refs/heads/trunk/packages/docs/site/static/img/dock/persistence-controls.webp)
-->

![ওয়ার্ডপ্রেস রিফ্রেশ, স্টোরেজ পছন্দ খোলা এবং প্লেগ্রাউন্ড এক্সপোর্ট করার ডক নিয়ন্ত্রণ](https://raw.githubusercontent.com/WordPress/wordpress-playground/refs/heads/trunk/packages/docs/site/static/img/dock/persistence-controls.webp)

<!--
Browser storage still belongs to the browser. Storage pressure, private browsing, profile changes, or clearing site data can remove it. Export a ZIP when you need a portable backup.
-->

ব্রাউজার স্টোরেজ মূলত ব্রাউজারের নিয়ন্ত্রণাধীন। স্টোরেজের অতিরিক্ত চাপ, প্রাইভেট ব্রাউজিং, প্রোফাইল পরিবর্তন কিংবা সাইটের ডেটা মুছে ফেলার ফলে এটি নষ্ট বা মুছে যেতে পারে। তাই বহনযোগ্য ব্যাকআপের প্রয়োজন হলে একটি জিপ (ZIP) ফাইল এক্সপোর্ট করুন।

<!--
![The Your Playgrounds pane with the current Playground](https://raw.githubusercontent.com/WordPress/wordpress-playground/refs/heads/trunk/packages/docs/site/static/img/dock/your-playgrounds.webp)
-->

![বর্তমান প্লেগ্রাউন্ড সহ Your Playgrounds প্যানেল](https://raw.githubusercontent.com/WordPress/wordpress-playground/refs/heads/trunk/packages/docs/site/static/img/dock/your-playgrounds.webp)

<!--
### Browser support
-->

### ব্রাউজার সমর্থন (Browser support)

<!--
WordPress Playground is designed to work across all major desktop and mobile browsers. This includes:

- **Desktop browsers**: Chrome, Firefox, Safari, Edge, and other Chromium-based browsers
- **Mobile browsers**: Safari (iOS), Chrome (Android), and other mobile browser variants
-->

ওয়ার্ডপ্রেস প্লেগ্রাউন্ড সকল প্রধান ডেস্কটপ ও মোবাইল ব্রাউজারে কাজ করার জন্য তৈরি করা হয়েছে। এর মধ্যে রয়েছে:

- **ডেস্কটপ ব্রাউজার**: Chrome, Firefox, Safari, Edge এবং ক্রোমিয়াম-ভিত্তিক অন্যান্য ব্রাউজার
- **মোবাইল ব্রাউজার**: Safari (iOS), Chrome (Android) এবং অন্যান্য মোবাইল ব্রাউজার ভ্যারিয়েন্ট

<!--
Playground leverages modern web technologies and should function consistently across these browser environments. However, some advanced features may have varying levels of support depending on the specific browser and its version.
-->

প্লেগ্রাউন্ড আধুনিক ওয়েব প্রযুক্তি ব্যবহার করে এবং এই সমস্ত ব্রাউজার পরিবেশে স্বাভাবিকভাবে কাজ করা উচিত। তবে নির্দিষ্ট ব্রাউজার এবং এর সংস্করণের ওপর ভিত্তি করে কিছু উন্নত ফিচারের সমর্থনের মাত্রায় ভিন্নতা থাকতে পারে।

<!--
### Performance expectations
-->

### পারফরম্যান্স প্রত্যাশা (Performance expectations)

<!--
Loading times vary based on what Playground needs to set up:

| Scenario                               | Typical Load Time          |
| -------------------------------------- | -------------------------- |
| Fresh WordPress (no plugins)           | 5-10 seconds               |
| With small plugins                     | 10-20 seconds              |
| With large plugins (e.g., WooCommerce) | 30-60 seconds              |
| On mobile devices                      | 1.5-2x slower than desktop |
-->

প্লেগ্রাউন্ডের কী সেটআপ করা প্রয়োজন তার ওপর ভিত্তি করে লোডিংয়ের সময় পরিবর্তিত হয়:

| পরিস্থিতি (Scenario)                   | স্বাভাবিক লোডের সময় (Typical Load Time) |
| -------------------------------------- | ---------------------------------------- |
| একদম নতুন ওয়ার্ডপ্রেস (প্লাগইন ছাড়া) | ৫-১০ সেকেন্ড                             |
| ছোট প্লাগইন সহ                         | ১০-২০ সেকেন্ড                            |
| বড় প্লাগইন সহ (যেমন, WooCommerce)     | ৩০-৬০ সেকেন্ড                            |
| মোবাইল ডিভাইসে                         | ডেস্কটপের তুলনায় ১.৫-২ গুণ ধীরগতির      |

<!--
![Playground performance graph](https://raw.githubusercontent.com/WordPress/wordpress-playground/refs/heads/trunk/packages/docs/site/static/img/playground-performance-graph.webp)
-->

![প্লেগ্রাউন্ড পারফরম্যান্স গ্রাফ](https://raw.githubusercontent.com/WordPress/wordpress-playground/refs/heads/trunk/packages/docs/site/static/img/playground-performance-graph.webp)

<!--
**Factors that affect performance:**

- **Plugin size**: Large plugins take longer to install at runtime
- **Network speed**: WASM files are 15-30MB
- **Device memory**: Low-memory devices may experience slowdowns
- **Browser**: Chrome/Edge perform best; Safari slightly slower
-->

**যেসব বিষয় পারফরম্যান্সে প্রভাব ফেলে:**

- **প্লাগইনের আকার (Plugin size)**: বড় আকারের প্লাগইন রানটাইমে ইনস্টল হতে বেশি সময় নেয়
- **নেটওয়ার্কের গতি (Network speed)**: WASM ফাইলগুলোর আকার সাধারণত ১৫-৩০ মেগাবাইট (MB)
- **ডিভাইসের মেমরি (Device memory)**: কম মেমরির ডিভাইসে কাজের গতি কমে যেতে পারে
- **ব্রাউজার (Browser)**: Chrome/Edge সবচেয়ে ভালো কাজ করে; Safari কিছুটা ধীরগতির হতে পারে

<!--
<blockquote>
<strong>Note:</strong> Opera Mini support is not currently confirmed.
</blockquote>
-->

<blockquote>
<strong>নোট:</strong> অপেরা মিনি (Opera Mini) সমর্থন বর্তমানে নিশ্চিত নয়।
</blockquote>

<!--
## When developing with Playground
-->

## প্লেগ্রাউন্ডের সাথে ডেভেলপমেন্ট করার সময়

<!--
### Iframe quirks
-->

### আইফ্রেমের বিশেষত্ব (Iframe quirks)

<!--
Playground renders WordPress in an [`iframe`](/developers/architecture/browser-iframe-rendering) so clicking links with `target="_top"` will reload the page you’re working on.
Also, JavaScript popups originating in the `iframe` may not always display.
-->

প্লেগ্রাউন্ড ওয়ার্ডপ্রেসকে একটি [`iframe`](/developers/architecture/browser-iframe-rendering)-এর ভেতরে রেন্ডার করে, তাই `target="_top"` যুক্ত লিঙ্কে ক্লিক করলে আপনি যে পেজে কাজ করছেন সেটি পুনরায় লোড হয়ে যাবে।
এছাড়াও `iframe` থেকে উৎপন্ন জাভাস্ক্রিপ্ট পপআপ সবসময় দৃশ্যমান নাও হতে পারে।

<!--
### Run WordPress PHP functions
-->

### ওয়ার্ডপ্রেস পিএইচপি ফাংশন চালানো (Run WordPress PHP functions)

<!--
Playground supports running PHP code in Blueprints using the [`runPHP` step](/blueprints/steps#RunPHPStep). To run WordPress-specific PHP functions, you’d need to first require [wp-load.php](https://github.com/WordPress/WordPress/blob/master/wp-load.php):

```json
{
	"step": "runPHP",
	"code": "<?php require_once('wordpress/wp-load.php'); OTHER_CODE ?>"
}
```
-->

ব্লুপ্রিন্টে [`runPHP` ধাপটি](/blueprints/steps#RunPHPStep) ব্যবহার করে পিএইচপি কোড চালানোর সুবিধা সমর্থন করে প্লেগ্রাউন্ড। ওয়ার্ডপ্রেস-নির্দিষ্ট পিএইচপি ফাংশনগুলো চালাতে প্রথমে [wp-load.php](https://github.com/WordPress/WordPress/blob/master/wp-load.php) ফাইলটি রিকয়ার (require) করতে হবে:

```json
{
	"step": "runPHP",
	"code": "<?php require_once('wordpress/wp-load.php'); OTHER_CODE ?>"
}
```

<!--
### Using WP-CLI
-->

### ডব্লিউপি-সিএলআই ব্যবহার করা (Using WP-CLI)

<!--
You can execute `wp-cli` commands via the Blueprints [`wp-cli`](/blueprints/steps#WPCLIStep) step. However, since Playground runs in the browser, it doesn't support the [full array](https://developer.wordpress.org/cli/commands/) of available commands. While there is no definite list of supported commands, experimenting in [the online demo](https://playground.wordpress.net/demos/wp-cli.html) will help you assess what's possible.
-->

ব্লুপ্রিন্টের [`wp-cli`](/blueprints/steps#WPCLIStep) ধাপের মাধ্যমে আপনি `wp-cli` কমান্ডগুলো চালাতে পারেন। তবে প্লেগ্রাউন্ড ব্রাউজারে চালিত হওয়ায় এটি উপলব্ধ কমান্ডের [সম্পূর্ণ পরিসর](https://developer.wordpress.org/cli/commands/) সমর্থন করে না। যদিও সমর্থিত কমান্ডগুলোর কোনো চূড়ান্ত তালিকা নেই, তবে [অনলাইন ডেমোতে](https://playground.wordpress.net/demos/wp-cli.html) পরীক্ষা-নিরীক্ষা করে আপনি সম্ভাব্য কার্যকারিতা যাচাই করে নিতে পারেন।
