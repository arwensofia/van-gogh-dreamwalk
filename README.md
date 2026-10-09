# Van Gogh Dreamwalk

Walk inside four Van Gogh paintings, starting from the spot where Vincent stood, and fall between them through a silver pool of memory.

**Live demo:** https://claude.ai/artifact/22C6b8dwwsyDg55T26FLDc

## Paintings

| Scene | Painting | Year |
|---|---|---|
| `starry` | The Starry Night | 1889 |
| `rhone` | Starry Night Over the Rhône | 1888 |
| `cafe` | Café Terrace at Night | 1888 |
| `bedroom` | The Bedroom | 1888 |

## How it works

- **Tracing.** The main shapes in each painting (cypress, spire, boats, people, furniture) are traced as polygons, saved in `art/*-meta.json`.
- **Volume.** Inflation maps (`*-inflate.png`, `*-hulls.png`) push the flat cut-outs out into 3D.
- **Projection.** Three shaders (objects, ground, sky) project the painting from Vincent's viewpoint onto the 3D shapes, so the view from the start position matches the canvas exactly.
- **Behind the cut-outs.** Inpainted fills (`*-skyfill.jpg`, `*-landfill.jpg`) show what the cypress, boats, people and furniture hide when you walk around them.
- **Beyond the frame.** Nothing is mirrored. Each layer (sky, land) spreads outward from the frame edge, and patches of Vincent's own brushwork from the same band of the canvas are blended over it on a triangle grid (Heitz & Neyret's tiling-and-blending), with copies of his stars scattered across the sky. The canvas fades into this across an irregular band inside the frame, so its edge never shows as a line. The painted skyline continues around the horizon. Near the painter the ground, the café's walls and the bedroom's walls and floor are laid with world-scale patches, the bedroom's from its own wall and floor textures (`*-wall.jpg`, `*-floor.jpg`).
- **Brushstroke direction.** Flow and orientation maps (`*-flow.png`, `*-orient.png`) make the stars, sky and water swirl in the direction of the brushstrokes.
- **Sound.** Each scene has an ambient soundscape generated in the browser with the Web Audio API.

Everything is in one file, `index.html`, which loads Three.js r170 from cdnjs. Starry Night loads first and the other three paintings stream in while you play: their textures are uploaded a frame at a time and their shaders compiled in parallel, so walking doesn't stall. The scene renders at a resolution that follows the GPU's measured load, to hold the display's frame rate. Footsteps are synthesised for each ground (grass, grit, cobbles, floorboards) in time with your stride.

## Controls

| Input | Action |
|---|---|
| WASD / arrows | Walk |
| Mouse | Look (Esc frees the cursor) |
| Shift | Run |
| Space | Hop |
| E | Dive into a silver pool to reach the next painting |
| 1–4 | Jump straight to a painting |
| M | Sound on/off |
| H | Show or hide the hint bar |

## Run locally

The page fetches its textures, so serve it over HTTP instead of opening the file directly:

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

## Known limitations

- Beyond the frame, the continuation is Vincent's brushwork and colour, not new subject matter: the sky and land carry on, but no new houses or boats appear there.
- Seen from the side, the cut-outs are rough 2D approximations, not full 3D models.
- The download is about 18 MB, mostly the four full-resolution paintings.

## Credits

The paintings by Vincent van Gogh (1853–1890) are in the public domain.
