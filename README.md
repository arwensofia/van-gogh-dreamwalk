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
- **Beyond the frame.** Panorama fills (`*-skyfill.jpg`, `*-landfill.jpg`) extend each painting past its edges using texture quilting. The Bedroom also has its own wall and floor textures.
- **Brushstroke direction.** Flow and orientation maps (`*-flow.png`, `*-orient.png`) make the stars, sky and water swirl in the direction of the brushstrokes.
- **Sound.** Each scene has an ambient soundscape generated in the browser with the Web Audio API.

Everything is in one file, `index.html`, which loads Three.js r170 from cdnjs. Starry Night loads first and the other three paintings stream in while you play.

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

- Near the frame edges, the brushwork is stretched or mirrored, since nothing was painted beyond the canvas.
- Seen from the side, the cut-outs are rough 2D approximations, not full 3D models.
- The download is about 18 MB, mostly the four full-resolution paintings.

## Credits

The paintings by Vincent van Gogh (1853–1890) are in the public domain.
