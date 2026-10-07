# How to Access Seedance 2.0 (October 2026): apps, official API and API hosts

Every working way to use ByteDance's Seedance 2.0 video model today, what each route asks of you (account, region, top-up, limits), and how to make a first video on each. Facts come from the providers' own pages, opened on 2026-10-07.

<sub>Last checked: 2026-10-07. Community guide, not affiliated with ByteDance, BytePlus, CapCut or any provider below. Routes change; open the linked page before you rely on one.</sub>

## Start here

| You want to… | Take | Section |
|---|---|---|
| make a few videos in a browser, outside China | Dreamina | [Route 1](#route-1-dreamina-web-international) |
| use it in China, in Chinese | Doubao or Jimeng AI | [Route 2](#route-2-doubao-and-jimeng-ai-china) |
| call it from code through ByteDance itself | BytePlus ModelArk | [Route 3](#route-3-byteplus-modelark-official-api) |
| call it from code with prepaid credit | an API host | [Route 4](#route-4-api-hosts) |

## Status on 2026-10-07

| Route | Kind | Seedance 2.0 models offered | What you need | Status |
|---|---|---|---|---|
| [Dreamina](https://dreamina.capcut.com/tools/seedance-2-0) | ByteDance creative web app | 2.0, 2.0 Mini | a Dreamina (CapCut) account | live; the page says "now live globally" |
| [Doubao](https://www.doubao.com) | ByteDance chat assistant, China | 2.0 | a Doubao login | live; "free after login" per its own page |
| [Jimeng AI](https://jimeng.jianying.com) (即梦AI) | ByteDance creative app, China | 2.0 family; the app's July 2026 notes add 2.0 Fast | a Jimeng account; credits or membership | live (China App Store listing, in-app purchases) |
| [BytePlus ModelArk](https://docs.byteplus.com/en/docs/modelark/seedance-2-0) | official API, international | 2.0, 2.0 Fast, 2.0 Mini | an account, an API key, and a balance above USD 30 (or a USD 30 savings plan, or a resource pack) to activate | live |
| API hosts (OpenRouter, Replicate, fal, ArgoLink and others) | third-party APIs | varies, see Route 4 | an account and credit | live |

Not covered: Volcengine Ark, ByteDance's cloud for mainland China. Seed's Chinese page links its experience center, but the Seedance pricing page did not load for us, so no figures are given.

## Route 1: Dreamina (web, international)

Dreamina is ByteDance's own creative platform outside China and calls itself "the official website for accessing Seedance 2.0".

1. Sign in at [dreamina.capcut.com](https://dreamina.capcut.com/tools/seedance-2-0) and open **AI Video**.
2. Write the prompt, and add references with **+**: Dreamina allows up to 12 per video, among them 9 images, 3 videos and 3 audio clips.
3. Pick **Dreamina Seedance 2.0** (or **2.0 Mini**, which Dreamina recommends for faster generation and fewer credits), then duration, aspect ratio and resolution, and press **Generate**.
4. Upscale or interpolate if you want, then **Download**.

Cost: free daily credits to try it, then a credit subscription. Plan prices are shown in local currency and change with promotions, so check the pricing panel when you sign in.

## Route 2: Doubao and Jimeng AI (China)

| | [Doubao](https://www.doubao.com) (豆包) | [Jimeng AI](https://jimeng.jianying.com) (即梦AI) |
|---|---|---|
| What it is | chat assistant that also makes videos | image and video creation platform, web and app |
| Seedance 2.0 | "Seedance 2.0 视频生成模型现已全面接入豆包，现在登录即可免费使用" (fully integrated, free after login) | Seedance 2.0 family; the App Store notes list "上线Seedance2.0 fast模型" (July 2026) and Seedance 2.5 (August) |
| Payment | free per the site's own description | credits; membership plans in the app |
| Audience | China market, Chinese interface | China market; app listed in the China App Store |

In Doubao, ask for a video in the chat. In Jimeng, open video generation and choose the Seedance model. Both are aimed at mainland-China users; this guide did not test sign-up from abroad.

## Route 3: BytePlus ModelArk (official API)

BytePlus is ByteDance's cloud outside China, and ModelArk is its model API. The three models are `dreamina-seedance-2-0-260128`, `dreamina-seedance-2-0-fast-260128` and `dreamina-seedance-2-0-mini-260615`.

| Requirement or limit | Value |
|---|---|
| Activation | balance above USD 30, or an AI Savings Plan of USD 30+, or a Seedance resource pack |
| Rate limits, individual account | 180 requests a minute, 3 jobs at once (4K: 15 a minute, 1 at once) |
| Rate limits, enterprise account | 600 a minute, 10 at once (4K: 15 a minute, 1 at once) |
| Price | $7.0 per million tokens at 480p/720p, $7.7 at 1080p, $4.0 at 4K; 5 s at 720p ≈ $0.76 |
| Real faces | reference images or videos of real people are not accepted directly; BytePlus offers preset digital characters, trusted outputs of its own models, and authorised likeness assets |
| Results | `video_url` valid for 24 hours; task records queryable for 7 days |

```bash
# Create a task
curl https://ark.ap-southeast.bytepluses.com/api/v3/contents/generations/tasks \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "dreamina-seedance-2-0-260128",
    "content": [{"type": "text", "text": "A paper lantern drifts down a night river, slow tracking shot"}],
    "resolution": "720p",
    "ratio": "16:9",
    "duration": 5
  }'

# Poll it: queued -> running -> succeeded (or failed / cancelled); the result has content.video_url
curl https://ark.ap-southeast.bytepluses.com/api/v3/contents/generations/tasks/$TASK_ID \
  -H "Authorization: Bearer $ARK_API_KEY"
```

Images, video and audio go into the same `content` array with a `role` (`first_frame`, `last_frame`, `reference_image`, `reference_video`, `reference_audio`). Sources: [Seedance 2.0 tutorial](https://docs.byteplus.com/en/docs/modelark/seedance-2-0), [create task](https://docs.byteplus.com/en/docs/modelark/create-video-generation-task-api), [retrieve task](https://docs.byteplus.com/en/docs/modelark/get-video-generation-task-api), [pricing](https://docs.byteplus.com/en/docs/modelark/model-pricing).

## Route 4: API hosts

Hosts resell the same model behind their own API, usually with a prepaid balance and no activation threshold. Prices and terms differ a lot; this table lists what each host's Seedance 2.0 page said on 2026-10-07.

| Host | Seedance 2.0 from | Fast | Mini | Billed by | Worth knowing |
|---|---:|---:|---:|---|---|
| [ArgoLink](https://argolink.io/gh-seedance-2-0-access-guide) | $0.053/s (480p) | $0.042/s | $0.026/s | output second | 4K to $0.58/s; failed jobs free; accepts fal and OpenRouter field names |
| [OpenRouter](https://openrouter.ai/bytedance/seedance-2.0) | $0.0673/s (480p) | $0.0404/s | $0.0336/s | tokens, at BytePlus's rates | 5.5% fee on card top-ups (5% crypto) |
| [Replicate](https://replicate.com/bytedance/seedance-2.0) | $0.08/s (480p) | $0.07/s | $0.04/s | output second | higher per-second rate when you send a video |
| [Kie.ai](https://kie.ai/seedance-2-0) | $0.095/s (480p) | $0.059/s | — | credits per second | prices marked beta; +10% credits on high-tier top-ups |
| [PiAPI](https://piapi.ai/seedance-2-0) | $0.10/s (480p) | $0.048/s | $0.042/s | output second | `-less-restriction` variants accept reviewed face references |
| [WaveSpeedAI](https://wavespeed.ai/models/bytedance/seedance-2.0/text-to-video) | $0.12/s (480p) | $0.10/s | $0.06/s | output second | with a reference video, input + output seconds at a lower rate |
| [Atlas Cloud](https://www.atlascloud.ai/models/bytedance/seedance-2.0/text-to-video) | $0.2419/s (720p) | — | — | tokens | its page states the 720p rate |
| [fal](https://fal.ai/models/bytedance/seedance-2.0/text-to-video) | $0.3024/s (720p) | $0.2419/s (720p) | $0.0721/s (480p) | tokens | 0.6× rate when a video is attached |

<sub>Lowest tier each page states, no video input. "—": we did not find that variant on the host. Full like-for-like price tables and the with-video arithmetic belong in a comparison; this table is for choosing where to sign up.</sub>

### First video through ArgoLink

Create a key in the ArgoLink console, then three calls:

```bash
BASE=https://argolink.io/v1
# 1. submit -> {"request_id": "..."}
curl $BASE/videos/generations -H "Authorization: Bearer $SEEDANCE_API_KEY" -H "Content-Type: application/json" \
  -d '{"model": "seedance-2.0-mini", "prompt": "A paper lantern drifts down a night river, slow tracking shot", "duration": 5, "resolution": "480p"}'
# 2. poll every 10-15 s until status is done (or failed / expired, which cost nothing)
curl $BASE/videos/$REQUEST_ID -H "Authorization: Bearer $SEEDANCE_API_KEY"
# 3. download
curl -L $BASE/videos/$REQUEST_ID/content -H "Authorization: Bearer $SEEDANCE_API_KEY" -o lantern.mp4
```

That clip costs 5 × $0.026 = $0.13. The same request with `"model": "seedance-2.0"` and `"resolution": "4k"` renders the only 4K Seedance tier, at $0.58/s. Field reference: the [Seedance 2.0 model page](https://argolink.io/en/models/seedance-2.0).

## Which route fits

| Situation | Route |
|---|---|
| A few clips to try the model, no code | Dreamina (outside China), Doubao (in China) |
| A product or pipeline that needs an SLA from ByteDance | BytePlus ModelArk, enterprise account |
| Code, small budget, no USD 30 activation | an API host with prepaid credit |
| 4K output | `seedance-2.0` only: BytePlus (1 job at a time), Replicate, WaveSpeedAI, Kie.ai, fal, OpenRouter, ArgoLink |

## FAQ

**Is Seedance 2.0 available outside China?**
Yes. Dreamina states it is live globally, and BytePlus ModelArk sells the API internationally. API hosts add more ways in.

**Is there a free way to use Seedance 2.0?**
Dreamina gives free daily credits, and Doubao describes Seedance 2.0 as free after login. The APIs are paid; the cheapest per-second rate found on 2026-10-07 was $0.026/s for Mini at 480p.

**What is the official Seedance 2.0 API?**
BytePlus ModelArk outside mainland China (model ID `dreamina-seedance-2-0-260128`). Inside mainland China, ByteDance Seed's Chinese Seedance 2.0 page points to Volcengine (火山引擎) instead.

**Why does BytePlus ask for USD 30 first?**
Its Seedance 2.0 and 2.5 models are activated only for accounts with a balance above USD 30, a USD 30+ savings plan, or a resource pack. The hosts in Route 4 bill each job from a prepaid balance instead; check each one's minimum top-up.

**Can I use photos of real people?**
Not directly on BytePlus, which routes portraits through preset characters, its own trusted outputs, or authorised likeness assets. Hosts apply their own review; PiAPI, for example, sells separate `-less-restriction` variants that accept face references after review.

**What is the difference between 2.0, Fast and Mini?**
Same inputs and reference limits (9 images, 3 videos, 3 audio clips). 2.0 renders up to 4K; Fast and Mini stop at 720p and cost less, Mini the least.

## Contributing

Found a route that works, or one that stopped working? Open an issue with the link and the date you checked it.

## License

MIT
