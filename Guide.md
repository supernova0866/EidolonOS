# EidolonOS — Guide

Full documentation for EidolonOS (EOS). If you're just looking for config fields, see `Config.md` in your template repo.

---

## What is EidolonOS?

EidolonOS is a GitHub-based personal site builder. You clone a template repo, fill in a `config.json`, enable GitHub Pages, and your site is live. No build tools, no CLI, no dependencies.

The template repo contains your personal data and config. The core repo (this one) stores the optional module HTML files that get fetched and injected into your page at runtime.

---

## How it works

1. You clone the template repo
2. `index.html` loads and reads your `config.json`
3. It builds the hero, about, and socials sections from your config
4. For each enabled module (`projects`, `hobbies`, `discord`), it fetches the corresponding HTML file from this core repo via raw GitHub URL
5. The fetched HTML is injected into the page DOM
6. All styles are already in `index.html` so modules render correctly immediately

---

## File Structure

### Core repo (this repo)
```
EidolonOS/
├── modules/
│   ├── projects.html    ← projects section chunk
│   ├── hobbies.html     ← hobbies section chunk
│   └── discord.html     ← discord presence section chunk
├── Guide.md             ← you are here
├── README.md
└── LICENSE
```

### Template repo (what you clone)
```
EidolonOS-Template/
├── index.html           ← hero + about + socials + all CSS + requester logic
├── projects.html        ← full projects page
├── config.json          ← your configuration
├── Config.md            ← quick config reference
├── README.md
├── LICENSE
└── data/
    ├── projects.json    ← your projects list
    ├── things.json      ← your hobbies list
    └── quotes.json      ← your quotes list
```

---

## Setup

### 1. Fork the template repo
Go to [EidolonOS-Template](https://github.com/supernova0866/EidolonOS-Template) and click Fork. Name it anything — your site will live at:
```
https://yourusername.github.io/reponame
```

### 2. Enable GitHub Pages
Repo → Settings → Pages → Source → `main` branch → Save.
Give it a minute to go live.

### 3. Edit config.json
Open `config.json` and fill in your details. See `Config.md` for every field explained.

### 4. Edit data files
- `data/projects.json` — add your projects (only needed if `modules.projects: true`)
- `data/things.json` — add your hobbies (only needed if `modules.hobbies: true`)
- `data/quotes.json` — add your quotes (only needed if `quotes: true`)

### 5. You're live
Visit your GitHub Pages URL. If something looks outdated, hard refresh with `Ctrl + Shift + R`.

---

## Modules

Modules are optional sections that load from this core repo. Enable them in `config.json` under `modules`.

### Projects
```json
"modules": {
  "projects": true
}
```
Reads from `data/projects.json`. Shows the first 3 projects on the homepage. If you have more than 3, a "View all" button appears automatically linking to `projects.html`.

Each project in `data/projects.json`:
```json
{
  "name": "Project Name",
  "desc": "Short description.",
  "tags": ["Tag1", "Tag2"],
  "status": "ongoing",
  "redirect": "https://github.com/you/project"
}
```
- `status` options: `ongoing` `paused` `planned` `completed` — remove field to show no badge
- `redirect` — full URL or `null` for no link

---

### Hobbies
```json
"modules": {
  "hobbies": true
}
```
Reads from `data/things.json`. Renders a 2-column card grid.

Each hobby in `data/things.json`:
```json
{
  "icon": "🎮",
  "title": "Gaming",
  "desc": "Short description."
}
```
- `icon` — emoji or full image URL

---

### Discord
```json
"modules": {
  "discord": {
    "enabled": true,
    "id": "your_discord_user_id"
  }
}
```
Shows a live Discord presence card powered by [Lanyard](https://github.com/phineas/lanyard). Displays your status, current activity, and Spotify if you're listening.

**Important — you must join the Lanyard Discord server for this to work:**
👉 [discord.gg/lanyard](https://discord.gg/lanyard)

Without joining, the presence fetch will fail and the card will show an error.

#### How to find your Discord user ID
1. Open Discord
2. Settings → Advanced → Enable Developer Mode
3. Right click your profile → Copy User ID

---

## Themes

EOS comes with Light and Dark by default. Extra themes can be enabled in `config.json`:
```json
"theme": {
  "default": "dark",
  "extras": {
    "midnight": true,
    "sepia": false,
    "ivory": false,
    "cobalt": false,
    "terminal": false,
    "cli": false
  }
}
```

| Theme | Description |
|---|---|
| `light` | Clean warm light — always enabled |
| `dark` | Default dark — always enabled |
| `midnight` | Deep purple dark |
| `sepia` | Warm brown, book-like feel |
| `ivory` | Soft warm light |
| `cobalt` | Deep blue |
| `terminal` | Green on black, computer aesthetic |
| `cli` | Hacker green with typewriter effect on scroll |

---

## Navbar
```json
"navbar": true
```

Enables a side navigation panel toggled by a 3-line button at the top left. Auto-populates with links to all active sections — no manual configuration needed.

---

## Age Field

The `age` field in `config.json` accepts multiple formats:

| Format | Example |
|---|---|
| `DD-MM-YYYY` | `"29-08-2009"` |
| `YYYY-MM-DD` | `"2009-08-29"` |
| Unix timestamp | `"1251504000"` |
| `null` | hides the age display entirely |

The age pill shows two numbers — years completed and the year currently running. Set `null` if you'd rather not show your age.

---

## Quotes
```json
"quotes": true
```

Reads from `data/quotes.json` — a plain array of strings:
```json
[
  "Your first quote.",
  "Another quote.",
  "Keep adding as many as you want."
]
```

Quotes shuffle randomly. A reroll button lets visitors cycle through them. Set `quotes: false` to hide the quote block entirely.

---

## Socials

All social fields accept full profile URLs. Set `null` or remove the field to hide that card.

Supported platforms: `github` `instagram` `twitter` `spotify` `youtube` `steam`
```json
"socials": {
  "github": "https://github.com/yourusername",
  "instagram": "https://instagram.com/yourhandle",
  "twitter": "https://x.com/yourhandle",
  "spotify": "https://open.spotify.com/user/yourid",
  "youtube": "https://youtube.com/@yourchannel",
  "steam": "https://steamcommunity.com/id/yourid"
}
```

---

## Troubleshooting

**Site looks outdated after pushing changes**
GitHub Pages caches aggressively. Hard refresh with `Ctrl + Shift + R`. Usually resolves within a few minutes.

**Discord card shows "could not reach Lanyard"**
You need to join [discord.gg/lanyard](https://discord.gg/lanyard) with the Discord account whose ID you used. Without joining the server, Lanyard has no data for your account.

**Scroll line not visible**
Known issue with CSS variable resolution on some hosts. Already fixed in the latest version of `index.html`.

**Module not loading**
Check that the module is set to `true` in `config.json` and that the corresponding data file exists in `data/`. Open browser DevTools console for specific error messages.

**Age not showing**
Make sure your date format matches one of the accepted formats. If using `DD-MM-YYYY`, the day must come first — `29-08-2009` not `08-29-2009`.

---

## Contributing

Found a bug or have a feature idea? Open an issue or submit a pull request.
Suggestions form → [link]

---

## Support

If EOS helped you, consider supporting the project.

⭐ Star the repo
☕ [Ko-fi](#)
