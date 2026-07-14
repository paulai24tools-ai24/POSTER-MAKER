# PosterLab AI — Master Prompt & Project Memory

**Project:** Paul and Rakesh Project
**Working title:** PosterLab AI
**Purpose of this file:** Paste this whole document into a new Claude chat (or reread it yourself) to resume this project with zero context loss. It records the original idea, the honest critique, every decision made, and what's still open.

---

## 1. One-liner

An AI web app that turns any uploaded photo into professional poster/cover artwork (sports, movie, magazine, album, event) through a conversational "AI creative director" instead of a prompt box.

## 2. Original vision (as brought to the table)

Core loop: **Upload photo → AI analyzes it → AI proposes creative directions in plain language → user picks a style or types edits in natural language → AI composition engine builds the poster → export in multiple formats (Instagram, Story, YouTube thumbnail, print, wallpaper).**

Key elements from the original spec:
- No prompt-writing for the user — AI infers subject count, mood, colors, category (sports/portrait/product/pet/etc.) and explains it back in plain language.
- Style library: 20+ presets (Sports, Movie Poster, Album Cover, Magazine Cover, Streetwear, Cyberpunk, Vintage, Bauhaus, Nike/Adidas-campaign style, etc.).
- "AI design elements" applied contextually: torn paper, halftone, film grain, light leaks, smoke, stadium lights, newspaper clippings, tape, stickers.
- AI-generated typography (VAMOS, CHAMPIONS, UNSTOPPABLE, etc.) that adapts to style.
- AI color direction (Black & Gold, Spain Red & Yellow, AMOLED Black, etc.).
- Natural-language smart editing ("remove the second person," "add dramatic lighting," "use Spain's colors").
- Signature differentiator proposed: an **AI Creative Director panel** that proactively pitches 3-5 directions after analyzing the photo, rather than a blank prompt box.
- Export targets: PNG/JPEG/PDF, print-ready, multiple aspect ratios.

## 3. The reality check (critique before building)

**Market is crowded, not empty.** Confirmed via research — this is not a greenfield idea:
- **Canva Magic Media** already does prompt + style-preset poster generation, plus Magic Edit (prompt-based background/object edits) and Magic Eraser.
- **Adobe Express** lets users upload a product/logo/portrait photo and regenerate poster templates around it.
- **Krea** has a dedicated "AI Movie Poster Generator" — upload a photo, get a cinematic poster with titles/taglines in ~10 seconds, free, no signup. This is nearly identical to the core loop proposed here.
- **Cutout.pro, Recraft, Piktochart, Quillbot** — all overlapping prompt-to-poster tools.
- **Posterly, AI Poster Maker** — mobile apps doing guided-input poster generation.

**Verdict:** the feature list is a plausible merge of things competitors already ship individually. No stated wedge/differentiation against incumbents with far more distribution and budget.

**The hardest claimed feature is a real open problem, not a prompting trick.** "AI Composition Engine" that autonomously decides layer hierarchy, typography placement, and negative space like a human designer does not exist off-the-shelf. Diffusion models still render in-image text unreliably — which is *why* Krea/Canva/Adobe composite typography as a separate layer on top of a generated image rather than asking the model to render it natively. Any real build needs to plan for text-as-overlay, not text-as-generated-pixels.

**No image-generation API is available in the Claude.ai chat/artifact environment** — only web search, code execution, and (inside artifacts) text/vision calls to the Claude API. True generative photo editing (background replacement, adding smoke/stadium lights, removing a person) requires a real image model (Flux Kontext, Gemini image edit / "Nano Banana," Ideogram, etc.) wired up outside this chat.

## 4. Strategic recommendation (open, not yet finalized)

Don't build the full 24-style, everything-app spec as v1. Narrow to **one niche with recurring, high-intent use** before building anything more:
- Candidate A: **Sports / match-day fan posters** — recurring weekly use, WhatsApp-status culture, underserved by generic tools.
- Candidate B: **Local event flyers** for small organizers — recurring B2B-ish use, real willingness to pay.

Philosophy applied: **Idea → Validation → Plan → Prototype → Feedback → Iteration → Portfolio Asset.** Validation (landing page / demand test) was flagged as arguably higher-leverage than building more product before knowing anyone wants it.

**This niche decision has NOT been made yet — it's the biggest open question.**

## 5. What was actually built (v0 clickable prototype)

Given a choice between (a) narrowing the niche into a real MVP PRD, (b) a landing page to test demand, (c) a technical architecture/API plan, or (d) a clickable UI prototype — **(d) was chosen.**

**File:** `code/posterlab-prototype.html` — single-file, no build step, opens in any browser.

**What's real, not mocked:**
- Image analysis: the uploaded photo is sent as an actual image to the Claude API, which returns subject count, mood, category, dominant colors, and 2 recommended poster styles, in the AI creative director's voice.
- Copywriting: every poster headline/tagline/subtext and every "director's note" in the chat panel comes from a live Claude text call, grounded in the photo analysis and whatever edit instruction the user types.
- Compositing and export: the final poster preview and downloaded PNG are built with a real Canvas 2D pipeline (color-grade filter, gradient overlays, procedural grain, typography placement) running on the user's actual uploaded image. Multi-format export (feed 1:1, story 9:16, print 3:4) genuinely re-crops and redraws.

**What's mocked / simplified:**
- The "poster-ization" itself is CSS/canvas styling across 4 style presets (Match Day, Cinematic, Editorial, Vintage) — not true generative image editing. It cannot remove a person, replace a background, or add smoke/light-leak elements as photorealistic pixels. That gap is intentional and disclosed in-app.

**Stack:** vanilla HTML/CSS/JS, Google Fonts (Bebas Neue / Playfair Display / IBM Plex Mono / Inter), Claude API (`claude-sonnet-4-6`) called client-side for vision + text, Canvas 2D API for compositing/export. No backend, no auth, no storage — everything is client-side and ephemeral.

**Design direction:** near-black charcoal canvas (#121116), single coral/red signature accent (#ff4b3e), condensed poster-style display type — justified by the subject matter itself (movie/sports poster culture), not a generic AI-design default.

## 6. Honest scorecard (as given)

- Original vision doc, as a vision/PRD: **6/10**
- Original vision doc, as an actionable v1 build plan: **3/10** (too broad, no niche, no differentiation, ignores typography rendering limits)
- v0 clickable prototype: **7/10** — real AI where it counts, clean interaction design, honest about its own limits. To reach world-class: needs a real image-editing model behind it, and a named target user.

## 7. Open questions (unresolved — pick up here)

1. **Which niche?** Sports/match-day vs. local event flyers vs. something else entirely — not decided.
2. **Demand validation** — no landing page / fake-door test has been built or run yet.
3. **Real image-generation API** — none is wired in yet. Candidates to evaluate: Flux Kontext, Gemini image edit ("Nano Banana"), Ideogram — need cost, latency, and text-rendering-quality comparison.
4. **Differentiation / wedge** — still no one-sentence answer to "why this over Krea's free movie-poster tool or Canva Magic Media."
5. **Business model** — not discussed yet (freemium credits? subscription? per-export pricing?).
6. **Target platform for real build** — recommended next step is Claude Code (needs a persistent codebase + real API keys + backend), not further chat-based artifacts.

## 8. Recommended next actions (in order)

1. Test `posterlab-prototype.html` on 5-10 real photos across categories — does the AI's style recommendation and copy actually feel sharp, or generic?
2. Decide the niche (open question #1) — pick one, kill the rest for v1.
3. Either: (a) build a one-page demand-validation landing page for that niche, or (b) move straight to Claude Code and wire in a real image-editing API scoped to that one niche.
4. Revisit business model and differentiation once real user reactions exist — don't guess them in the abstract.

## 9. How to resume this project

Paste this entire file back to Claude (or reread it yourself) and say: *"Continue the PosterLab AI project from here — pick up at [open question #]."* Everything above is sufficient to resume without re-explaining the idea, the critique, or what's already been built.
