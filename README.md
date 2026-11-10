# Civitas

A Monopoly-style board game written in Java with the **Model-View-Controller** pattern. We built it for the Object-Oriented Programming and Design course (PDOO, 2021-22) at the University of Granada and later rebuilt its interface: it now has an animated isometric board made with Swing and [FlatLaf](https://www.formdev.com/flatlaf/).

> The game's interface is in Spanish.

![Game in progress with the property management panel open](docs/partida.png)

## How to play

- **2 to 4 players**. Each one starts with **€7,500** and collects **€1,000** every time they pass Go (*Salida*).
- On your turn you roll the die and move. If you land on a **free street** you can buy it; if it already has an owner, you pay them rent.
- You can build up to **4 houses** on your streets. With 4 houses you can swap them for a **hotel**. The rent is `base × (1 + houses + 4 × hotels)`.
- You can turn a street into a **social street** (*calle social*), which charges 2% less rent.
- **Surprise squares** draw a card: collect or pay a fixed amount, pay or collect for each building, or become a **Speculator** (*Especulador*: you pay half and can build twice as much).
- The game ends when a player goes **bankrupt**. Whoever has the most money wins.

## The interface

- **Isometric board** drawn with `Graphics2D`:
  - Each square is a slab with thickness, and its sides are painted in the owner's colour.
  - Houses and hotels are drawn on top of the streets.
  - The die is a cube that bounces when rolled, and the tokens hop from square to square.
- **Square card:** hovering over a square highlights it and shows its owner, prices, current rent and buildings in the side panel.
- **Side panel:**
  - the players, with their balance;
  - the turn actions, which are only enabled when they are allowed;
  - the surprise card that has just been drawn, with no dialogs blocking the game;
  - property management, with the cost of each building;
  - the game log.
- **Light and dark theme:** menu *Ver → Tema oscuro* or <kbd>Cmd/Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>D</kbd>.
- **Shortcuts:** <kbd>Enter</kbd> rolls the die and <kbd>Cmd/Ctrl</kbd> + <kbd>N</kbd> starts a new game.
- **Debug mode** when creating a game: the die always rolls 1, handy for testing.

<p>
  <img src="docs/tema-oscuro.png" alt="Dark theme with the card of a square under the mouse" width="70%">
  <img src="docs/nueva-partida.png" alt="New game dialog" width="26%">
</p>

## Running it

You need **JDK 21** or later. On macOS, for example:

```bash
brew install openjdk@21
```

Then, from the project root:

```bash
./run.sh
```

The script compiles the code into `build/classes` and launches the game. It uses `JAVA_HOME` if it is set and, otherwise, Homebrew's JDK 21.

It can also be opened as a **NetBeans** (Ant) project: FlatLaf is already on the classpath and the main class is `GUI.CivitasApp`.

## Structure

```
src/
├── civitas/             Model: board, squares, players, surprises,
│                        die, log and the turn state machine
├── controladorCivitas/  Event-driven controller: receives the actions from
│                        the view, runs them on the model and refreshes the view
└── GUI/                 View in Swing + FlatLaf
    ├── CivitasApp           entry point
    ├── CivitasView          main window (implements Vista)
    ├── TableroIsometrico    board, animations and mouse
    ├── Panel*               players, actions, square card and log
    ├── Dialogo*             new game and end of game
    └── Tema                 light and dark colours
lib/
└── flatlaf-3.7.2.jar    look and feel (Apache 2.0 licence)
```

The model does not depend on the view. `CivitasJuego` exposes which operations are allowed at each moment (`puedeAvanzar()`, `puedeComprar()`, `puedeConstruirHotel(ip)`…), and the view uses that information to enable or disable its buttons. `GestorEstados` decides how the turn state changes after each operation.

## Authors

Denis Valentin Stoyanov D'Antonio and Raúl Cruz Arévalo.

## Licence

Civitas' code is distributed under the [MIT](LICENSE) licence. FlatLaf (`lib/flatlaf-3.7.2.jar`) keeps its own licence, Apache 2.0.
