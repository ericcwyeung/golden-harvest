# Golden Harvest Financial Corporation — Website Package

This is the complete source code for the GHFC website (Home, Services, Our Team, Contact + Privacy & Terms).

## What's inside
- React + TypeScript + Vite + TailwindCSS front-end
- tRPC + Drizzle backend (used only for the Contact form submissions)
- All brand assets, fonts wired in, your wheat logo as crisp SVG
- 4 cinematic field background images + 6 team photos

## Running it locally
You'll need Node.js 18+ and pnpm installed.

```bash
pnpm install          # install dependencies
pnpm dev              # start the local dev server (prints a localhost URL)
pnpm build            # build the production version into /dist
pnpm check            # TypeScript type-check
```

## Where to edit content (no deep coding needed)
Almost everything you'll want to change lives in ONE file:

**`client/src/lib/siteData.ts`**
- `COMPANY` — name, email, phone numbers, office addresses, licensing line
- `SERVICES` — the 6 service blocks (title + short blurb)
- `TEAM` — the 6 team members: name, title, credentials, bio, email, phone,
  LinkedIn URL, WeChat URL, and which photo file to use
- `STAGES` — the 7 scrollytelling steps on the home page

### Replacing a team member's bio
In `client/src/lib/siteData.ts`, find the member and replace the `bio:` text.
Currently they all use a shared `PLACEHOLDER`. Just type the real bio in quotes.

### Replacing a team photo
1. Put the new photo in `client/public/img/team/` (e.g. `james.jpg`)
2. For best results it should be a 4:5 portrait, head-and-shoulders, ~800x1000px,
   on a light background. (A helper script lives in `scripts/normalize_team.py`
   if you want to auto-crop a batch consistently — needs Python + opencv + pillow.)
3. The filename in `siteData.ts` (`photo: "/img/team/james.jpg"`) must match.

### Updating LinkedIn / WeChat links
In `siteData.ts`, change each member's `linkedin: "#"` and `wechat: "#"` to the
real URLs. Leave them out (delete the line) to hide that button.

### Updating the company email
Search `info@ghfc.ca` in `siteData.ts` and replace with the real address.

## Legal pages
- Privacy Policy: `client/src/pages/Privacy.tsx`
- Terms of Service: `client/src/pages/Terms.tsx` (non-SMS version)

## Contact form
Submissions are saved to the database via `server/routers.ts` (`contact.submit`).
The table is defined in `drizzle/schema.ts` (`contactSubmissions`). When you host
this, you'll point it at a Postgres database and run `pnpm db:push` once.

## Images / brand
- Backgrounds: `client/public/img/scenes/` (01-soil, 02-seed, 03-growth, 04-harvest)
- Team photos: `client/public/img/team/`
- Logo: drawn in code at `client/src/components/brand/WheatMark.tsx`
- Colors & fonts: `client/src/index.css` (top of file = brand palette)

## Hosting (when ready)
This is a full-stack app (because of the contact form). Easiest paths:
- Deploy the whole thing to a Node host (Render, Railway, Fly.io, or the
  Easy-Peasy production deploy) and connect a Postgres database.
- If you'd rather keep it ultra-simple and drop the database, the contact form
  can be switched to email-only (e.g. Formspree) and the site can then be hosted
  as a static site on Netlify/Vercel.
- Point your parked domain's DNS to whichever host you choose.

Reach back out any time and I can finish hosting + connect your domain for you.
