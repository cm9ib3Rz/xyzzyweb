# xyzzy: classic text adventures in the browser

**A Z-machine interpreter written in TypeScript that runs entirely in the browser, so a class can play the Infocom classics (Zork I–III, Deadline and Trinity) with no install.**

### [▶ Open xyzzy](https://cm9ib3Rz.github.io/xyzzyweb/)

No install, no sign-up, no adverts. Everything runs in your browser, and saved games stay on your own device.

---

## What is the Z-machine?

In 1979 Infocom wanted its games to run on dozens of incompatible home computers. Rather than rewrite each game for each machine, they invented an imaginary computer, the **Z-machine**, and compiled every game to its machine code (*Z-code*). To bring a whole catalogue to a new computer, they only had to write one small program for it: an **interpreter** that pretends to be a Z-machine.

This project is one of those interpreters. It is a good example of ideas from A Level Computer Science working in a real, much-loved piece of software:

- **Virtual machines and interpreters.** The story file is a program for a machine that doesn't exist; `src/vm/` is the software that makes it exist.
- **Fetch–decode–execute.** `src/vm/Processor.ts` fetches each Z-code instruction, decodes its operands and runs it, the same cycle a real CPU follows.
- **Stacks, memory and data structures.** The call stack, the object tree, the dictionary and the packed text encoding (three characters in every two bytes) are all there to read.
- **Parsing.** Typing `take the lamp` becomes dictionary lookups and a grammar match inside the game itself.

## Using it

1. **[Open xyzzy](https://cm9ib3Rz.github.io/xyzzyweb/)** and enter the **access code** your teacher gives you. Different codes unlock different games.
2. Pick a game. Type commands at the prompt and press **Enter**: `look`, `north`, `take lamp`, `inventory`.
3. The sidebar has everything else:

| Control | What it does |
|---|---|
| **Quicksave / Quickload** | One-click save and restore of your current position |
| **Saves…** | Named save slots, so you can keep several positions. The game's own `save` and `restore` commands use the same slots. |
| **View / Download Transcript** | Everything the game has printed so far, to read back or keep as a text file |
| **Quit to Menu** | Leave the game and choose another |

Saves are kept in the browser (IndexedDB), so they stay on that computer and that browser. Use the same machine next time, or download a transcript if you want a record elsewhere.

## A note on the games

The interpreter is this project's own work and is released under the MIT licence. **The games are not.** Zork, Deadline and Trinity are copyrighted Infocom titles and are **not** covered by this repository's licence. The story files in `public/data/` are lightly obfuscated so that the site does not simply hand out copies of them, and the access code keeps the library behind a classroom gate. Neither is real protection, and neither is meant to be.

If you run your own copy, you are responsible for the story files you put in it. Plenty of freely distributable Z-code games exist (for example on the [IF Archive](https://www.ifarchive.org/)), and the interpreter is built for version 3 story files like these.

---

## Run it yourself

You need [Node.js](https://nodejs.org) 24 or newer.

```bash
git clone https://github.com/cm9ib3Rz/xyzzyweb.git
cd xyzzyweb
npm install
npm run dev        # open the address it prints
```

| Command | What it does |
|---|---|
| `npm run dev` | Start a local development server |
| `npm test` | Run the test suite |
| `npm run build` | Build the site into `dist/` |
| `npm run preview` | Serve the built site locally |

The built site is plain static files (`dist/`), so it can be hosted anywhere. This repository publishes it to **GitHub Pages** with the workflow in [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml): on every push it runs the tests, builds, and deploys. To do the same in your own copy, set **Settings → Pages → Source** to **GitHub Actions**.

### Adding or changing games

The game list is in [`src/ui/games.ts`](src/ui/games.ts) and the access codes in [`src/ui/gameAccess.ts`](src/ui/gameAccess.ts). To obfuscate story files for deployment, put the originals in a `data-source/` folder (it is gitignored) and run:

```bash
node scripts/obfuscate-data.mjs
```

This writes the obfuscated copies into `public/data/`. A plain, unobfuscated story file also works: add it to `games.ts` with `obfuscated: false`.

### How the project is laid out

| Path | What is in it |
|---|---|
| [`src/vm/`](src/vm) | The Z-machine itself: memory, header, instructions, processor, stack, objects, dictionary, text encoding and Quetzal save files. No web code, so it can be tested on its own. |
| [`src/screen/`](src/screen) | The screen: a DOM version for the browser and a headless version for the tests |
| [`src/ui/`](src/ui) | The page: game menu, access gate, save panel, transcript and controls |
| [`src/storage/`](src/storage) | Saved games in IndexedDB |
| [`scripts/`](scripts) | The story-file obfuscation script |
| [`test/`](test) | Unit tests and scripted playthroughs |

### How it is checked

The test suite covers the text encoding, the header, the object table, the parser and Quetzal save files, and plays through long scripted sessions of the real games to check that the interpreter reaches the right rooms and prints the right text. The tests run on every push to GitHub.

---

## Notes and acknowledgements

- The Z-machine was designed by **Joel Berez** and **Marc Blank** at Infocom (1979).
- This interpreter follows **The Z-Machine Standards Document** by Graham Nelson, which is how the Z-machine is documented today.
- Zork, Deadline and Trinity are the work of Infocom's implementors. They are still some of the best writing in games. Enjoy them.

## Licence

[MIT](LICENSE) © 2026 cm9ib3Rz, for the interpreter source code only. The game story files are not covered; see [A note on the games](#a-note-on-the-games).
