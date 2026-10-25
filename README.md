# Weibo Android AdGuard Filter (Experimental)

An experimental AdGuard filter based on two user-provided PCAPdroid captures of the Weibo Android app, reviewed on 2026-10-26.

## Status

**Experimental — not yet validated on-device.** The captures show several ad-related endpoints and one ad-impression monitoring request containing ad IDs and third-party tracking URLs. This is enough to propose candidate rules, but not proof that blocking each request removes visible ads or is free of side effects.

## Observed in captures

- `adstrategy.biz.weibo.com` — `POST /v3/strategy/ad`; ad strategy endpoint.
- `bootpreload.uve.weibo.com` — `POST /v2/ad/preload`; ad preload endpoint.
- `mediadata.api.weibo.com` — `POST /wbapplua/wbpullad.lua`; ad-pull endpoint by path name.
- `ef-dongfeng.tanx.com` — `GET /tanx`; Tanx ad-related request.
- `df.tanx.com` — `GET /spf3`; Tanx-related request.
- `api.weibo.cn` — `GET /2/h5/comonitor`; requests contained `action=show`, ad IDs, and encoded third-party measurement URLs, consistent with ad-impression tracking.

The captures also contain ordinary Weibo API, login, image, video, push, and SDK traffic. The filter intentionally does **not** block entire Weibo domains, the generic encrypted `dns.weibo.cn/v3/d` endpoint, or generic SDK click/action endpoints, because those may serve unrelated app functions.

## Filter URL

Add this URL to AdGuard for Android Custom filters:

`https://raw.githubusercontent.com/psv1kdump/weiboblockads_PCAPdroid/main/weibo-adguard.txt`

Rules are scoped to `$app=com.sina.weibo`. This package ID is assumed to be the official Weibo Android app; verify the package ID on your device. If your installed app uses another package ID, the app-scoped rules will not match. Check the AdGuard filtering log if your version handles app modifiers differently.

## Install and test

1. Open AdGuard for Android and find **Settings → Content blocking → Custom filters** (labels may vary by version).
2. Add the filter URL above, then enable/update it.
3. Force-stop Weibo and reopen it.
4. Test startup, feed, post/article details, images, video, comments, search, login, and notifications.
5. Inspect AdGuard's filtering log to confirm which rules matched.

If any normal feature fails, disable the custom filter and identify the matching rule in the log. If ads remain, record the exact screen/ad type and capture a focused trace.

## Limitations

- The rules are experimental; no controlled before/after device test has been performed.
- Some traffic is encrypted, and the capture does not reveal all request paths or response bodies.
- Blocking measurement requests may reduce tracking but may not hide the corresponding ad.
- Ad delivery can use shared APIs or other endpoints, so these rules will not remove every ad.
- URL blocking cannot selectively rewrite arbitrary HTTPS response bodies.
- Weibo may change endpoints over time.

## Privacy

Do not commit raw PCAP/PCAPNG files to this repository. Captures can contain account identifiers, device identifiers, tokens, URLs, and other sensitive metadata.
