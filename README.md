# Circus Charlie Runner

A browser-based endless runner game inspired by Circus Charlie. Single HTML file, no build step, no dependencies.

## How to Run

### Docker (recommended)

Requires [Docker](https://docs.docker.com/get-docker/) and Docker Compose.

```bash
docker compose up
```

Then open [http://localhost:8092](http://localhost:8092).

To stop:

```bash
docker compose down
```

### Without Docker

Just open `circus-runner.html` directly in any modern browser — no server required.

```
double-click circus-runner.html
```

Or serve it with a local static server:

```bash
# Python
python -m http.server 8080

# Node (npx)
npx serve .
```

Then open `http://localhost:8080/circus-runner.html`.

---

## Controls

| Action | Input |
|--------|-------|
| Jump | Space / Up arrow / Tap JUMP button |
| Double jump | Second tap/press while airborne |

---

## Ad Integration

The game has two ad slots you need to fill with your own credentials.

### 1. Banner Ad (bottom of screen)

**File:** `circus-runner.html` — around **line 165**

Replace the placeholder comment with your real AdSense tag:

```html
<div id="ad-banner">
  <!-- REPLACE THIS COMMENT WITH YOUR ADSENSE CODE -->
  <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX" crossorigin="anonymous"></script>
  <ins class="adsbygoogle"
       style="display:inline-block;width:320px;height:50px"
       data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"
       data-ad-slot="XXXXXXXXXX"></ins>
  <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
</div>
```

Replace:
- `ca-pub-XXXXXXXXXXXXXXXX` → your **Publisher ID** (appears twice)
- `XXXXXXXXXX` → your **Ad Unit ID**

Also delete the fallback `AD` text inside the div.

### 2. Rewarded Ad ("WATCH AD - CONTINUE" button)

**File:** `circus-runner.html` — around **line 408**, inside `function showRewardedAd(onRewarded)`

The function currently uses a 5-second fake ad placeholder. Replace it with your ad network's SDK call. The game expects you to call `onRewarded(true)` when the user finishes the ad and `onRewarded(false)` if it was skipped or errored.

**CrazyGames SDK example:**
```js
function showRewardedAd(onRewarded) {
  CrazyGames.SDK.ad.requestAd('rewarded', {
    adFinished: () => onRewarded(true),
    adError:    () => onRewarded(false),
  });
}
```

**Poki SDK example:**
```js
function showRewardedAd(onRewarded) {
  PokiSDK.rewardedBreak().then(withReward => onRewarded(withReward));
}
```

Other supported networks: Google IMA, IronSource, AdMob (WebView), AppLovin MAX.

---

## Creating a Google AdSense Account

1. Go to [https://adsense.google.com](https://adsense.google.com) and sign in with a Google account.
2. Click **Get started** and enter your website URL (the domain where you'll host the game).
3. AdSense will review your site (can take 1–14 days). Your site must be publicly accessible.
4. Once approved, go to **Ads > By ad unit > Display ads** and create a new ad unit.
5. Choose **Banner** size (e.g. 320×50 for mobile or 728×90 for desktop).
6. Copy the generated `<ins>` snippet — it contains your `ca-pub-XXXXXXXXXXXXXXXX` publisher ID and `data-ad-slot` ID.
7. Paste those values into the banner slot as described above.

> AdSense requires a real domain with live content. It will not approve `localhost` or `file://` URLs.

---

## Hosting

For ads to work, the game must be served from a real HTTPS domain. Options:

- **GitHub Pages** — free, just push to a repo and enable Pages in settings
- **Netlify / Vercel** — drag-and-drop the HTML file
- **Any web host** — upload the single file via FTP/SFTP
