# Pearl White

Vite static site + one Vercel serverless function (`api/booking.js`).

## Owner settings (change here only)
| Setting | Where |
|---|---|
| Owner email | Vercel env var `OWNER_EMAIL` |
| Owner WhatsApp (digits only, e.g. `639XXXXXXXXX`) | Vercel env vars `OWNER_WHATSAPP_NUMBER` (server) and `VITE_OWNER_WHATSAPP_NUMBER` (chat button) |
| GoHighLevel webhook | Vercel env var `GHL_WEBHOOK_URL` |

Copy `.env.example` to `.env` for local use. Never commit `.env`.

## How a booking flows
Form -> `POST /api/booking` -> the function validates, then sends in parallel:
1. **GHL webhook** with `{name,email,phone,checkIn,checkOut,guests,message}`
2. **Owner email** "New Pearl White Booking Request" via Resend (`RESEND_API_KEY`)
3. **Owner WhatsApp** via Meta WhatsApp Cloud API (`WHATSAPP_CLOUD_TOKEN`, `WHATSAPP_PHONE_NUMBER_ID`)

Channels without credentials are skipped. Note: WhatsApp Cloud API free-text messages only deliver if the owner messaged your business number in the last 24h, otherwise an approved template is needed. Alternative: have a GoHighLevel workflow send the owner an email + WhatsApp/SMS from the webhook.

## Vercel setup
Project > Settings > Environment Variables: add the variables above (all environments), then Redeploy. `VITE_*` values are baked in at build time, so redeploy after changing them.

## Develop
`npm install`, then `npx vercel dev` (runs site + `/api`). `npm run dev` runs the site only; submitting the form needs the API. Build: `npm run build`.
