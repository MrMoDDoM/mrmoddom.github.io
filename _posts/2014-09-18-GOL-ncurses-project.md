---
title: Game of Life ncurses
excert: An implementation of the famous Game of Life with ncurses
tags:
 - GOL
 - Game of Life
 - ncurses
 - terminal
 - linux
 - C
category:
 - project
toc: false
---

> All the file and code for this project are available on the [Github Repo](https://github.com/MrMoDDoM/Game-Of-Life-Ncurses)
{: .prompt-info}

Small implementation of the famous Conway's Game of Life cellular automaton, using ```ncurses``` terminal graphics libraries.

Some commands are available during execution:
```
  -[SPACE] -> generate a new random starting grid
  -'i'      -> show a small window with information about the current session.
  -'u'      -> hide the info window
  -'q'      -> terminate and exit the program
  -'c'      -> change the color of the cells
  -'b'      -> change the background color
  -'p'      -> enable step-by-step mode: to continue, press 'p'
  -'o'      -> enable continuous mode
  -'e'      -> increase the delay between iterations
  -'w'      -> decrease the time between iterations
```

It is possible to modify the size of the grid or the character of live/dead cells from the 'define' at the beginning of the program.

# Links
- [GitHub Repo](https://github.com/MrMoDDoM/Game-Of-Life-Ncurses)