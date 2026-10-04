# wall-craft

Turn images into wallpapers at an exact screen size. Crop one image to a ratio, or build a collage from
several, then export at a standard resolution. Includes a content-aware fill for empty bars and a spot
heal brush. One HTML file, no build step, no dependencies, no network calls. Open it from disk or serve it
as a static page. Images are decoded and drawn in your browser and never uploaded.

## What it does

### Collage

![Collage tab](docs/screenshot-collage.png)

- Load several images, pick a layout, and choose which image sits in each cell. Click a cell, then click an
  image in the list, or drag an image from the list onto a cell.
- Reorder by dragging the small handle in the top-left corner of a cell onto another cell. The two swap, and
  the whole cell moves, including its own zoom and position, so each picture keeps its framing. A ghost
  thumbnail follows the pointer and the target cell is outlined. Dropping on an empty cell moves the image
  there. With the keyboard, focus a handle and use the arrow keys to swap with the neighbouring cell.
- Every cell has its own reposition, zoom and orientation: drag inside a cell, use the wheel, or the Selected
  panel. Fit width and Fit height set the zoom so that edge of the image matches the cell. The same panel flips,
  rotates and straightens the picture (see Flip, rotate and straighten below).
- **Panels**: columns by rows with a gap, an outer margin and a border of your choice. Gap, margin and border
  are tenths of a percent of the width, so a layout looks the same at every export size.
- **Grid**: columns by rows, edge to edge, no gaps or borders, on whole-pixel edges so there are no seams. One
  row gives borderless vertical strips.
- **Custom**: your own layout of merged and split cells, for a hero image or a bento arrangement. See below.
- **Diagonal**: parallel diagonal cuts with one image per slice. See below.
- **Stack** and **Polaroid**: every loaded image is scattered over an even grid with random tilt and a
  shadow, optionally framed as a polaroid. Drag a card, or the small handle in its corner, to move it (it comes
  to the front). With the keyboard, focus a handle and use the arrow keys to nudge it. Each has its own
  border width and colour. Every card sits wholly inside the usable area, tilt included, so the canvas edge
  and the taskbar strip never cut a picture off.
  - **Stack cards follow their picture's own shape.** The photo area has exactly the proportions of the
    picture, with the border added around it, so a panorama or a tall portrait is shown whole, not cropped
    to a fixed ratio. (Older builds clamped cards to between 0.7 and 1.6.)
  - **Polaroid cards have a square photo area**, like a real polaroid, so a picture is cropped to a square
    at first. Use Fit width to show the whole of a wide picture, or Fit height for a tall one.
  - **Zoom, fit and move the picture inside a card** with the Selected panel (zoom slider, Fit width, Fit
    height, flips, Rotate 90, Straighten, Reset), the same as for a cell. Click a card to choose it. Wheel over a card to zoom its picture,
    and Shift-drag to move the picture inside the card (a plain drag still moves the card). The move follows
    the card's own tilted axes. Filters and the heal brush follow the zoomed picture.
- Stack and polaroid layouts come from a seed. Type one to reproduce an arrangement, or Reshuffle for a new
  one. Changing the seed discards manual card moves.

![Polaroid layout](docs/screenshot-polaroid.png)

#### Taskbar offset

On by default: 48 px at the bottom of a 1080 px high canvas, the height of a standard Windows taskbar. It keeps a strip along one edge clear in every
collage layout (Panels, Grid, Custom, Diagonal, Stack and Polaroid), so nothing sits under the taskbar. Pick
the edge (Off, Bottom, Top, Left or Right) and a size. The size is shown in pixels at the current export size
and scales with it (about 4.4% of the height, so 48 px at 1080 and 64 px at 1440). Saved sessions that still have the old 70 px default are moved to 48 px, and any other size you chose is kept. The strip is whole pixels, and it
shows the background colour or gradient. The preview marks it with a dashed line. Margin is measured from the
edge of the strip, and cards in Stack and Polaroid stay wholly clear of it. Set it to Off to use the whole
canvas. Crop mode ignores it.

#### Filters

![Filters](docs/screenshot-filters.png)

Every slot can have its own filter: each collage cell, slice or card, and the cropped image. Choose a slot
(click a cell, or a card in Stack and Polaroid), then use the Filter panel, which is there in both Crop and
Collage and names the slot it is editing ("overlay" for the cropped image, "panel 2" for a cell, or a card's file name,
shortened to its start and end when it is long, such as `SaveInta...50522_n.jpg`, with the full name on hover). Panel 3 above has Vintage, panel 2 has Noir, panel 4 has Film and panel 5 has Paper.

- **Light**: Light / dark, Contrast and Fade (lifts the blacks for a matte look).
- **Colour**: Saturation, Warmth, Hue, Sepia and Black and white. Sepia and Black and white are blends, so
  50 is half way.
- **Effects**: Blur, Vignette, Grain (fine noise), Paper texture (soft fibres and a slightly warm cast) and
  Weave (a woven pattern turned 45 degrees, so every cell is a diamond, with neighbouring threads alternating
  over and under).
- **Presets**: Vivid, Noir, Vintage, Film, Cool, Dreamy, Paper and Weave set a whole look in one click, and
  the sliders then fine-tune it. Double-click a slider to zero it, Reset filter clears the slot, and Apply to
  all copies the current look to every slot.
- **A filter belongs to the slot, not the picture.** Swapping two pictures with the corner handle leaves each
  slot's look where it is. In the custom grid designer a merged cell keeps its first filter, and a split cell's
  first half keeps it. In Stack and Polaroid there are no slots, so click a card to choose which one is edited
  (it is outlined), and its filter then goes with that card.
- Filters change the picture only, not the background, the gaps or a polaroid's frame, and empty parts of a slot
  stay empty. They do not cross a diagonal cut.
- Preview and export match exactly. The filters are applied by this page rather than by the browser's canvas
  filter property, which Safari does not support. The preview keeps each slot's filtered result and only redoes
  the slot you are changing, and the export filters at full resolution, which adds a second or two for a 4K
  collage with every effect on. Texture sizes are fixed in export pixels, so a grain or a weave looks the same
  size in the preview as in the file.

#### Diagonal slices

![Diagonal slices](docs/screenshot-diagonal.png)

A wide image and a portrait one, joined by a slanted cut with a thin line along it.

- **Slices** sets how many images there are (2 to 6), separated by parallel cuts, left to right. The defaults
  are 2 slices, a cut at 70%, 3 degrees, a 5 px white line, inside the taskbar-free area.
- **Position** puts the last cut at that percentage of the usable width (the canvas minus the taskbar strip), measured at mid height. With two slices
  that is the one cut, so 66 to 75 gives the usual two-thirds or three-quarters look. With more slices the
  earlier cuts spread evenly before it.
- **Angle** leans the cut, from -60 to 60 degrees. 0 is a straight vertical cut and a positive angle leans
  the top of the line to the right.
- **Line thickness** is measured across the line, shown in pixels at the current export size (5 px at 1920
  wide, and it scales with the export), and 0 removes the line. **Line colour** uses the colour picker, including sampling from your images.
- Each slice keeps its own zoom and position (drag inside it, wheel to zoom), and the corner handle swaps
  two slices. Slices meet with a sub-pixel overlap, so there is no hairline of background between them even
  with the line off.

#### Custom grid designer

![Custom grid designer](docs/screenshot-designer.png)

Choose Custom, then Design layout to open a popup. It edits a draft and only changes the page when you
press Apply layout, so Cancel (or Esc) throws it away.

- The canvas is divided into columns by rows of **units**, and every cell is a rectangle of units. Drag
  across the grid to select. The selection grows to take in every cell it touches, so it is always a clean
  rectangle of whole cells.
- **Merge cells** joins the selection into one cell, and the merged cell keeps the first image. **Split
  left and right** and **Split top and bottom** halve a cell. **Break into units** turns a cell back into
  single units.
- **Subdivide grid** makes the grid twice as fine, which is how a cell gets split further. **Coarsen grid**
  undoes that when every cell edge sits on an even unit. Columns and Rows resize the grid (up to 16 by 12),
  and the cell count is capped at 72.
- Start from a preset (Bento, Hero left, Hero top, Pinwheel) or Reset to single units. Images carry over in
  reading order. Dashed lines inside a cell show the units it is made of.
- It works with the keyboard: arrow keys move a cursor and Shift with the arrows selects a block. Focus
  stays inside the popup, and Esc closes it. On a phone it becomes one column and a finger drag selects.
- Gap, outer margin and border apply to a custom layout the same way as to Panels, inside the taskbar-free area. Each cell keeps its own
  zoom and position, and the corner handle swaps two cells.

### Crop

![Crop tab](docs/screenshot-crop.png)

- One image under a fixed-ratio frame. Drag to position, mouse wheel or slider to zoom (0.1x to 8x). The
  bright area is what gets exported, and the dimmed part outside the frame is what gets cut.
- Ratios: 16:9, 21.5:9, 16:10, 4:3, 1:1 and a custom W:H. 21.5:9 is exactly 43:18, the ratio behind 3440x1440 and
  5160x2160.
- The line under the preview shows the active image's name, format, pixel size and file size, so the export
  can be matched to it. It warns when a JPEG or WebP source is set to export as PNG, which is lossless but
  usually many times larger.
- Background behind the image and between panels: solid, or a two-colour linear, radial or conic gradient.
- Colours use a built-in picker rather than the browser's: a saturation and brightness square, a hue strip
  and a hex field, plus a way to take a colour from your own images. Click or drag on a loaded image to
  sample an exact pixel, or take one of its six most common colours. It is used for the background, panel
  borders, and card borders.

![Colour picker](docs/screenshot-picker.png)

### Repair

![Repair panel](docs/screenshot-repair.png)

Available in Crop and in Collage. The panel's header shows what it is acting on: the crop image, the selected
cell or slice, or the selected card.

- **Fill bars** fills the part of the frame (Crop) or of the selected cell (Collage, in Panels, Grid, Custom
  and Diagonal) that the image does not cover. It makes a new image in the list at the size of that area at
  the export size, puts it in the slot at zoom 1, and leaves the original alone. Cards in Stack and Polaroid
  always fill their photo area, so there is nothing to fill and the button is off.
  - **Content-aware** continues texture and edges from the image. It is multi-scale PatchMatch with patch
    voting, the family of method behind Photoshop's content-aware fill. It suits sky, water, grass, walls
    and bokeh, for bars up to about a third of the frame per side.
  - **Mirror** reflects the image into the bars. **Blur** puts a softened, scaled copy behind it. Both are
    instant fallbacks for content where content-aware smears.
  - Content-aware runs in slices so the page stays responsive, shows progress, and can be cancelled.
- **Heal brush**: turn it on, paint over a blemish, release. The area is rebuilt from the pixels around it.
  Up to 6 undo steps per image. In Collage, paint on any cell, slice or card: the stroke goes to the picture
  under the brush, however it is zoomed, panned or tilted, and that cell or card is selected. The fix is made to
  the picture itself, so it shows everywhere that picture is used.

![Fill bars, before and after](docs/screenshot-fill-before-after.png)

What it cannot do: it copies and recombines what is already in the image, so it cannot invent faces or
objects, and detailed structure can repeat or smear. A faint seam at the edge of a fill, as in the example
above, is normal. Generative expand in current Photoshop uses a cloud model, which a single local file
cannot.

### Export

- Standard sizes per ratio. 16:9: 1280x720 up to 7680x4320. 21.5:9: 2580x1080, 3440x1440, 5160x2160,
  5120x2160. 16:10: 1280x800 up to 3840x2400. 4:3: 1024x768 up to 3200x2400. 1:1: 1024x1024 up to 4096x4096. A custom ratio lists widths
  from 1280 to 5120 with the height worked out, or take a custom width. No side goes over 8192.
- The export size sets the real aspect, so 5120x2160 (2.370) gives a slightly narrower frame than
  3440x1440 (2.389).
- PNG (lossless), JPEG or WebP with a quality slider. Check size encodes without downloading so you can see
  the file size first.
- Set the file name before saving. The field shows the default as a placeholder and a live "Saves as" line
  with the final name. The extension always comes from the chosen format, so typing `sunset.webp` does not
  produce `sunset.webp.webp`. Path characters, control characters and names Windows reserves are replaced
  or prefixed, and an empty name falls back to `wall-<width>x<height>-<date>`. Press Enter in the field to
  export. The name is not remembered between visits, so an old name is never reused by accident.
- A warning appears when a source is being enlarged to fill the export size.

## The side panel

The left column is organised by task, not by feature, so you rarely scroll and never hunt for a section.

- **Three tabs** along the top. **Source** has Images and Settings. **Layout** has Canvas (mode, ratio, export
  size, background) and Collage (layout and its controls, the taskbar offset). **Edit** has Selected, Filter and
  Repair, and opens on an "Editing" strip that names the slot you are working on, with a "filter on" badge. The Edit
  tab shows a dot when that slot has a filter. Arrow keys, Home and End move between tabs, and the last tab you used
  is remembered.
- **A pinned export bar** under the tabs is always on screen, whichever tab you are on: file name, format
  (PNG, JPEG, WebP), a quality slider when the format is lossy, the Export image button, the "Saves as" line
  and Check size. Enter in the name field exports.
- **Sliders sit on one line**: label, slider, value. Click a value, or focus it and press Enter, to type an exact
  number. Enter or clicking away applies it (it is clamped to the slider's range and step), Escape cancels.
- **Filter groups fold.** Light, Colour and Effects each fold on their own, and a dot beside a group's name shows that
  something in it is set, even when it is folded. Presets sit above the groups.
- **Sections fold.** Click a header, or focus the heading and press Enter or Space. The header keeps its title and
  its count when folded. The button beside the tabs reads Collapse all when everything is open and Expand all
  otherwise. Selected, Filter, Repair and Settings start folded, and your choices are remembered.
- **The column scrolls inside itself** between the tabs and the export bar, sized to the window, so the preview
  and the export bar never move. On a narrow screen the tabs stick to the top of the window and the export bar to
  the bottom.

## Settings files

> Previously called wall-fit. Settings files and saved browser data from that name still load, and the design, mode
> and accent are carried over automatically on first open.

Save settings writes every control (mode, ratio, layout, colours, seed, card positions, filters) to one JSON file.
Load settings reads it back, validating each field. `Ctrl+S` saves, `Ctrl+O` loads. Settings are also kept
in this browser between visits. Images are held in memory only, so after a reload the settings come back and
the pictures have to be loaded again.

## Flip, rotate and straighten

The Selected panel (Edit tab) turns the picture in the chosen slot: the crop image, a cell or slice, or a card.
Each slot has its own orientation, and it stays with the picture when two pictures are swapped.

- **Straighten** is a slider from -45 to 45 degrees in 0.1 steps, for a crooked horizon. Positive is clockwise.
  You can click its value and type an exact figure.
- **Flip horizontal** and **Flip vertical** mirror the picture as you see it. **Rotate 90** turns it a quarter turn
  clockwise. They compose the way you would expect on screen: Rotate 90 then Flip horizontal mirrors the turned
  picture, and flipping a picture you have already straightened mirrors the tilt too, so the slider still turns
  clockwise afterwards.
- A turned picture is scaled so it still covers its cell at zoom 1, so straightening never leaves empty corners, and
  panning is kept to where the picture still covers. Zoom out below 1 to see the whole turned picture.
- Fit width and Fit height measure the turned picture's outline. Reset clears zoom, position, flips and turn.
- Filters, the heal brush and Fill bars all follow the turned picture, and Mirror fill reflects it in its own axes.

## Getting images in

Drop files on the Images panel or on the preview, click to browse, or copy an image and paste with `Ctrl+V`.
Dragging an image straight out of a web page often hands the browser a link rather than a file, which the
page cannot fetch because the CSP blocks every network request. Copy and paste is the reliable route.

**Clear all images** (under the thumbnails, once something is loaded) removes every loaded picture after a
prompt. The layout, settings and the filters on slots stay, and the cells and crop are left empty. Each picture
also has its own x to remove just that one.

## Where the data goes

Nowhere. Images are decoded with `createImageBitmap` and drawn to a canvas, never inserted into the page.
Settings are kept in this browser's localStorage until you reset. The only way data leaves the page is a
file you download yourself, and the CSP in the head blocks network access. Settings loaded from a file are
clamped and checked, including colours, card positions and image references, before anything reaches the
canvas.

## Limits

- Needs a current browser: Chrome or Edge 90+, Firefox 98+, Safari 15+ (it uses `createImageBitmap`,
  `ResizeObserver` and `replaceChildren`).
- Big photos (over 2400 px) are also kept as a 2048 px copy that the preview draws from whenever the
  screen cannot show more detail, which keeps dragging and zooming smooth. The export always uses the
  full image. Zoom in far enough and the preview switches to the full image on its own.
- Settings files from before Slices was folded into Grid still load: they become a one-row grid with the same
  number of columns.
- A cell's zoom, position and orientation come back with a settings file, and are applied to whichever picture then
  fills that cell, so load the same images in the same order to get the same result.
- Card positions from dragging, Stack or Polaroid filters, and a card picture's zoom, position and orientation, are tied to the order images were loaded, so they
  come back with a settings file only if you load the same images in the same order. They are dropped on a plain
  page reload. Filters on cells, slices and the crop image belong to their slot and always come back.
- A settings file over 2 MB is refused (they are a few KB).
- Fill bars treats transparent pixels in an image as empty, so a PNG with transparent corners gets them
  filled too.
- Reset, Load settings and removing an image are refused while a fill or heal is running, rather than
  racing it.
- 40 images per session.
- Export width is capped at 8192. Very large exports need a lot of memory and a browser that allows a canvas
  that size.
- Fill and heal edit a canvas copy of the image in memory. An image over 120 megapixels is refused.
- Content-aware fill is solved at up to 768 px on the long side and scaled up behind the sharp image, so
  the filled area is softer than the original.
- JPEG and WebP encoding depend on the browser. If a format is not supported, the browser writes PNG and
  the page says so.
- Conic gradients need `createConicGradient`. Without it the background falls back to solid.

## Appearance

Three independent controls in the top bar, each saved separately, so changing one never resets the others.

**Design** sets the shape language, surface hue and backdrop texture:

| Design | Shape | Backdrop |
|---|---|---|
| `cobalt` | Slight radius, navy | Blue glow from the top, accent rules on each panel. The default |
| `chamfer` | Cut corners | Instrument grid |
| `console` | Square, graphite | Horizontal scan rules |
| `contour` | Soft radii | A single accent wash |

**Mode** sets the lightness ramp only, and every design supports every mode:

| Mode | Base |
|---|---|
| `dark` | Lights off. The default |
| `dusk` | Dark, lifted off black, for long sessions |
| `sepia` | Warm paper, bright but low glare |
| `light` | Cool white, closest to print |

**Accent** is any hue. Eight presets are offered, plus a hue/chroma wheel and a hex field. The accent's
lightness is measured against the surface it sits on, so any hue stays readable in any combination. The
favicon is the wall-craft logo painted in the live accent.

Printing forces one light palette regardless of design and mode and keeps the bar as a masthead.

## Built from

The single-file-tool template: the same shell, appearance system, state
layer, CSP and CI check as the other tools in the set. The workspace fills the screen instead of the
template's capped column (`--wrap: none`).

Screenshots use generated sample images.

## Deployment

`_headers` (Netlify, Cloudflare Pages) and `.htaccess` (Apache) carry the same CSP and hardening headers.
Neither is needed to run the file locally, including from `file://`. The policy is the template's
unchanged: `default-src 'none'`, no network, no eval, no remote fonts or scripts.

## Licence

MIT. See `LICENSE`.
