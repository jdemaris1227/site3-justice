# Carroll County PAC — Site Deployment Guide

Three sites. Each deploys independently to Vercel in under 5 minutes.

---

## THE THREE SITES

| Folder | Purpose | Suggested Domain |
|---|---|---|
| `site1-pac` | PAC attack site — navy/gold, openly partisan, full Buenger scandal, donate/volunteer | electpsoras.com or carrollcountypac.com |
| `site2-voter-guide` | Neutral voter guide — newspaper aesthetic, side-by-side comparison, sourced | carrollcountyvotes2026.com or carrollcountystatesattorney2026.com |
| `site3-justice` | Accountability/justice site — dark, bold, Brady Rule focus, the Buenger case in detail | carrollcountyjustice.org |

---

## DEPLOYING TO VERCEL

### Option A — Drag & Drop (Fastest)
1. Go to vercel.com — sign in or create account
2. Click "Add New Project"
3. Click "Deploy" → "Browse" and select the site folder (e.g. `site1-pac`)
4. Vercel auto-detects the `vercel.json` and deploys as static
5. You get a preview URL immediately (e.g. `site1-pac-xyz.vercel.app`)
6. Add your custom domain in Project Settings → Domains

### Option B — GitHub (Recommended for ongoing updates)
1. Create three GitHub repos (one per site)
2. Push each folder's contents to the corresponding repo
3. In Vercel, "Import Git Repository" → connect each repo
4. Automatic redeploys on every push

---

## AFTER DEPLOYMENT — REQUIRED CHANGES

### 1. Wire the Donate Button (Site 1)
Find this line in `site1-pac/index.html`:
```
Replace this with your ActBlue or Anedot donation link.
```
Replace the modal content with a redirect to your actual donation processor URL.

**Recommended processors:**
- Anedot (anedot.com) — better for Republican-aligned PACs, lower fees
- ActBlue (actblue.com) — more name recognition, standard for MD races

### 2. Add Your Domain to Footer Disclosures
When you have domains, update the footer text in each site:
- Site 1: change `carrollcountypac.com` reference
- Site 2: change `carrollcountystatesattorney2026.com` reference  
- Site 3: change `carrollcountyjustice.org` reference

### 3. Wire the Volunteer Form (Site 1)
The volunteer form currently shows a success message but doesn't send data anywhere.
Options:
- **Easiest:** Use Formspree (formspree.io) — add `action="https://formspree.io/f/YOUR_ID"` to the form tag, free tier handles 50 submissions/month
- **Better:** Use Netlify Forms if deploying to Netlify instead of Vercel
- **Best:** Wire to a Google Sheet via Zapier or Make.com

### 4. Add Psoras Headshot (If Available)
All three sites try to load the headshot from:
`https://opposeallanculver.com/wp-content/uploads/2025/12/psoras-headshot.jpg`
If that URL breaks or returns 404, the fallback is the "GP" initials circle.
For a permanent fix, host the image yourself and update the `src` attribute.

### 5. Meta Tags / SEO
Add these to each site's `<head>` when domains are confirmed:
```html
<meta property="og:title" content="[Site Title]">
<meta property="og:description" content="[Site Description]">
<meta property="og:url" content="https://yourdomain.com">
<meta property="og:type" content="website">
<link rel="canonical" href="https://yourdomain.com">
```

---

## CHATBOT NOTES

All three chatbots call the Anthropic API directly from the browser. The API key is handled server-side by Anthropic's proxy — no key needed in the HTML.

The chatbots are pre-loaded with:
- Full Buenger scandal timeline with judge quotes
- The "cleared is a cover story" rebuttal
- SB 763 and SB 896 opposition testimony
- George Psoras platform details
- Carroll County voting information

To update chatbot knowledge (when MPIA documents arrive):
1. Open the site's `index.html`
2. Find the `const SYSTEM_PROMPT =` or `const voterGuideSystem =` or `const justiceSystem =` variable
3. Add new facts to the relevant section
4. Save and redeploy

---

## GOOGLE ANALYTICS

Add this before `</head>` in each site to track traffic:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```
Replace `G-XXXXXXXXXX` with your Google Analytics 4 measurement ID.

---

## FACEBOOK PIXEL

For paid ad conversion tracking, add this before `</head>`:
```html
<script>
!function(f,b,e,v,n,t,s){if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};if(!f._fbq)f._fbq=n;
n.push=n;n.loaded=!0;n.version='2.0';n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];s.parentNode.insertBefore(t,s)}(window,
document,'script','https://connect.facebook.net/en_US/fbevents.js');
fbq('init', 'YOUR_PIXEL_ID');
fbq('track', 'PageView');
</script>
```

---

*Carroll County Small Businesses PAC · March 2026*
