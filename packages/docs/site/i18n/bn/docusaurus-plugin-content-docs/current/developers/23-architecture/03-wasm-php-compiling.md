---
title: পিএইচপি কম্পাইল করা
slug: /developers/architecture/wasm-php-compiling
---

<!--
# Compiling PHP
-->

# পিএইচপি কম্পাইল করা

<!--
The build pipeline lives in a [`Dockerfile`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile). It was originally forked from [seanmorris/php-wasm](https://github.com/seanmorris/php-wasm)
-->

বিল্ড পাইপলাইনটি একটি [`Dockerfile`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile)-এ সংরক্ষিত রয়েছে। এটি মূলত [seanmorris/php-wasm](https://github.com/seanmorris/php-wasm) থেকে ফর্ক করা হয়েছিল।

<!--
In broad strokes, that `Dockerfile`:
-->

স্থূলভাবে বললে, সেই `Dockerfile` নিম্নলিখিত কাজগুলো করে:

<!--
- Installs all the necessary linux packages (like `build-essential`)
- Downloads PHP and the required libraries, e.g. `sqlite3`.
- Applies a few patches.
- Compiles everything using [Emscripten](https://emscripten.org/), a drop-in replacement for the C compiler.
- Compiles `php_wasm.c` – a convenient API for JavaScript.
- Outputs a `php.wasm` file and one or more JavaScript loaders, depending on the configuration.
- Transforms the Emscripten's default `php.js` output into an ESM module with additional features.
-->

- সমস্ত প্রয়োজনীয় লিনাক্স প্যাকেজ ইনস্টল করে (যেমন `build-essential`)
- PHP এবং প্রয়োজনীয় লাইব্রেরিগুলো ডাউনলোড করে, যেমন `sqlite3`।
- কয়েকটি প্যাচ প্রয়োগ করে।
- C কম্পাইলারের ড্রপ-ইন বিকল্প হিসেবে [Emscripten](https://emscripten.org/) ব্যবহার করে সবকিছু কম্পাইল করে।
- `php_wasm.c` কম্পাইল করে – যা JavaScript-এর জন্য একটি সুবিধাজনক API।
- কনফিগারেশনের উপর নির্ভর করে একটি `php.wasm` ফাইল এবং এক বা একাধিক JavaScript লোডার আউটপুট হিসেবে প্রদান করে।
- Emscripten-এর ডিফল্ট `php.js` আউটপুটকে অতিরিক্ত সুবিধাযুক্ত একটি ESM মডিউলে রূপান্তর করে।

<!--
To find out more about each step, refer directly to the [Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile).
-->

প্রতিটি ধাপ সম্পর্কে আরও বিশদ জানতে সরাসরি [Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile)-টি দেখুন।

<!--
## Building
-->

## বিল্ড করা

<!--
With Docker running and the repository dependencies installed, run these commands from the repository root:
-->

Docker চালু থাকা অবস্থায় এবং রিপোজিটরির ডিপেন্ডেন্সি ইনস্টল করার পর, রিপোজিটরির রুট ডিরেক্টরি থেকে এই কমান্ডগুলো চালান:

```sh
# Build all supported PHP versions for the web, in both JSPI and Asyncify modes.
npx nx recompile-php:all php-wasm-web

# Build only PHP 8.4 for the web, in JSPI mode.
npx nx recompile-php:jspi php-wasm-web -- --PHP_VERSION=8.4
```

<!--
Replace `php-wasm-web` with `php-wasm-node` to build for Node.js, or `recompile-php:jspi` with `recompile-php:asyncify` to build the Asyncify variant. The output goes to `packages/php-wasm/web-builds/<major>-<minor>/<mode>/` or `packages/php-wasm/node-builds/<major>-<minor>/<mode>/`.
-->

Node.js-এর জন্য বিল্ড করতে `php-wasm-web`-এর বদলে `php-wasm-node`, অথবা Asyncify ভেরিয়েন্ট বিল্ড করতে `recompile-php:jspi`-এর বদলে `recompile-php:asyncify` ব্যবহার করুন। আউটপুট ফাইলগুলো `packages/php-wasm/web-builds/<major>-<minor>/<mode>/` অথবা `packages/php-wasm/node-builds/<major>-<minor>/<mode>/` ডিরেক্টরিতে জমা হয়।

<!--
## Debug builds
-->

## ডিবাগ বিল্ড (Debug builds)

<!--
Use `--WITH_DEBUG=yes` to build PHP.wasm with readable JavaScript output and DWARF debug information for stepping through C code in a WebAssembly debugger:
-->

WebAssembly ডিবাগারে C কোডের মধ্য দিয়ে স্টেপ-বাই-স্টেপ যাওয়ার জন্য পাঠযোগ্য JavaScript আউটপুট এবং DWARF ডিবাগ তথ্যসহ PHP.wasm বিল্ড করতে `--WITH_DEBUG=yes` ব্যবহার করুন:

```sh
# Build PHP 8.4 for debugging in the browser.
npx nx recompile-php:jspi php-wasm-web -- --PHP_VERSION=8.4 --WITH_DEBUG=yes

# Build PHP 8.4 for debugging in Node.js.
npx nx recompile-php:jspi php-wasm-node -- --PHP_VERSION=8.4 --WITH_DEBUG=yes
```

<!--
The same option works with `recompile-php:asyncify`. Debug builds produce larger files and run more slowly than optimized builds. They replace the selected version's artifacts in the output directory described above. Rebuild with `--WITH_DEBUG=no --WITH_SOURCEMAPS=no` to restore an optimized build.
-->

একই অপশন `recompile-php:asyncify`-এর সাথেও কাজ করে। অপ্টিমাইজড বিল্ডের তুলনায় ডিবাগ বিল্ড তুলনামূলক বড় ফাইল তৈরি করে এবং ধীরগতিতে চলে। এগুলো উপরে বর্ণিত আউটপুট ডিরেক্টরিতে নির্বাচিত সংস্করণের আর্টিকফ্যাক্টগুলোকে প্রতিস্থাপন করে। পুনরায় একটি অপ্টিমাইজড বিল্ডে ফিরে যেতে `--WITH_DEBUG=no --WITH_SOURCEMAPS=no` দিয়ে পুনরায় বিল্ড করুন।

<!--
For WebAssembly source maps, use `--WITH_SOURCEMAPS=yes`:
-->

WebAssembly সোর্স ম্যাপের জন্য `--WITH_SOURCEMAPS=yes` ব্যবহার করুন:

```sh
npx nx recompile-php:jspi php-wasm-web -- --PHP_VERSION=8.4 --WITH_SOURCEMAPS=yes
```

<!--
This generates a `php.wasm.map` file and copies the source files needed for debugging into the build output. For web builds, the source map URL points to the local development server at `http://127.0.0.1:5400`; run `npm run dev` to serve it.
-->

এটি একটি `php.wasm.map` ফাইল তৈরি করে এবং ডিবাগিংয়ের জন্য প্রয়োজনীয় সোর্স ফাইলগুলো বিল্ড আউটপুটে কপি করে। ওয়েব বিল্ডের ক্ষেত্রে, সোর্স ম্যাপ URL-টি `http://127.0.0.1:5400`-এ অবস্থিত লোকাল ডেভেলপমেন্ট সার্ভারকে নির্দেশ করে; এটি পরিবেশন করতে `npm run dev` চালান।

<!--
### Emscripten options
-->

### Emscripten অপশন

<!--
The build script translates these options into compiler flags in the [PHP Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile):
-->

বিল্ড স্ক্রিপ্টটি এই অপশনগুলোকে [PHP Dockerfile](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile)-এর কম্পাইলার ফ্ল্যাগে রূপান্তর করে:

| Flag           | Purpose                                                                                                | When Playground uses it                                                 |
| -------------- | ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| `-O0`          | Disables optimization of the final WebAssembly and JavaScript output.                                  | `WITH_DEBUG=yes` or `WITH_SOURCEMAPS=yes`, replacing the default `-O3`. |
| `-g2`          | Keeps function names and readable JavaScript, without retaining DWARF information in the final module. | Node.js builds when neither debug option is enabled.                    |
| `-g3`          | Retains DWARF information for source-level debugging.                                                  | `WITH_DEBUG=yes` or `WITH_SOURCEMAPS=yes`.                              |
| `-gsource-map` | Generates a WebAssembly source map from compiler debug information.                                    | `WITH_SOURCEMAPS=yes`.                                                  |

<!--
See the [Emscripten compiler reference](https://emscripten.org/docs/tools_reference/emcc.html) for details on these flags.
-->

এই ফ্ল্যাগগুলোর বিস্তারিত জানার জন্য [Emscripten compiler reference](https://emscripten.org/docs/tools_reference/emcc.html) দেখুন।

<!--
### Runtime assertions
-->

### রানটাইম অ্যাসারশন (Runtime assertions)

<!--
Debug information and runtime assertions are separate settings. Playground explicitly passes `-s ASSERTIONS=0`, including in debug builds, so `--WITH_DEBUG=yes` does not enable extra runtime checks.
-->

ডিবাগ তথ্য এবং রানটাইম অ্যাসারশন সম্পূর্ণ আলাদা সেটিংস। প্লেগ্রাউন্ড স্পষ্টভাবে ডিবাগ বিল্ডসহ সকল ক্ষেত্রে `-s ASSERTIONS=0` পাস করে, তাই `--WITH_DEBUG=yes` কোনো অতিরিক্ত রানটাইম চেক সক্রিয় করে না।

<!--
To investigate a runtime failure with assertions, change that setting in the PHP Dockerfile's final `emcc` command and rebuild. Emscripten documents `-s ASSERTIONS=1` for runtime checks and `-s ASSERTIONS=2` for additional, slower checks. There is no `WITH_ASSERTIONS` build option. See the [Emscripten assertions reference](https://emscripten.org/docs/tools_reference/settings_reference.html#assertions).
-->

অ্যাসারশনসহ রানটাইম ব্যর্থতা তদন্ত করতে PHP Dockerfile-এর চূড়ান্ত `emcc` কমান্ডে সেই সেটিংটি পরিবর্তন করুন এবং পুনরায় বিল্ড করুন। Emscripten সাধারণ রানটাইম চেকের জন্য `-s ASSERTIONS=1` এবং অতিরিক্ত, ধীরগতির চেকের জন্য `-s ASSERTIONS=2` নথিভুক্ত করে। কোনো `WITH_ASSERTIONS` বিল্ড অপশন নেই। বিস্তারিত জানার জন্য [Emscripten assertions reference](https://emscripten.org/docs/tools_reference/settings_reference.html#assertions) দেখুন।

<!--
## PHP next builds
-->

## PHP নেক্সট বিল্ড (PHP next builds)

<!--
Playground can also run the next PHP version from the php-src development branch in the web runtime. These builds are published separately from the main repository because the generated WebAssembly files are large and change often.
-->

প্লেগ্রাউন্ড ওয়েব রানটাইমে php-src ডেভেলপমেন্ট ব্রাঞ্চ থেকে পরবর্তী পিএইচপি সংস্করণও চালাতে পারে। এই বিল্ডগুলো প্রধান রিপোজিটরি থেকে আলাদাভাবে প্রকাশ করা হয় কারণ জেনারেট হওয়া WebAssembly ফাইলগুলো আকারে বেশ বড় এবং ঘন ঘন পরিবর্তিত হয়।

<!--
The nightly refresh workflow builds the php-src development branch, writes the web artifacts to the gitignored `packages/playground/website/public/php-next/` directory, and publishes the result to the `php-next-builds` branch. Website deploys and the local dev server sync that branch before serving `?php=next`.
-->

নাইটলি রিফ্রেশ ওয়ার্কফ্লো php-src ডেভেলপমেন্ট ব্রাঞ্চ বিল্ড করে, ওয়েব আর্টিকফ্যাক্টগুলোকে গিট-উপেক্ষিত `packages/playground/website/public/php-next/` ডিরেক্টরিতে লেখে এবং ফলাফলটি `php-next-builds` ব্রাঞ্চে প্রকাশ করে। ওয়েবসাইট ডিপ্লয়মেন্ট এবং লোকাল ডেভেলপমেন্ট সার্ভার `?php=next` পরিবেশন করার আগে সেই ব্রাঞ্চটি সিঙ্ক করে নেয়।

<!--
To refresh the local copy manually, run:
-->

ম্যানুয়ালি লোকাল কপি রিফ্রেশ করতে চালান:

```sh
npm run sync:php-next
```

<!--
To rebuild the web artifacts locally from the php-src development branch, run:
-->

php-src ডেভেলপমেন্ট ব্রাঞ্চ থেকে স্থানীয়ভাবে ওয়েব আর্টিকফ্যাক্টগুলো পুনরায় বিল্ড করতে চালান:

```sh
npm run recompile:php:web:next
```

<!--
`php=next` currently ships web main modules only. Matching extension side modules and Playground CLI support are separate follow-up work.
-->

`php=next` বর্তমানে শুধুমাত্র ওয়েব মেইন মডিউল সরবরাহ করে। মানানসই এক্সটেনশন সাইড মডিউল এবং প্লেগ্রাউন্ড CLI সমর্থন পরবর্তী পৃথক কাজের অন্তর্ভুক্ত।

<!--
## PHP extensions
-->

## PHP এক্সটেনশন

<!--
PHP is built with several extensions listed in the [`Dockerfile`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile).
-->

[`Dockerfile`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/Dockerfile)-এ তালিকাভুক্ত বেশ কয়েকটি এক্সটেনশনসহ PHP বিল্ড করা হয়।

<!--
Some extensions, like `zip`, can be turned on or off during the build. Others, like `sqlite3`, are hardcoded.
-->

`zip`-এর মতো কিছু এক্সটেনশন বিল্ড করার সময় চালু বা বন্ধ করা যেতে পারে। অন্যগুলো, যেমন `sqlite3`, হার্ডকোড করা থাকে।

<!--
If you need to turn off one of the hardcoded extensions, feel free to open an issue in this repo. Better yet, this project needs contributors. You are more than welcome to open a PR and author the change you need.
-->

আপনি যদি কোনো হার্ডকোড করা এক্সটেনশন নিষ্ক্রিয় করতে চান, তবে নিঃসংকোচে এই রিপোজিটরিতে একটি ইস্যু খুলতে পারেন। আরও ভালো হয়, যদি আপনি নিজেই এগিয়ে আসেন কারণ এই প্রজেক্টে অবদানকারী প্রয়োজন। প্রয়োজনীয় পরিবর্তনের জন্য পিআর (PR) ওপেন করতে আপনাকে আন্তরিকভাবে স্বাগত জানানো হচ্ছে।

<!--
PHP.wasm can also load dynamic `.so` extensions before PHP starts. Built-in
dynamic extensions such as `intl`, `xdebug`, `redis`, and `memcached` are
distributed with the Node package, and external extensions can be supplied with
a manifest that selects the artifact matching the active PHP version and async
mode. See [Loading PHP extensions](/developers/apis/javascript-api/php-extensions)
for the runtime API.
-->

PHP চালু হওয়ার আগেই PHP.wasm ডায়নামিক `.so` এক্সটেনশনও লোড করতে পারে। বিল্ট-ইন ডায়নামিক এক্সটেনশন যেমন `intl`, `xdebug`, `redis` এবং `memcached` নোড প্যাকেজের সাথে বিতরণ করা হয়, এবং সক্রিয় পিএইচপি সংস্করণ ও অ্যাসিনক্রোনাস মোডের সাথে সামঞ্জস্যপূর্ণ আর্টিকফ্যাক্ট নির্বাচনকারী একটি ম্যানিফেস্টের মাধ্যমে বাহ্যিক এক্সটেনশনও সরবরাহ করা যেতে পারে। রানটাইম API-এর জন্য [Loading PHP extensions](/developers/apis/javascript-api/php-extensions) দেখুন।

<!--
## C API exposed to JavaScript
-->

## JavaScript-এর কাছে উন্মুক্ত C API

<!--
The C API exposed to JavaScript lives in the [`php_wasm.c`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/php_wasm.c) file. The most important functions are:
-->

JavaScript-এর কাছে উন্মুক্ত C API-টি [`php_wasm.c`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/php_wasm.c) ফাইলে অবস্থিত। সবচেয়ে গুরুত্বপূর্ণ ফাংশনগুলো হলো:

<!--
- `void phpwasm_init()` – It creates a new PHP context and must be called before running any PHP code.
- `int phpwasm_run(char *code)` – Runs a PHP script and writes the output to /tmp/stdout and /tmp/stderr. Returns the exit code.
- `void phpwasm_refresh()` – Destroy the current PHP context and starts a new one. Call it after running one PHP script and before running another.
-->

- `void phpwasm_init()` – এটি একটি নতুন PHP কনটেক্সট তৈরি করে এবং যেকোনো PHP কোড চালানোর আগে অবশ্যই এটি কল করতে হবে।
- `int phpwasm_run(char *code)` – এটি একটি PHP স্ক্রিপ্ট চালায় এবং আউটপুট /tmp/stdout এবং /tmp/stderr-এ লেখে। এক্সিট কোড রিটার্ন করে।
- `void phpwasm_refresh()` – বর্তমান PHP কনটেক্সট ধ্বংস করে একটি নতুন কনটেক্সট শুরু করে। একটি PHP স্ক্রিপ্ট চালানোর পর এবং অন্য একটি স্ক্রিপ্ট চালানোর পূর্বে এটি কল করুন।

<!--
Refer to the inline documentation in [`php_wasm.c`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/php_wasm.c) to learn more.
-->

আরও জানতে সরাসরি [`php_wasm.c`](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/php/php_wasm.c)-এর ইনলাইন ডকুমেন্টেশন দেখুন।

<!--
## Build configuration
-->

## বিল্ড কনফিগারেশন

<!--
The build is configurable via the [Docker `--build-arg` feature](https://docs.docker.com/engine/reference/commandline/build/#set-build-time-variables---build-arg). You can set them up through the `build.js` script, just run this command to get the usage message:
-->

বিল্ড প্রক্রিয়াটি [Docker `--build-arg` feature](https://docs.docker.com/engine/reference/commandline/build/#set-build-time-variables---build-arg)-এর মাধ্যমে কনফিগার করা যায়। আপনি `build.js` স্ক্রিপ্টের মাধ্যমে এগুলো সেট করতে পারেন, ব্যবহারের নির্দেশাবলী দেখতে এই কমান্ডটি চালান:

```sh
npx nx recompile-php:jspi php-wasm-web -- --help
```

<!--
**Selected build options:**
-->

**নির্বাচিত বিল্ড অপশনসমূহ:**

<!--
This list highlights debug and basic build settings. For the full set of options, run the help command above; see [the build script](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/build.js) for platform-specific defaults.
-->

এই তালিকায় ডিবাগ এবং প্রাথমিক বিল্ড সেটিংসগুলো তুলে ধরা হয়েছে। সম্পূর্ণ অপশনগুলোর তালিকার জন্য উপরের সাহায্য কমান্ডটি চালান; প্ল্যাটফর্ম-নির্দিষ্ট ডিফল্ট জানার জন্য [বিল্ড স্ক্রিপ্টটি](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/compile/build.js) দেখুন।

<!--
- `WITH_DEBUG` – `yes` or `no`. Build with DWARF debug information and disable final optimization. See [Debug builds](#debug-builds).
- `WITH_SOURCEMAPS` – `yes` or `no`. Generate WebAssembly source maps and disable final optimization. See [Debug builds](#debug-builds).
- `PHP_VERSION` – The PHP version to build. Use a major/minor version such as `8.4` to select its latest release from [the supported PHP versions](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/supported-php-versions.mjs), or an exact release such as `8.4.25`. The build clones the corresponding `php-<version>` tag from php-src.
- `WITH_LIBXML` – `yes` or `no`, default: `yes`. Whether to build with `libxml2` and the `dom`, `xml`, and `simplexml` PHP extensions (`DOMDocument`, `SimpleXML`, ..).
- `WITH_LIBZIP` – `yes` or `no`, default: `yes`. Whether to build with `zlib`, `libzip`, and the `zip` PHP extension (`ZipArchive`).
-->

- `WITH_DEBUG` – `yes` অথবা `no`। DWARF ডিবাগ তথ্যসহ বিল্ড করে এবং চূড়ান্ত অপ্টিমাইজেশন নিষ্ক্রিয় করে। [ডিবাগ বিল্ড (Debug builds)](#debug-builds) দেখুন।
- `WITH_SOURCEMAPS` – `yes` অথবা `no`। WebAssembly সোর্স ম্যাপ তৈরি করে এবং চূড়ান্ত অপ্টিমাইজেশন নিষ্ক্রিয় করে। [ডিবাগ বিল্ড (Debug builds)](#debug-builds) দেখুন।
- `PHP_VERSION` – যে পিএইচপি সংস্করণটি বিল্ড করতে চান। [সমর্থিত পিএইচপি সংস্করণ](https://github.com/WordPress/wordpress-playground/blob/trunk/packages/php-wasm/supported-php-versions.mjs) থেকে সর্বশেষ রিলিজ নির্বাচন করতে `8.4`-এর মতো প্রধান/গৌণ সংস্করণ, অথবা `8.4.25`-এর মতো নির্দিষ্ট রিলিজ ব্যবহার করুন। বিল্ড স্ক্রিপ্ট php-src থেকে সংশ্লিষ্ট `php-<version>` ট্যাগ ক্লোন করে।
- `WITH_LIBXML` – `yes` অথবা `no`, ডিফল্ট: `yes`। `libxml2` এবং `dom`, `xml` ও `simplexml` PHP এক্সটেনশন (`DOMDocument`, `SimpleXML`, ..) সহ বিল্ড করা হবে কিনা।
- `WITH_LIBZIP` – `yes` অথবা `no`, ডিফল্ট: `yes`। `zlib`, `libzip` এবং `zip` PHP এক্সটেনশন (`ZipArchive`) সহ বিল্ড করা হবে কিনা।
