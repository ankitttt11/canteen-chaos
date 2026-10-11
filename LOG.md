# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.

Delete the example before you submit.



### CC-01 — "The search suggestions are behind everything"

**Reproduced:** Typed `sa` into the menu search box. The suggestion list
appeared, but the category bar covered the top of the list. The visible
suggestions could be opened, but the dropdown was displayed behind the
category bar.

**Cause:** The sticky `.filters` and `.cat-tabs` elements use `z-index: 40`.
The suggestions are inside `.search-wrap`, which created a lower stacking
context with `z-index: 1`. The `.suggest-box` had `z-index: 100`, but that
value could not place it above the higher-level sibling stacking context.

**Fix:** Changed `.search-wrap` from `z-index: 1` to `z-index: 50` in
`frontend/style.css`. This places the search wrapper, including its
suggestion dropdown, above the category bar.

**Checked:** Refreshed the application, searched for `sa`, and confirmed
that the complete suggestion list appeared above the category bar. Opened
both the top suggestion (`Samosa`) and the bottom suggestion (`Masala Dosa`)
successfully.

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
