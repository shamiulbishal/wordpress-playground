---
slug: /developers/apis/javascript-api/blueprint-json-in-api-client
---

<!--
# Blueprints JSON and the API Client
-->

# ব্লুপ্রিন্টস JSON এবং API ক্লায়েন্ট (Blueprints JSON and the API Client) {#blueprints-json-and-the-api-client}

<!--
The Playground API client can be initialized with a [JSON Blueprint](/blueprints). This is a convenient way of preconfiguring it in any way you like without worrying about progress bars and fetching remote files:
-->

প্লেগ্রাউন্ড API ক্লায়েন্ট একটি [JSON ব্লুপ্রিন্ট](/blueprints)-এর সাথে ইনিশিয়ালাইজ (শুরু) করা যেতে পারে। প্রোগ্রেস বার এবং রিমোট ফাইল ফেচিং নিয়ে চিন্তা না করে আপনার পছন্দ অনুযায়ী ক্লায়েন্টকে পূর্বনির্ধারিত কনফিগারেশনে সাজানোর এটি একটি সুবিধাজনক উপায়:

```ts
import { startPlaygroundWeb } from 'https://playground.wordpress.net/client/index.js';

const client = await startPlaygroundWeb({
	iframe: document.getElementById('wp'),
	remoteUrl: `https://playground.wordpress.net/remote.html`,
	blueprint: {
		preferredVersions: {
			wp: '6.3',
			php: '8.3',
		},
		steps: [
			{ step: 'login' },
			{
				step: 'installPlugin',
				pluginData: {
					resource: 'wordpress.org/plugins',
					slug: 'gutenberg',
				},
			},
		],
	},
});
await client.isReady();
```

<!--
Running a JSON Blueprint is only possible during the initialization of the API client.
-->

একটি JSON ব্লুপ্রিন্ট চালানো শুধুমাত্র API ক্লায়েন্টের ইনিশিয়ালাইজেশন চলাকালীনই সম্ভব।

<!--
If this is sufficient for your needs, read more about [JSON Blueprints](/blueprints).
-->

যদি এটি আপনার চাহিদার জন্য যথেষ্ট হয়, তবে [JSON ব্লুপ্রিন্ট](/blueprints) সম্পর্কে আরও জানুন।

<!--
If you need to work with an already initialized client, you should look into [Blueprint functions](/developers/apis/javascript-api/blueprint-functions-in-api-client).
-->

যদি আপনাকে ইতিমধ্যে ইনিশিয়ালাইজ করা একটি ক্লায়েন্ট নিয়ে কাজ করতে হয়, তবে আপনার [ব্লুপ্রিন্ট ফাংশন (Blueprint functions)](/developers/apis/javascript-api/blueprint-functions-in-api-client) দেখা উচিত।
