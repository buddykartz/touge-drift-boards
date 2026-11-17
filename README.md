# touge-drift-boards

Roadside hoarding artwork and messages for the Apex Crews game.

The game reads `boards.json` from this repository's site
(`https://buddykartz.github.io/touge-drift-boards/boards.json`) and shows the pictures and
messages it lists on the hoardings beside the road. Changing this repository changes what the
game shows, with no app update.

## Files

| File | What it is |
| --- | --- |
| `boards.json` | The manifest: which pictures and messages go on which pass. |
| `images/` | The pictures the manifest names. |

## The manifest

`boards.json` is one JSON object with three optional lists of entries: one for every pass, one per
world, and one per pass.

```json
{
  "version": 1,
  "enabled": true,
  "revision": 1,

  "default": [
    { "image": "images/touge-drift.png" },
    { "message": ["KEEP IT", "SIDEWAYS"] }
  ],

  "worlds": {
    "port": [
      { "image": "images/night-owls.jpg" },
      { "message": "HARBOUR LIGHTS\nOPEN ALL NIGHT", "background": "#10182E", "color": "#4FA3F7" }
    ]
  },

  "passes": {
    "ember-descent": [
      { "image": "images/ember-descent.png", "start": "2026-01-01", "end": "2026-12-31" }
    ]
  }
}
```

### Top level

| Field | Required | Meaning |
| --- | --- | --- |
| `version` | yes | Must be `1`, or the whole file is ignored. |
| `enabled` | no | `false` switches every remote hoarding off. Defaults to `true`. |
| `revision` | no | Not read by the game. Add one to it when you want the game to look at the pictures again. |
| `default` | no | Entries for every pass that has nothing more specific. |
| `worlds` | no | Entries by world name. |
| `passes` | no | Entries by pass name. |

### Which list a pass shows

The most specific list wins: the pass's own list, or else its world's list, or else `default`.
The lists are not mixed.

The entries go up in order: the first entry on the first hoarding along the road, the second on
the next, and round again if there are more hoardings than entries. If a list has more entries
than the pass has hoardings, the ones at the end are not shown there.

A list with nothing in it, such as `"frost-pass": []`, means "built-in artwork on this pass".

### An entry

Each entry is either a picture or a message, never both.

| Field | Used with | Meaning |
| --- | --- | --- |
| `image` | picture | Path from this folder, ending `.png`, `.jpg` or `.jpeg`. No `..` and no web addresses. |
| `message` | message | One string with `\n` line breaks, or a list of strings. Up to 3 lines of 24 characters, shown in capitals. Latin letters, digits and plain punctuation only. |
| `background` | message | Background colour as `"#RRGGBB"`. Defaults to near-black. |
| `color` | message | Text colour as `"#RRGGBB"`. Defaults to off-white. |
| `start`, `end` | both | `"YYYY-MM-DD"`: the first and last day the entry shows. Either can be left out. |

### Names to use

Spell them exactly as shown, in small letters. A list for a name the game does not know is ignored.
"Stages" are the career stage numbers raced on that pass (the career has 28 stages in four crews).

| Pass (name in the game) | Name in `passes` | Its world, for `worlds` | Stages |
| --- | --- | --- | --- |
| Pine Hollow | `pine-hollow` | `woodland` | 1 |
| Cedar Steps | `cedar-steps` | `farmland` | 2, 5 |
| High Moor | `high-moor` | `moor` | 3 |
| Ochre Ridge | `maple-ridge` | `desert` | 4 |
| Frost Hollow | `frost-pass` | `alpine` | 6, 11 |
| Ember Descent | `ember-descent` | `quarry` | 7, 8 |
| Glowworm Bends | `sakura-bends` | `port` | 9 |
| Coral Coast | `coral-coast` | `coast` | 10 |
| Aurora Crest | `moonlit-crest` | `summit` | 12, 13, 14 |
| Wren Gully | `wren-gully` | `woodland` | 15 |
| Granite Stair | `granite-stair` | `quarry` | 16, 26 |
| Kelp Bay | `kelp-bay` | `coast` | 17 |
| Saltpan Dash | `saltpan-dash` | `desert` | 18 |
| Larch Zigzag | `larch-zigzag` | `alpine` | 19, 24 |
| Long Barrow | `long-barrow` | `farmland` | 20 |
| Starfall Ridge | `starfall-ridge` | `summit` | 21, 27 |
| Lantern Quay | `lantern-quay` | `port` | 22 |
| Heather Sweep | `heather-sweep` | `moor` | 23 |
| Glacier Road | `glacier-road` | `alpine` | 25, 28 |

Some worlds now hold more than one pass, so a `worlds` list reaches every pass in that world, and a
`passes` list reaches just one. Every pass has several hoardings (three to five kinds of prop in each
world's rotation are hoardings), and Solo Drift and Endless use the same passes.

### The crews

| Crew | Stages | Colour |
| --- | --- | --- |
| Night Owls | 1 to 7 | `#4FA3F7` |
| Red Lanterns | 8 to 14 | `#E8552E` |
| Jade Foxes | 15 to 21 | `#2FBF8F` |
| Storm Crows | 22 to 28 | `#9B6BFF` |

## Pictures

- **Size: 1480 x 600 pixels.** That is the shape of a hoarding's face (about 2.47 to 1). The game
  scales it down to 768 x 311, so there is nothing to gain from anything larger.
- A picture of a different shape is never stretched. It is scaled to cover the face, and whatever
  hangs over is cut off evenly, top and bottom or left and right.
- PNG or JPEG. Keep each under about 300 KB: players download them over their own connection.
- Anything see-through in a PNG shows white.
- Use a few large words, strong contrast and bold shapes: a hoarding is on screen for a second or
  two, at an angle. Small print will not be read.

## Limits

Anything over a limit is left out, and the rest still shows.

| | Limit | If you go over |
| --- | --- | --- |
| The manifest | 64 KB | The whole file is ignored |
| Entries in one list | 8 | Only the first 8 are used |
| Entries in all lists together | 48 | The whole file is ignored |
| Different pictures and messages | 24 | The whole file is ignored |
| One picture's file | 1.5 MB | That entry is skipped |
| One picture's size | 4096 pixels a side, 6 million pixels in all, at least 64 wide | That entry is skipped |
| All downloads in one check | 12 MB | The check is abandoned; the hoardings stay as they were |
| A message | 3 lines of 24 characters | That entry is skipped |

An entry with a mistake in it is left out on its own. A file that is wrong as a whole (not valid
JSON, the wrong `version`, or over a whole-file limit) is ignored, and the hoardings keep what
they showed before.

## How long a change takes

1. GitHub publishes a change within a minute or so, and may keep serving the old version for up
   to ten minutes after that.
2. The game checks when it is opened, a couple of seconds after the menu appears, and again when
   it comes back to the front after more than an hour. It never checks during a race.
3. New art goes up the next time a pass is loaded, so usually on the first race after the check.

### Replacing a picture

The game only looks at the pictures again when the manifest itself has changed. If you upload a
new picture under the same name as an old one, also add one to `"revision"` in `boards.json`. Or
give the new picture a new name and change the entry to match.

### If nothing changes

- Open the manifest's address in a browser. Is it your latest version?
- Is the file still valid JSON? One missing comma or quotation mark makes the game ignore all of it.
- Does each picture's name in the manifest match the file exactly, capitals included?
- Is the entry in date and inside the limits?

## What may be shown

- Pictures and words only. The game never downloads or runs code.
- Everything must suit the game's age rating, whenever it was uploaded.
- Everything must be ours to use: our own artwork, or artwork we have written permission for.
- No tracking: nothing here may count views or collect anything about players.
- Hoardings are decoration. They are not tappable and lead nowhere.
