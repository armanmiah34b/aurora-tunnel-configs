# aurora-tunnel-configs

Config delivery for the **Aurora Tunnel** Android app.

The admin panel (a Cloudflare Worker) commits two **encrypted** bundles here
after every publish, and the app downloads them straight from this repo's raw
file URLs — so per-user config traffic never reaches the Worker.

| File | Contents |
|---|---|
| `aurora_configs_2026_v2.enc` | server list + categories, AES-256-GCM envelope |
| `aurora_app_settings.enc` | lock / force-update / branding settings |
| `<deviceId>.json` | per-device trial + VIP status mirror (so the app reads status from here instead of writing KV on every launch) |

Nothing in this repository is plaintext: every bundle is an encrypted envelope
(`sensei:v2:` shared key by default, `sensei:v3:` once `APP_CERT_SHA256` is set
on the Worker). This repo is public **on purpose** — the ciphertext is useless
without the key that ships inside the APK, and the app needs an unauthenticated
host it can hit on the free tier.

Do not edit files here by hand; publish from the admin panel
(`/Aurora-tunnel/admin`) so validation and encryption always run first.

The full panel snapshot (credential hashes, device ledger) lives in the
**private** `aurora-tunnel-backups` repo instead — never here.
