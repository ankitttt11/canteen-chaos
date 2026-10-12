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


### CC-04 — "The buttons don't work on my tablet"

**Reproduced:** I opened the site in Chrome's iPad Mini view. Clicking
`Add to Cart` and the favourite star did not produce any visible result.

**Cause:** At tablet widths, invisible `::after` layers were placed over the
dish cards. These layers had a higher display layer (`z-index`) than the
buttons and received the clicks instead. The buttons' JavaScript was not the
problem; the clicks never reached it.

**Fix:** Added `pointer-events: none` to the tablet overlay rule in
`frontend/style.css`. This makes the invisible layers ignore clicks so the
buttons underneath can receive them.

**Checked:** Refreshed the iPad Mini view and clicked `Add to Cart`. The cart
count increased. I also clicked a favourite star and saw the saved message.

**Time:** Not recorded.


### CC-05 — "The category bar scrolls away on my phone"

**Reproduced:** I opened the site in iPhone SE view and scrolled down through
the menu. The dish cards moved up, but the category and filter bar also
scrolled away instead of staying available at the top.

**Cause:** On small screens, `.view` used `overflow-x: hidden`. This made the
menu view act like a scrolling container. The sticky filter bar then used
that container instead of the browser window, so it did not remain visible
while the page was scrolled.

**Fix:** Changed the mobile `.view` rule from `overflow-x: hidden` to
`overflow-x: clip` in `frontend/style.css`. This still clips horizontal
overflow without interfering with the sticky filter bar.

**Checked:** Refreshed the iPhone SE view and scrolled through the menu. The
filter buttons and category tabs stayed visible while the dishes moved below
them.

**Time:** Not recorded.


### CC-06 — "I ordered more than they had"

**Reproduced:** The menu showed only 3 `Dal Tadka Thali` left, but the cart
allowed a quantity of 5. I continued to checkout and tried to place the
order.

**Cause:** The backend checked only whether stock was zero. It did not check
whether the requested quantity was greater than the available stock. This
allowed an order for 5 items when only 3 were available.

**Fix:** Added a backend validation check in `backend/logic/validation.js`.
The server now rejects the order when the requested quantity is greater than
the dish stock.

**Checked:** Retried checkout with 5 items while only 3 were available. The
order was rejected with `Invalid order: Dal Tadka Thali: only 3 left`.

**Additional improvement:** Added a frontend stock limit in
`frontend/js/state.js`. The cart now also stops increasing when its quantity
reaches the available stock. The backend validation remains in place because
frontend checks can be bypassed. I tested it with 3 available items: the
quantity stayed at 3 and the app showed `Only 3 left` when I tried to add a
fourth.

**Time:** Not recorded.


### CC-07 — "Cancelling makes it worse"

**Reproduced:** Rajma Chawal had 23 items in stock. I placed an order for
one, so the stock became 22. I then cancelled that order. Instead of going
back to 23, the stock went down again.

**Cause:** The cancellation function was subtracting the cancelled quantity
from stock. Cancellation should return the reserved item to the stock.

**Fix:** Changed the cancellation calculation in
`backend/logic/validation.js` from subtracting the quantity to adding it.

**Checked:** Placed one Rajma Chawal order and confirmed stock changed from
23 to 22. Cancelled the order and confirmed stock returned to 23.

**Time:** Not recorded.


### CC-08 — "An old coupon still works"

**Reproduced:** I added a dish to the cart, opened checkout, and applied the
coupon `FRESHERS24`. The coupon was expired, but the checkout accepted it and
gave a discount.

**Cause:** The coupon list endpoint checked the expiry date, but the pricing
function that actually calculated the discount did not check `expiresAt`.
Therefore, a user could still send the old coupon code directly.

**Fix:** Added an expiry-date check to `backend/logic/pricing.js`. A coupon
is now rejected when its expiry date is today or earlier.

**Checked:** Applied `FRESHERS24` again after restarting the server. The app
showed `This coupon has expired` and gave no coupon discount.

**Time:** Not recorded.


### Supporting fix — "My Orders showed no orders"

**Reproduced:** After placing an order, the browser saved the order token,
but the My Orders page showed `No orders found`. The Counter page could still
see the order.

**Cause:** The frontend sent a `memberId` query parameter. The backend
received it but compared orders against `req.query.userId`, which was empty.
Therefore, no order matched the current member.

**Fix:** Changed the backend filter to compare the order’s member ID with
`req.query.memberId` in `backend/routes/orders.routes.js`.

**Checked:** After restarting the server and placing a fresh order, the
order appeared in My Orders.

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
