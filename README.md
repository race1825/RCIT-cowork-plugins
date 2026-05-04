<<<<<<< HEAD
# RCIT Cowork Plugins

A Claude Cowork / Claude Code plugin marketplace by **Race-Conz IT Solutions**, owned by Horace B. "Race" Briones — Proprietor & Project Manager.

This repo bundles four themed plugins built around RCIT's day-to-day delivery work for LGUs, cooperatives, hotels/resorts, and SMBs in the Bicol region.

## Plugins

| Plugin | What it does | Skills inside |
|---|---|---|
| **rcit-writing** | Writing toolkit for proposals, blog posts, web copy, case studies, and RCIT delivery standards. | `RCIT-writing-skill`, `rcit-guidelines`, `enhance-prompt` |
| **rcit-design** | Synthesizes Stitch projects into a semantic `DESIGN.md` design system. | `design-md` |
| **rcit-seo** | Applies meta tags, Open Graph, JSON-LD schema, sitemap, robots.txt, and performance hints to any website project. | `websiteSEO` |
| **caveman** | Ultra-compressed output mode (~75% fewer tokens) for casual / low-stakes work. Explicit-invocation only. | `caveman` |

## Install in Claude Cowork (desktop & web)

1. Open Cowork → **Settings → Plugins → Add marketplace**
2. Paste this URL:
   ```
   https://github.com/race1825/RCIT-cowork-plugins
   ```
3. Pick the plugins you want and click **Install**.
4. Skills will appear in the slash menu (`/rcit-writing-skill`, `/rcit-guidelines`, `/design-md`, `/websiteSEO`, `/caveman`, `/enhance-prompt`).

## Install in Claude Code (CLI / VS Code)

```bash
/plugin marketplace add race1825/RCIT-cowork-plugins
/plugin install rcit-writing@rcit-cowork-plugins
/plugin install rcit-design@rcit-cowork-plugins
/plugin install rcit-seo@rcit-cowork-plugins
/plugin install caveman@rcit-cowork-plugins
```

## Repo structure

```
RCIT-cowork-plugins/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace manifest (lists all 4 plugins)
├── plugins/
│   ├── rcit-writing/
│   │   ├── .claude-plugin/plugin.json
│   │   └── skills/
│   │       ├── RCIT-writing-skill/
│   │       ├── rcit-guidelines/
│   │       └── enhance-prompt/
│   ├── rcit-design/
│   │   ├── .claude-plugin/plugin.json
│   │   └── skills/design-md/
│   ├── rcit-seo/
│   │   ├── .claude-plugin/plugin.json
│   │   └── skills/websiteSEO/
│   └── caveman/
│       ├── .claude-plugin/plugin.json
│       └── skills/caveman/
├── LICENSE
└── README.md
```

## License

MIT. The `caveman` skill is adapted from [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) (MIT) as a single-file opt-in skill.

## About

**Race-Conz IT Solutions** — Cybersecurity, structured cabling, IT consulting, system development, and home automation for the Bicol region. Sophos & Microsoft partner. 18+ years in IT.

Maintained by [Horace B. "Race" Briones](https://github.com/race1825).
=======
# RCIT-cowork-plugins
RCIT-cowork-plugins
>>>>>>> 78e2fbdc63a5380b6c519b66a5edb0e8cc36c9ca
