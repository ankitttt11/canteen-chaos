# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.

Delete the example before you submit.



### CC-01 — "The search suggestions are behind everything"

**Reproduced:** I typed `sa` in the search box. The suggestions appeared,
but the category bar covered the top of the list. This made the dropdown look
partly hidden. The visible suggestions could still be opened.

**Cause:** The category bar was given a higher CSS display layer
(`z-index: 40`) than the search area (`z-index: 1`). The suggestion box had
its own high layer (`z-index: 100`), but it was still inside the lower-layer
search area. Because of this, the category bar appeared in front of it.

**Fix:** I changed the search area’s `z-index` from `1` to `50` in
`frontend/style.css`. This puts the complete search area above the category
bar, so its suggestions can be seen properly.

**Checked:** I refreshed the application and searched for `sa` again. The
complete suggestion list appeared above the category bar. I clicked the top
suggestion, `Samosa`, and the bottom suggestion, `Masala Dosa`; both opened
the correct dish details.

**Time:** Not recorded.

**Time:** Not recorded.


### CC-02 — "Can't read anything in dark mode"

**Reproduced:** I checked the `Grilled Sandwich` card in light mode and
could read its name and price. After switching to dark mode, the name and
price became almost invisible. Other text, such as the grey details line,
was still visible.

**Cause:** The dish body had a fixed dark-brown text color. That fixed color
worked in light mode, but it stayed dark when the page changed to dark mode.
It ignored the theme’s normal text color.

**Fix:** Changed the dish body color from `#2b2118` to `var(--ink)` in
`frontend/style.css`. `var(--ink)` automatically uses the correct text color
for the selected theme.

**Checked:** Refreshed the application and checked `Grilled Sandwich` and
`Rs. 34` in both themes. The dish name and price were readable in light mode
and dark mode.

**Time:** Not recorded.


### CC-03 — "The menu is wider than my phone"

**Reproduced:** I used Chrome’s mobile view at `320px` wide and searched for
`Paneer`. The long dish name was cut off and the card extended beyond the
phone preview. The card and its `Add to Cart` button did not fit correctly.

**Cause:** On small screens, `.name-btn` used `width: max-content`. This
means the button tried to stay as wide as the complete dish name. A long
dish name could therefore make the menu card wider than the phone.

**Fix:** Changed the mobile `.name-btn` rule to `width: 100%; min-width: 0`
in `frontend/style.css`. This makes the name fit inside the card and wrap to
the next line when needed.

**Checked:** Tested the same long dish at `320px` and `375px`. The name
wrapped correctly, the card stayed inside the phone width, and the complete
`Add to Cart` button was visible.

**Time:** Not recorded.


## CC-0X — "<the complaint, in short>"

**Reproduced:**

**Cause:**

**Fix:**

**Checked:**

**Time:**



## Could not fix

For anything you investigated but did not solve. Say what you tried and
where you got to. This is worth marks — leaving it blank when you got
stuck is not.

### CC-0X — "<the complaint>"

**What I tried:**

**Where I got to:**

**What I would try next:**



## Extra credit

Anything not on the bug log: a problem you found yourself, a test you
wrote, or a fix you are unsure about. Same format, plus one line on how
you noticed it.
