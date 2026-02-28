# Conway's Game of Life in C

![Language: C](https://img.shields.io/badge/Language-C-blue.svg)
![Build: CMake](https://img.shields.io/badge/Build-CMake-brightgreen.svg)
![Platform: Windows Console](https://img.shields.io/badge/Platform-Windows%20Console-0078D6.svg)

A console-based implementation of **Conway’s Game of Life** written in C.
The project simulates a 2D cellular automaton and renders it in the terminal using the Win32 console API for significantly faster output than plain `printf`.

## Why this project

This repository demonstrates:
- clean separation between simulation logic and rendering,
- manual memory management in C,
- argument parsing and interactive CLI input,
- file-based import/export of simulation states.

## Demo (GIF / Screenshot)

### Console preview
> Add your best short recording or screenshot here for profile visitors.
>
> Suggested file path: `assets/gameplay.gif` (or `assets/screenshot.png`)

```md
![Gameplay demo](assets/gameplay.gif)
```

If you'd like, I can also help you generate a polished demo GIF from one of the patterns in `examples/`.

## Game rules

Each cell has two possible states: **alive** or **dead**.
At every iteration, the next state is calculated from the 8 neighboring cells:

- **More than 3 live neighbors** → the cell dies (overpopulation).
- **Exactly 3 live neighbors** → the cell becomes alive (or stays alive).
- **Exactly 2 live neighbors** → the cell keeps its current state.
- **Fewer than 2 live neighbors** → the cell dies (underpopulation).

## Build

### Prerequisites
- CMake 3.23+
- A C compiler with C99 support
- Windows environment (the rendering path uses Win32 console functions)

### Build steps
```bash
cmake -S . -B build
cmake --build build
```

By default, CMake generates an executable named `Nagy_hazi`.

## Run

```bash
./Nagy_hazi <width> <height> <input_state_file> <output_file> <iteration_count>
```

### Arguments
- `width` *(required)*: board width.
- `height` *(required)*: board height.
- `input_state_file` *(optional)*: file to load the initial board from.
  - If omitted, a random board is generated.
- `output_file` *(optional)*: file path to save the final board state.
- `iteration_count` *(optional)*:
  - `0` (default) → real-time graphical mode until `Esc` is pressed.
  - `> 0` → headless simulation for a fixed number of steps, then optional save.

If `width` and `height` are not provided, the program prompts for all parameters interactively.

## Example patterns

Sample pattern files are available in `examples/`, including:
- oscillators,
- spaceships,
- glider gun,
- and other large known configurations.

## Project structure

- `main.c` – program entry point, settings, argument handling, run loop.
- `conwayGame.c/.h` – core game state and simulation step logic.
- `gameEngine.c/.h` – console rendering and screen handling.
- `econio.c/.h` – console/keyboard utilities.
- `examples/` – sample starting configurations.

## Core data structures

### `GameState`
Stores the current board:
- `width`, `height`
- `bool* cells` in a **1D contiguous array** of size `width * height`

### `Vector`
Represents coordinates (`x`, `y`) for helper operations.

### `Screen`
Represents the rendering buffer and Win32 console handles.

### `Settings` and `Params`
Used in `main.c` for application defaults and runtime parameters.

## Key functions (high-level)

- `createNewState`, `destroyGameState` – allocate/free board state.
- `clearCells`, `randomizeCells` – initialize board content.
- `countAliveNeighbours` – count live neighbors around a cell.
- `calculateNextState`, `stepGame` – compute simulation progression.
- `loadStateFromFile`, `saveStateToFile` – persistence.
- `convertToChar`, `render2d` – visual output in console.

## What I learned

Building this project strengthened practical C and systems-programming skills:

- Designing compact data layouts (`width * height` flat array) for performance and simpler memory allocation.
- Managing dynamic memory lifecycle (`create`/`destroy`) carefully to avoid leaks during iterative simulation.
- Separating concerns between simulation logic and rendering code to keep modules maintainable.
- Balancing readability and performance by replacing slow console printing with Win32 buffer rendering.
- Building CLI UX that supports both interactive mode and scripted/headless runs.

## Notes

- Random initialization depends on `srand(time(NULL))` in `main.c`.
- In graphical mode, frame timing is controlled by `fps` in `Settings`.
- The renderer hides the cursor during runtime and restores it on exit.
