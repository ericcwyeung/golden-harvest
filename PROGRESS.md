# GHFC Website — Build Progress

## Project
- Path: /home/user/golden-harvest (web-db-user: Vite+React19+Tailwind4+tRPC+Drizzle+wouter)
- Pages: Home(scrollytelling), Services, Team, Contact + Privacy/Terms. Info-only, NO visitor auth. Theme light. bg-cream.
- Preview URL: https://3000-il19o9f47n6kc7u9tzbjd.sandbox.easy-peasy.ai/
- Domain: user has parked domain, no hosting knowledge — give deploy/DNS guidance at END only.
- DEPLOY RULE: never production same turn as preview; user must explicitly approve production.

## ROUND-2 DONE (v3)
- Photos: eye-line normalized (scripts/normalize_team.py, IOD=150px, eyes at 40% top, 800x1000 white bg). All 6 consistent (incl new James AI placeholder from /home/user/uploads/James_Zhang.png). Team.tsx now object-center.
- ANIMATION: new client/src/components/home/WheatField.tsx — CSS-animated swaying wheat stalks (wheat-sway kf), drifting pollen motes (pollen-rise kf), wind gust shimmer (wind-gust kf). Density+color shift with journey stage (green->gold). Used in Home hero (stage=1) AND Scrollytelling (stage=(active+.5)/len). index.css has the keyframes + prefers-reduced-motion guard. Scenes also got slow ken-burns zoom.
- pnpm check PASSES. Deploying preview next.
- STILL PENDING from user: real bios, confirm email (info@ghfc.ca placeholder), James real info+photo (using AI placeholder), LinkedIn/WeChat real links ("#").

## (archived) ROUND-2 STATUS NOTES (v3 in progress)
- Photo normalize v2 (FACE_FRAC .26) STILL inconsistent per vision review: Shirley/Eva too small+low, Henry too big+high, Eric too low.
  ROOT CAUSE: haar face-box size varies per person (diff forehead/chin inclusion). 
  v3 FIX (DO THIS): switch normalize_team.py to EYE-LINE alignment. Detect eyes (haarcascade_eye) within face ROI,
  compute eye-center + inter-ocular distance (IOD). Scale so IOD = constant (e.g. 0.13*OUT_W=104px), place eye-center at
  y=0.42*OUT_H, x=center. This is far more stable than face box. Fallback to face-box if <2 eyes found (manual nudge per person).
  Per-person manual offset dict may be needed for Shirley(.png, smaller src 848x1264) since eyes may not detect.
  Output 800x1000 jpg q88 to client/public/img/team/. Rebuild /tmp/team_sheetN.jpg + re-view until consistent.
- James AI placeholder regenerated + saved to /home/user/uploads/James_Zhang.png (1536x2752), now runs through normalizer too.
- After photos uniform: set Team.tsx img to object-cover object-center (currently object-top).
- THEN: animated moving crops/wheat on Home (Scrollytelling.tsx). index.css already has @keyframes drift + sway.

## ROUND-2 STATUS NOTES (old)
- Photo normalize v1 done (scripts/normalize_team.py, opencv haar). Faces detected OK but contact-sheet review
  shows still inconsistent head sizes/headroom because haar face boxes vary + James is OLD AI placeholder at diff scale.
  TODO v2: tighten — make FACE_FRAC smaller/looser for the tight ones OR detect eyes for alignment; regen a NEW James AI
  headshot matching the same crop spec, OR just re-run with tuned params. Re-make /tmp/team_sheet.jpg + re-view to confirm.
  Team.tsx currently object-top — change to object-center after photos are uniform.
- Photos source in /home/user/uploads (Shirley_Li.png Eva_Yuen.jpg Henry_Chiu.jpg Randal_To.jpg Eric_Yeung.jpg). James = no upload, keep/regen placeholder.
- THEN do the animated moving crops on Home (see task 1).

## CURRENT TASK (user feedback round 2)
1. HOME: user wants the CROPS / field to MOVE — vivid animated movement, not a static photo.
   Plan: add animated wheat field. Approach = layered parallax + CSS/SVG animated swaying wheat
   over the scene photos, plus subtle drifting/ken-burns on bg. Consider an animated foreground
   layer of wheat stalks (SVG WheatMark instances) swaying with @keyframes sway, wind gradient,
   floating particles/pollen. Keep mobile-friendly + prefers-reduced-motion guard.
   File to edit: client/src/components/home/Scrollytelling.tsx (+ index.css keyframes already has drift/sway).
2. TEAM PHOTOS: user uploaded 5 real photos to /home/user/uploads/ :
   Shirley_Li.png (848x1264), Eva_Yuen.jpg (3696x5174), Henry_Chiu.jpg (3797x5304),
   Randal_To.jpg (3611x5416), Eric_Yeung.jpg (3557x5336). NO James Zhang yet (keep AI placeholder james.jpg).
   All are clean head&shoulders on white/light bg. Problem = different scale/crop -> normalize.
   PLAN: use face detection to crop all to IDENTICAL 4:5 ratio, same head-size ratio + headroom,
   pad onto uniform WHITE (#FFFFFF) bg, output ~800x1000 jpg q88 -> overwrite client/public/img/team/{shirley,eva,henry,randal,eric}.jpg
   Tool: python opencv (pip install opencv-python-headless) haar cascade face detect; fallback manual ratios.
   Then Team.tsx already uses object-cover object-top — after normalization switch to object-cover object-center.
3. Email unknown for now (keep info@ghfc.ca placeholder). James Zhang info coming later.
4. LinkedIn/WeChat still "#" placeholders. Bios still PLACEHOLDER.

## Brand (index.css CSS vars + @theme tokens) — DONE
navy #1A3050, navy-d #0E1E32, gold #C49A3C, gold-l #E2C47A, gold-p #F2E4C4,
cream #F5F0E8, linen #ECEAE4, charcoal #2A2A2A, stone #888888.
Tailwind: bg-navy bg-navy-d text-gold bg-cream text-charcoal (via @theme inline).
Fonts: Cormorant Garamond (serif/headings), Raleway (sans/body) in client/index.html.
.kicker gold eyebrow. .gold-rule gradient line. .ghfc-input form field. .legal-body legal type.
index.css has @keyframes drift (animate-drift) and sway already.

## Files (all built, working, pnpm check passed)
- components/brand/WheatMark.tsx (official wheat SVG, currentColor), Logo.tsx (tone light|dark)
- components/SiteHeader.tsx (sticky, transparent over hero, mobile menu), SiteFooter.tsx (2 offices)
- components/Layout.tsx (<Layout> pt-20; <Layout bare> home), LegalShell.tsx
- components/home/Scrollytelling.tsx — scroll bg crossfade, 7 STAGES, framer-motion, progress rail growing wheat, FloatingGrains
- lib/siteData.ts — COMPANY, SERVICES[6], TEAM[6], STAGES[7]. EDIT CONTENT HERE.
- pages/Home.tsx (hero 04-harvest bg + philosophy + Scrollytelling + services preview + CTA)
- pages/Services.tsx (6 cards lucide icons), Team.tsx (6 cards photo/name/title/bio/email/phone/linkedin/wechat)
- pages/Contact.tsx (form -> trpc.contact.submit, success state, offices), Privacy.tsx, Terms.tsx (non-SMS)
- App.tsx routes: / /services /team /contact /privacy /terms + NotFound
- drizzle/schema.ts contactSubmissions table PUSHED. server/routers.ts contact.submit (zod, randomUUID).
- trpc client import: `import { trpc } from "@/lib/trpc"`

## Assets client/public/img/ (optimized)
scenes/01-soil.jpg 02-seed.jpg 03-growth.jpg 04-harvest.jpg
team/shirley.jpg eva.jpg james.jpg(AI placeholder) henry.jpg randal.jpg eric.jpg

## Content facts
Team: Shirley Li (Director, EPC CHS), Eva Yuen, James Zhang, Henry Chiu, Randal To, Eric Yeung — bios PLACEHOLDER.
Contact: 604-783-0990 / 236-521-4980, info@ghfc.ca(placeholder), www.ghfc.ca
Offices: #2401-4710 Kingsway (Metrotower I) Burnaby BC V5H 4M2 ; #555-650 W 41st Ave Vancouver BC
Services: Small Business, Estate, Investment, Tax, Retirement Planning, Risk Management (NO product desc).
STAGES: 01 Planning,02 Finding ground,03 Discovering,04 Seeding,05 Watering,06 Fertilizing,07 Harvest.
