# MEMORY.md

## Competitor Watchlist

- Skill: `workspace/skills/x-competitor-watchlist/`
- Monitors 13 DeFi accounts on X/Twitter
- GitHub token stored in `config.json` under `github.token`
- Live site: https://bartega.github.io/klawp/competitor-watchlist.html
- Layout: 4-column grid, Electric Blue (#00F0FF) / Deep Navy (#0A1929)

## CRITICAL REQUIREMENTS (ALWAYS):
1. **Font size**: body must be 20px
2. **3 tweets per card**: always exactly 3 tweet-rows per protocol
3. **Recent tweets only**: within last 3 days
4. **Dropdown embeds**: every tweet needs onclick="toggleEmbed(this)" + hidden tweet-embed blockquote + ▼ arrow
5. **3 columns**: grid-template-columns: repeat(3, 1fr)

## HOW TO SCRAPE TWEETS (MANUAL BROWSER)

Use browser to scrape x.com manually - NOT the x-research API!

1. `browser start --profile openclaw`
2. Visit each account: `browser open https://x.com/aave`
3. Take snapshot to see tweets (use twitter.com for embeds, not x.com)
4. Copy real tweet URLs to template

Accounts: @aave, @0xfluid, @lifiprotocol, @sparkdotfi, @JumperExchange, @Morpho, @yearnfi, @compoundfinance, @VenusProtocol, @lista_dao, @hyperlendx, @jup_lend, @eulerfinance

## WORKFLOW - ALWAYS FOLLOW THIS:

1. Edit: `skills/x-competitor-watchlist/assets/template.html` (ONLY SOURCE OF TRUTH)
2. Copy to output: 
   ```bash
   cp skills/x-competitor-watchlist/assets/template.html vercel-deploy/public/competitor-watchlist.html
   cp skills/x-competitor-watchlist/assets/template.html competitor-watchlist.html
   ```
3. Push to GitHub:
   ```bash
   git add -f competitor-watchlist.html vercel-deploy/public/competitor-watchlist.html
   git commit -m "Update"
   git push
   ```

## NEVER edit these files directly:
- competitor-watchlist.html
- vercel-deploy/public/competitor-watchlist.html
