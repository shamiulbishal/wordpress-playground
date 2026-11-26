---
title: ব্রাউজারের জন্য WordPress বান্ডল করা
description: ওয়েব বান্ডলার Dockerfile কীভাবে সাধারণ WordPress-কে SQLite-সহ ব্রাউজার-অপ্টিমাইজড বান্ডলে রূপান্তর করে তা জানুন।
---

<!--
# Bundling WordPress for the browser
-->

# ব্রাউজারের জন্য WordPress বান্ডল করা {#bundling-wordpress-for-the-browser}

<!--
The [web bundler Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/src/wordpress-playground/wordpress/Dockerfile) turns a vanilla WordPress into a browser-optimized one:

-   Makes WordPress run on SQLite using the [official drop-in plugin](https://github.com/WordPress/sqlite-database-integration) as MySQL is unsupported in the browser.
-   Reduces the WordPress website size from about 70MB to about 10MB, or 5MB compressed.
-   Runs the WordPress installation wizard.
-   Bundles WordPress as a minified zip file.
-->

[ওয়েব বান্ডলার Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/src/wordpress-playground/wordpress/Dockerfile) একটি সাধারণ WordPress-কে ব্রাউজার-অপ্টিমাইজড সংস্করণে রূপান্তর করে:

- ব্রাউজারে MySQL সমর্থিত না হওয়ায় [অফিসিয়াল ড্রপ-ইন প্লাগইন](https://github.com/WordPress/sqlite-database-integration) ব্যবহার করে WordPress-কে SQLite-এ চালানোর উপযোগী করে।
- WordPress ওয়েবসাইটের আকার প্রায় 70MB থেকে কমিয়ে প্রায় 10MB-তে (অথবা কমপ্রেসড অবস্থায় 5MB-তে) নিয়ে আসে।
- WordPress ইনস্টলেশন উইজার্ড চালায়।
- WordPress-কে একটি মিনিফায়েড zip ফাইল হিসেবে বান্ডল করে।

<!--
Build a new bundle with `nx bundle-wordpress playground-wordpress-builds --wp-version=<version>`, e.g.:
-->

`nx bundle-wordpress playground-wordpress-builds --wp-version=<version>` কমান্ডের মাধ্যমে একটি নতুন বান্ডল তৈরি করুন, যেমন:

```
nx bundle-wordpress playground-wordpress-builds --wp-version=6.1
```

<!--
The bundler outputs:

-   `packages/playground/wordpress-builds/public/wp-6.1.zip` – zipped WordPress files
-   `packages/playground/wordpress-builds/public/wp-6.1/` – a directory with static assets for the specified WordPress versions
-->

বান্ডলারটি নিচের আউটপুটগুলো তৈরি করে:

- `packages/playground/wordpress-builds/public/wp-6.1.zip` – জিপ করা WordPress ফাইলগুলো
- `packages/playground/wordpress-builds/public/wp-6.1/` – নির্দিষ্ট WordPress সংস্করণগুলোর জন্য স্ট্যাটিক অ্যাসেটসমৃদ্ধ একটি ডিরেক্টরি

<!--
Consult [the web bundler Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/src/wordpress-playground/wordpress/Dockerfile) for more details (like the list of supported WordPress versions) and modify it to customize the default WordPress installation.
-->

আরও বিস্তারিত তথ্যের জন্য (যেমন সমর্থিত WordPress সংস্করণের তালিকা) [ওয়েব বান্ডলার Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/src/wordpress-playground/wordpress/Dockerfile) দেখুন এবং ডিফল্ট WordPress ইনস্টলেশন কাস্টমাইজ করতে এটি পরিবর্তন করুন।
