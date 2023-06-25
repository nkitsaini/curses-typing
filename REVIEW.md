- Debugging:
	- Set up logging that will write to a file. This will simplify debugging. Search python logging.

- Linting:
	- Set up a simple linting and format on save to keep the code consistent.

- Type Hinting:
	- If we specify type hints in all the functions that vscode intellisense will be much better:
		`def get_xxxxx(stdscr):` we can do `def get_xxxxx(stdscr: curses.window)`, and now vscode can tell
		us about all the functions.
	- similary for parameters other then `stdscr` 

- Sound:
	- use `pygame` library for sound performance. search for `pygame.mixer`.
	


`main:32` => Another approch is this, which does not require us to handle DOWN/UP seperately
```py
if key==curses.KEY_UP:
    current_row_idx-=1
		current_row_idx = current_row_idx % len(menu)
elif key==curses.KEY_DOWN:
	  current_row_idx+=1
		current_row_idx = current_row_idx % len(menu)
```
Or 
```py
if key==curses.KEY_UP:
    current_row_idx-=1
elif key==curses.KEY_DOWN:
	  current_row_idx+=1
...

current_row_idx = curr_row_idx % len(menu)
```


`main:16` => instead of turnign attr on and off you can do
```py
stdscr.addstr(y, x, row, curses.color_pair(1))
```


`main:28` => two calls to `show_menu_scr` can be merged in one 
```py
# Current Version

show_menu_scr(....)
refresh()
while True:
	...
	show_menu_scr(....)
	refresh()
	

# Proposed version
while True:
	show_menu_scr(....)
	refresh()
	...
	
```


`main:45` => I don't think endwin is required if we're using `curses.wrapper`


`cur_type:86` => The colors can be initialized once at the start and we can refer them by better names instead of numbers.
like this:
```py
COLORS = {}

def init_colors():
	curses.init_pair(1, curses.COLOR_BLACK, cursese.COLOR_GREEN)
	curses.init_pair(2, curses.COLOR_BLACK, cursese.COLOR_RED)
	...

	COLORS["green"] = curses.color_pair(1)
	COLORS["red"] = curses.color_pair(2)
	...
```
Or 
```py
# Similar to your commented code

GREEN = None
RED = None

def init_colors():
  global GREEN
  global RED

	curses.init_pair(1, curses.COLOR_BLACK, cursese.COLOR_GREEN)
	curses.init_pair(2, curses.COLOR_BLACK, cursese.COLOR_RED)
	...

	GREEN = curses.color_pair(1)
	RED = curses.color_pair(2)
	...
```
Now in rest of the code we can use `COLORS['green']` or `COLORS['red']` to refer to these colors.

Instead of dictionary using a class might give better type hinting.



`cur_type:149` => `stdscr.refresh()` can be called once in the loop, instead of calling in each if branch.

```py
# current
while True:
	if ...:
		...
		refresh()
	elif ...:
		...
		refresh()
	else ...:
		...
		refresh()

# Proposed solution
while True:
	if ...:
		...
	elif ...:
		...
	else ...:
		...
	refresh()
```


