# Game of Life

Het 2-dimensionale speelveld van het Game of Life bestaat uit cellen die levend (zwart) of dood (wit) kunnen zijn.
Of een cel overleeft naar de volgende generatie, hangt af van die cel en van de directe buren.
De spelregels voor het bepalen van de volgende generatie uit de huidige zijn als volgt:

* Any live cell with fewer than two live neighbours dies, as if by underpopulation.
* Any live cell with two or three live neighbours lives on to the next generation.
* Any live cell with more than three live neighbours dies, as if by overpopulation.
* Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

These rules, which compare the behavior of the automaton to real life, can be condensed into the following:

* Any live cell with two or three live neighbours survives.
* Any dead cell with three live neighbours becomes a live cell.
* All other live cells die in the next generation. Similarly, all other dead cells stay dead.
