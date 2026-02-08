# WannaShip.fyi

Curated database of validated startup ideas with complete playbooks — from idea to first dollar.

## Setup

- **Hosting:** GitHub Pages (enable in repo Settings → Pages)
- **Domain:** Add `CNAME` file, configure DNS at registrar
- **Payments:** Replace checkout placeholder in `index.html` with Dodo Payments link
- **Product delivery:** Send Notion template link via email after purchase (see below)

## Security

- **Never** put the Notion template link in the repo or on the thank-you page — it's publicly accessible
- Send the link only via email after verified purchase
- Use Dodo webhooks + email integration for automated delivery

## Automated Email Delivery

Dodo Payments supports webhooks that trigger emails on `payment.succeeded`. **Recommended:** Resend or AutoSend.

### Option A: Resend (simple, free tier)

1. Sign up at [resend.com](https://resend.com), verify your domain
2. Dodo Dashboard → Webhooks → Add Endpoint → select **Resend**
3. Add your Resend API key
4. Use this transformation (replace `YOUR_NOTION_LINK` with your template URL):

```javascript
function handler(webhook) {
  if (webhook.eventType === "payment.succeeded") {
    const p = webhook.payload.data;
    const NOTION_LINK = "YOUR_NOTION_LINK"; // Add your link here
    webhook.url = "https://api.resend.com/emails";
    webhook.payload = {
      from: "WannaShip <hello@wannaship.fyi>",
      to: [p.customer.email],
      subject: "Your WannaShip playbook is ready",
      html: `<h2>You're in!</h2><p>Hi ${p.customer.name || 'there'},</p><p>Thanks for your purchase. Here's your database:</p><p><a href="${NOTION_LINK}" style="display:inline-block;padding:12px 24px;background:#f59e0b;color:#18181b;text-decoration:none;border-radius:8px;font-weight:600;">Access Your Database</a></p><p>Click the link above, then hit "Duplicate" to get your own copy.</p><p>— WannaShip</p>`,
      text: `Your database: ${NOTION_LINK} - Click "Duplicate" to get your copy.`
    };
  }
  return webhook;
}
```

### Option B: AutoSend

Similar flow — Dodo Dashboard → Webhooks → AutoSend. Create a template in AutoSend with `{{notionLink}}` and pass it in `dynamicData`.

### Manual fallback

Until automation is set up: check Dodo Dashboard for new sales, copy buyer email, send the Notion link manually via Gmail or similar.

## Assets

Add to `assets/`:
- `og-image.png` — 1200×630 for social shares
- `favicon.ico` — site favicon
