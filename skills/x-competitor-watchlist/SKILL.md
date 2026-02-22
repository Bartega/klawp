---
name: x-competitor-watchlist
description: Monitor X/Twitter accounts and generate an HTML competitor watchlist with embedded tweets, TVL data, and branding. Use when: (1) User wants to track competitor accounts on X, (2) Building a competitor intelligence dashboard, (3) Setting up automated social media monitoring with TVL, (4) Creating a DeFi/tech watchlist page.
---

# X Competitor Watchlist

⚠️ **CRITICAL REQUIREMENTS - NEVER DEVIATE FROM THESE:**

1. **3 COLUMNS**: Use grid-template-columns: repeat(3, 1fr)
2. **3 TWEETS PER CARD**: Each protocol card must have exactly 3 tweet-rows  
3. **RECENT POSTS ONLY**: Tweets must be from the last 3 days max (no older posts)
4. **DROPDOWN EMBEDS**: Every tweet must have:
   - `onclick="toggleEmbed(this)"` on the tweet-summary
   - A hidden tweet-embed blockquote below it
   - A tweet-expand arrow (▼) that changes when open

Generate an HTML page monitoring X/Twitter accounts with dropdown tweet embeds, TVL data, and brand theming.

## CRITICAL WORKFLOW RULES:

⚠️ **template.html IS THE ONLY SOURCE OF TRUTH**

1. NEVER edit competitor-watchlist.html or vercel-deploy/public/ directly
2. ALWAYS edit skills/x-competitor-watchlist/assets/template.html
3. After editing template.html, copy it to output locations:
   ```bash
   cp skills/x-competitor-watchlist/assets/template.html vercel-deploy/public/competitor-watchlist.html
   cp skills/x-competitor-watchlist/assets/template.html competitor-watchlist.html
   ```
4. Then push to GitHub

## Quick Start

1. Configure accounts in `config.json`:
```json
{
  "accounts": [
    {"username": "aave", "tvl": "$25.98B"},
    {"username": "yearnfi", "tvl": "$289M"}
  ],
  "postsPerAccount": 3,
  "outputPath": "competitor-watchlist.html"
}
```

2. Add avatar images to `avatars/` folder (username.png format)

3. Run the scraper via OpenClaw browser

4. Open the generated HTML file.

## Features

- **Four-column grid** layout
- **Dropdown tweet embeds** - click to expand
- **TVL data** from DeFiLlama API
- **Profile avatars** in card headers
- **Brand theming** - Electric Blue (#00F0FF), Deep Navy (#0A1929)
- **Square corners** styling

## Configuration

Edit `config.json` to add/remove accounts and adjust TVL values. GitHub token is stored in `config.json` under `github.token`.

## Adding Kamino TVL to Header

To add Kamino TVL to the top right of the header (dynamically fetched from DeFiLlama):

1. Add CSS to `<style>`:
```css
header { position: relative; }
.kamino-tvl { position: absolute; top: 1rem; right: 1rem; background: var(--card-bg); border: 1px solid var(--border); padding: 0.5rem 0.75rem; }
.kamino-tvl-label { font-size: 0.6rem; color: var(--text-muted); text-transform: uppercase; }
.kamino-tvl-value { font-size: 0.85rem; font-weight: 700; color: var(--green); }
```

2. Add HTML inside `<header>`:
```html
<div class="kamino-tvl">
    <div class="kamino-tvl-label">Kamino TVL</div>
    <div class="kamino-tvl-value">$0B</div>
</div>
```

3. Run the fetch-tvl script to update dynamically:
```bash
cd skills/x-competitor-watchlist
chmod +x scripts/fetch-tvl.sh
./scripts/fetch-tvl.sh
```

The script fetches from `https://api.llama.fi/protocol/kamino` and updates the TVL value in the HTML.

## Running the Skill

1. Copy template to output:
```bash
cp assets/template.html vercel-deploy/public/competitor-watchlist.html
```

2. Fetch latest Kamino TVL:
```bash
cd skills/x-competitor-watchlist
./scripts/fetch-tvl.sh
```

3. Generate dynamic highlights:
```bash
python3 scripts/generate-highlights.py
```

4. Push to GitHub:
```bash
git add -f vercel-deploy/public/competitor-watchlist.html
git commit -m "Update competitor watchlist"
git push https://[token]@github.com/Bartega/klawp.git main
```

5. Trigger Vercel deployment (optional):
```bash
curl -X POST "https://api.vercel.com/v1/integrations/deploy/prj_zNAgyhgVYIlOo5KOTzHcmkA7uwkQ/N3DP9lkLTA"
```

## TVL Updates & GitHub Push

Cron job runs 3x daily (8 AM, 8 PM) and 1 PM:
- Fetches latest TVL from DeFiLlama API
- Updates the HTML
- Auto-pushes to GitHub

**Live URL:** https://Bartega.github.io/klawp/competitor-watchlist.html

## Output

Generates `competitor-watchlist.html` with:
- Protocol cards in 4-column grid
- 3 tweets per account with engagement stats
- **Tweet summaries must be at least 10 words** - expand short tweets into descriptive summaries
- TVL displayed next to account name
- Profile avatars
- Dropdown expand/collapse for tweet embeds
- Dark theme with brand colors

## Scraping Real Tweets (MANUAL BROWSER)

To get REAL tweets from X, you must manually scrape x.com using the browser:

1. Start browser: `browser start --profile openclaw`
2. Open each account: `browser open https://x.com/aave`
3. Take snapshot to see tweets
4. Copy real tweet URLs from the page
5. Update template.html with real URLs

Manual scraping = using browser to visit x.com, NOT using x-research API.

### Accounts to scrape:
- @aave, @0xfluid, @lifiprotocol, @sparkdotfi, @JumperExchange, @Morpho, @yearnfi, @compoundfinance, @VenusProtocol, @lista_dao, @hyperlendx, @jup_lend, @eulerfinance
