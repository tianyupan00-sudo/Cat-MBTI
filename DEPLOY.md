# CatTI Deployment Guide

Everything you need is in the `dist/` folder. Just drag it into Cloudflare Pages and you're live.

---

## Before deploying — fill in your Xiaohongshu link (2 minutes)

Open `dist/index.html` in any text editor, find (Cmd/Ctrl+F) these two placeholders, and replace them:

| Placeholder | Replace with | Example |
|---|---|---|
| `XHS_PROFILE_ID` | Your XHS profile user ID | `5f8a3b1c000000000100abcd` |
| `XHS_HANDLE` | Your XHS display name | `猫猫 Sam` |

**How to find your XHS profile ID:** Open your profile on xiaohongshu.com (web version), look at the URL. It's the alphanumeric string at the end:

```
https://www.xiaohongshu.com/user/profile/5f8a3b1c000000000100abcd
                                          ^^^^^^^^^^^^^^^^^^^^^^^^ this part
```

Both placeholders appear **4 times** (once per footer — landing, quiz, result, gallery). Your editor's "Replace All" handles it in one shot.

---

## Deploy to Cloudflare Pages (drag-drop, no GitHub needed)

1. Go to **https://dash.cloudflare.com/sign-up** and create a free account (email only, no card).

2. Once in the dashboard, click **Workers & Pages** in the left sidebar → **Create** → **Pages** tab → **Upload assets**.

3. Project name: `catti` (or whatever you want — this becomes part of your URL like `catti.pages.dev`).

4. Click **Drag and drop** and drop the **entire `dist/` folder** into the upload area. Wait for all 18 files to finish uploading.

5. Click **Deploy site**. After ~30 seconds you'll get a URL like `https://catti.pages.dev` or `https://catti-a1b.pages.dev`.

6. Open that URL in your phone and laptop. Verify:
   - Landing page loads
   - Images show up
   - Quiz works end-to-end
   - Share-as-image works
   - Credit footer link opens your XHS profile

That's it. The site is public now.

---

## Recommended: turn on Web Analytics (free, 2 minutes)

Free, privacy-preserving (no cookies, no user tracking), and you can see: daily visitors, what country they're from, which pages they hit, completion rate.

1. In Cloudflare dashboard → **Analytics & Logs** → **Web Analytics** → **Add a site**.
2. Enter your `catti.pages.dev` URL.
3. Copy the `<script>` snippet it gives you.
4. Open `dist/index.html`, find the closing `</body>` tag, paste the snippet right before it.
5. Re-upload `dist/index.html` via the Pages dashboard (same project → Create new deployment → Upload just the updated HTML, or drop the whole `dist/` folder again to replace).

---

## About mainland China access

Cloudflare Pages serves from a global CDN. **Mainland access works but isn't guaranteed fast** — typical load time is 2–5 seconds, occasionally slower during peak hours or network issues.

**For a "丢出去玩玩" launch this is fine.** If CatTI catches on and you want guaranteed fast China load times, options for later:

| Option | Cost | Needs |
|---|---|---|
| Cloudflare China Network | ~$20/mo | ICP beian + Cloudflare paid plan |
| Tencent Cloud COS + CDN | ~¥10-50/mo | ICP beian, real-name |
| Aliyun OSS + CDN | ~¥10-50/mo | ICP beian, real-name |
| Vercel + Netlify mirror | Free | Slow in China too — not a real solution |
| GitCode Pages (domestic GitHub-alt) | Free | Chinese phone # to register |

ICP beian (备案) takes 2-4 weeks and requires a domain you own. Only worth it if the site is growing.

---

## Updating the site later

Any time you want to push changes:
1. Edit files locally
2. Re-upload the whole `dist/` folder via Pages dashboard → your project → **Create deployment**
3. Or: connect it to a GitHub repo for automatic deploys on `git push` (one-time setup under project **Settings** → **Builds & deployments**).

---

## What's in `dist/`

```
dist/
├── index.html          (~120 KB — the app, slim, no embedded images)
├── og.jpg              (~100 KB — preview image shown when link is shared)
└── images/             (~3 MB total — 16 cat portraits, lazy-loaded per result)
    ├── INFJ.jpg  ENFJ.jpg  ISFJ.jpg  ESFJ.jpg
    ├── INTJ.jpg  ENTJ.jpg  ISTJ.jpg  ESTJ.jpg
    ├── INFP.jpg  ENFP.jpg  ISFP.jpg  ESFP.jpg
    └── INTP.jpg  ENTP.jpg  ISTP.jpg  ESTP.jpg
```

**First-paint experience:**
- Landing page: only HTML loads (~120 KB). Fast.
- Quiz: still no images loaded.
- Result: loads exactly 1 image (~180 KB) for the user's result cat.
- Gallery: loads all 16 on demand.

Result: On a decent 4G connection, the user sees your landing page in ~1 second instead of the 8-15 seconds the original 3.8MB file would've taken.

---

## Share verification

After deploying, test that link previews work correctly by pasting your URL into:

- **WeChat** (朋友圈 or a chat): should show the og.jpg preview with title + description
- **Twitter**: use https://cards-dev.twitter.com/validator to preview
- **Facebook/Instagram share**: use https://developers.facebook.com/tools/debug/ to preview and refresh cache
- **Telegram/iMessage/Slack**: auto-previews

If a preview looks wrong on Twitter/FB, click "Re-scrape" on their debug tool — they cache for 7 days otherwise.

---

## Next steps (Phase 2, if you want them later)

- A/B test landing page copy
- Track completion rate by question (find drop-off)
- Add an analytics event for which MBTI people get (to see the distribution)
- Custom short domain (e.g., `catti.cat`, $15-30/year on Porkbun or Namecheap)
- ICP beian + mainland hosting (only if viral)

Have fun with the launch 🐾
