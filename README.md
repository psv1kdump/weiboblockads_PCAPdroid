# Weibo Android AdGuard Filter (Experimental)

An experimental AdGuard filter draft based on a user-provided PCAPdroid capture of the Weibo Android app, reviewed on 2026-10-26.

## Status

**Experimental — not yet validated on-device.** The capture exposed several ad-related-looking HTTP endpoints. Endpoint names and hostnames are evidence for candidate rules, not proof that blocking each request will remove visible ads or be free of side effects.

## Capture observations

The capture included plaintext HTTP requests to these hosts/endpoints:

- `adstrategy.biz.weibo.com` — `POST /v3/strategy/ad` (ad strategy endpoint by name)
- `bootpreload.uve.weibo.com` — `POST /v2/ad/preload` (ad preload endpoint by name)
- `mediadata.api.weibo.com` — `POST /wbapplua/wbpullad.lua` (pull-ad endpoint by name)
- `ef-dongfeng.tanx.com` — `GET /tanx?...` (Tanx ad/tracking-related endpoint)
- `df.tanx.com` — `GET /spf3?...` (Tanx-related endpoint)

The capture also contained Weibo API, login, image, video, push, and SDK traffic. This filter deliberately does not block whole Weibo domains or broad SDK hosts, to reduce the risk of breaking normal app features.

## Rules file

`weibo-adguard.txt` contains path-specific candidate rules, scoped to `$app=com.sina.weibo`.

The package ID is assumed to be the official Weibo Android package. Verify it on your device. If your installed app uses a different package ID, the `$app` modifier will not match. If your AdGuard version does not support/apply the app modifier, inspect the filtering log.

## Install

1. Open AdGuard for Android and locate **Settings → Content blocking → Custom filters** (labels may vary by version).
2. Add this filter URL: `https://raw.githubusercontent.com/psv1kdump/weiboblockads_PCAPdroid/main/weibo-adguard.txt`
3. Enable/update the filter.
4. Force-stop Weibo and reopen it. Test startup, feed, article/post details, images, video, comments, login, and notifications.
5. Inspect AdGuard's filtering log to confirm which rules match.

## Validation checklist

- [ ] Startup ad reduced/removed
- [ ] Feed ads reduced/removed
- [ ] Pop-up/interstitial ads reduced/removed
- [ ] Login and account functions work
- [ ] Feed, images, video, comments and search work
- [ ] No repeated errors or broken loading

If ads remain, capture another PCAPdroid trace focused on the exact ad scenario. If any feature breaks, disable the filter and report the affected screen and matching log entry.

## Limitations

- One capture only; no before/after test has been performed.
- Some traffic is encrypted; this capture may not expose all request paths or response bodies.
- Ad delivery may use other endpoints or shared APIs, so these rules will not necessarily remove all ads.
- URL blocking cannot selectively rewrite arbitrary HTTPS response bodies.
- Rules may become outdated as Weibo changes its SDKs or endpoints.

## Privacy

Do not commit raw PCAP files to this repository. Captures may contain device identifiers, account identifiers, URLs, tokens, and other sensitive metadata.
