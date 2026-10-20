# Cosmin Tutea — CV website

**Live site:** https://ioksan.github.io

A single-page CV website with an interactive 3D Earth, built as my mini-project for PFD200 Professional Development (FdSc Computing, NCG / Elizabeth School of London, 2026).

The page opens with a particle galaxy that forms into a photorealistic Earth. Six orbiting nodes link to the CV sections: About, Experience, Projects, Skills, Education and Contact. Scrolling moves from the 3D scene into the readable CV.

## Why I built it this way

I wanted a portfolio that shows front-end and graphics skills, not only a list of jobs. The content still reads as a normal CV for a recruiter who just scrolls. If WebGL is not available, the 3D scene is skipped and the sections remain readable.

## Tech stack

| Area | Technology |
| --- | --- |
| Markup and layout | HTML5, CSS3 (custom properties, flexbox, backdrop blur) |
| 3D rendering | Three.js r128, self-hosted (`three.min.js`) |
| Shaders | Custom GLSL: day/night Earth blend, cloud layer, 18,000-particle morph |
| Post-processing | Bloom (bright pass + separable Gaussian blur), ACES tone mapping, chromatic aberration, film grain, vignette |
| Branding | MiniPause animated lemniscate logo, drawn in SVG from JavaScript |
| Hosting | GitHub Pages |

## How it works

1. **Intro morph.** Each particle stores two positions: a point on the Earth's sphere and a point in a spiral galaxy. A vertex shader blends between them with a smoothstep curve.
2. **Earth.** A fragment shader mixes NASA day and night textures by the angle to a moving sun, adds specular light on water, and fades with the intro.
3. **Navigation.** Six sprites orbit the Earth. Their screen positions are projected each frame and HTML labels follow them. Clicking a label scrolls to its section.
4. **Post-processing.** The scene renders to an off-screen target, then passes through bright-pass, blur and composite shaders. If render targets fail, the page falls back to a plain render.

## Security and privacy

These choices follow the threat table in my portfolio (Task 1.4):

- **No email or phone number on the page.** Contact goes through LinkedIn, so the address cannot be scraped for spam or phishing. The downloadable CV is a public version without them.
- **Three.js is self-hosted** instead of loaded from a CDN, so the page does not run third-party script it cannot verify.
- **Content Security Policy** in a meta tag limits scripts, styles, fonts and images to known sources and blocks forms and `<base>` changes.
- **External links** use `rel="noopener noreferrer"`.
- **GitHub account** protected with two-factor authentication.

## Accessibility

- Visible focus outlines for keyboard users.
- `prefers-reduced-motion` turns off smooth scrolling and the scroll cue animation.
- Readable layout below 600 px wide; the particle count drops on small screens.
- Content stays readable without JavaScript.

## Project management

Planned and tracked in Jira as a Scrum backlog (project WEBCV3) across five two-week sprints, from setup in August 2026 to deployment and write-up in October 2026.

## Run locally

No build step. Clone the repository and serve the folder:

```bash
git clone https://github.com/Ioksan/Ioksan.github.io.git
cd Ioksan.github.io
python -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` directly from disk also works.

## Known limitations

- `index.html` is about 1.6 MB because the Earth textures are embedded as data URIs. Moving them to separate compressed image files would speed up the first load.
- The 3D scene is demanding on older phones.
- Google Fonts are loaded from Google's servers.

## Credits and licence

- Three.js © 2010–2021 Three.js authors, MIT licence (see `THREE_LICENSE.txt`).
- Earth textures based on NASA imagery.
- Site code and content © 2026 Andrei-Cosmin Tutea. Built under the MiniPause brand.
