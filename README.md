# Cosmin Tutea — CV website

**Live site:** https://ioksan.github.io

A one-page CV website with a real-time 3D Earth, built as my mini-project ("WEB CV") for PFD200 Professional Development (FdSc Computing, NCG / Elizabeth School of London, 2026).

The first screen shows my name, role and the three things a recruiter needs: the CV as a PDF, LinkedIn and GitHub. A 3D Earth turns beside them, with six orbiting labels that jump to the CV sections. Scrolling brings the CV up on a light sheet over the scene.

## Why it looks like this

Version 1 opened on the 3D scene alone, and a visitor had to scroll before learning who I was. Recruiters spend around 7.4 seconds on a first look at a CV and respond best to simple layouts with clear headings (HR Dive, 2018, reporting the Ladders eye-tracking study). Version 2 keeps the Earth as the one striking element and puts everything else in plain, scannable order:

- name, role and actions on the first screen;
- dates in a left column and results in a right column, read top to bottom;
- one column of text under 72 characters per line;
- the page prints as a clean CV with the scene removed.

The typeface is IBM Plex, a quiet nod to the IBM equipment my IT career started on.

## Tech stack

| Area | Technology |
| --- | --- |
| Markup and layout | HTML5, CSS3 (grid, custom properties, print styles) |
| 3D rendering | Three.js r128, self-hosted (`three.min.js`) |
| Shaders | Custom GLSL: day/night Earth blend, cloud layer, 18,000-particle morph |
| Post-processing | Bloom (bright pass + separable Gaussian blur), ACES tone mapping, vignette |
| Type | IBM Plex Sans and IBM Plex Sans Condensed (Google Fonts) |
| Hosting | GitHub Pages |

## How it works

1. **Content first.** The CV is ordinary HTML. It shows before any texture loads and still works with JavaScript off.
2. **Intro.** Each particle stores a point on the Earth and a point in a spiral galaxy; a vertex shader blends between them over a timed 3.5-second intro, so slow devices finish at the same moment.
3. **Earth.** A fragment shader mixes NASA day and night textures by the angle to a moving sun and adds a highlight on water. Phones load a 2K texture instead of the 4K one.
4. **Navigation.** Six sprites orbit the Earth; their screen positions are projected each frame and HTML labels follow them. The side column has the same six links for keyboard and mobile users.
5. **Battery.** Once the CV covers the screen, the render loop stops drawing.

The code is commented section by section in `index.html`.

## Security and privacy

These choices follow the threat table in my portfolio (Task 1.4):

- **No email or phone number on the page.** Contact goes through LinkedIn, so the address cannot be scraped for spam or phishing. The downloadable CV is a public version without them.
- **Three.js is self-hosted** instead of loaded from a CDN, so the page does not run third-party script it cannot verify.
- **Content Security Policy** in a meta tag limits scripts, styles, fonts and images to known sources and blocks forms and `<base>` changes.
- **External links** use `rel="noopener noreferrer"`.
- **GitHub account** protected with two-factor authentication.

## Accessibility

- Automated audit with axe-core against WCAG 2.1 A and AA: no violations (21 October 2026).
- Skip link, labelled sections and visible focus outlines for keyboard users.
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

- The Earth textures total about 1.2 MB. They load after the text, but on a slow connection the globe appears a few seconds later.
- The 3D scene is demanding on older phones; the orbit labels are hidden on small screens.
- Google Fonts are loaded from Google's servers.
- Not yet tested with a screen reader.

## Credits and licence

- Three.js © 2010–2021 Three.js authors, MIT licence (see `THREE_LICENSE.txt`).
- Earth textures based on NASA imagery, in `assets/`.
- Recruiter reading time: HR Dive (2018) *Recruiters spend 7.4 seconds looking at your resume*, reporting the Ladders eye-tracking study.
- Site code and content © 2026 Andrei-Cosmin Tutea. Built under the MiniPause brand.
