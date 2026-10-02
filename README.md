# BSQ — Biggest Square

Finds the largest square of empty cells in a map of obstacles, in a single pass using dynamic programming, written in C.

## About

Built in 2021. Given a text map made of empty cells (`.`) and obstacles (`o`), the program finds the biggest possible square that contains no obstacle and prints the map with that square filled with `x`. When several squares have the same size, the one closest to the top-left is chosen.

The project was written under strict constraints: C only, no standard string helpers (a custom `libmy` provides them), and a strict coding style.

## Features

- Reads the whole map in one `read()` call based on the file size (`stat`).
- Input validation: argument count, readable file, line count matching the header, equal line lengths, allowed characters only (exit code `84` on error).
- **Dynamic programming** solution in `O(rows × cols)`: each cell stores the size of the largest square whose bottom-right corner is that cell (`1 + min(left, top, top-left)`).
- Writes the result directly to standard output with `write()`.

## Tech stack

- C (C99), `gcc`
- GNU Make
- `libmy`: a small hand-written static library of string / number utilities

## Architecture

```
src/
├── main.c         Open / stat / read the map file, run the pipeline
├── error.c        Argument and map-format validation
├── manage_tab.c   Convert the text map into an int matrix
├── algo.c         Dynamic-programming pass (min of the 3 neighbours + 1)
├── replace.c      Locate the maximum and mark the square
└── display.c      Print the map with the square drawn as 'x'
lib/my/            libmy.a — custom string / number helpers
include/my.h
```

## Build & Run

Requirements: a C compiler and `make`.

```bash
make            # builds lib/my then ./bsq
make fclean     # remove the binary
```

```bash
./bsq <map_file>
```

Example map (`map.txt`) — the first line is the number of rows:

```
9
...........................
....o......................
............o..............
...........................
....o......................
...............o...........
...........................
......o..............o.....
..o.......o................
```

Output:

```
.....xxxxxxx...............
....oxxxxxxx...............
.....xxxxxxxo..............
.....xxxxxxx...............
....oxxxxxxx...............
.....xxxxxxx...o...........
.....xxxxxxx...............
......o..............o.....
..o.......o................
```
