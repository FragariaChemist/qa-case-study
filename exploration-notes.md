# Exploration notes: Sauce Demo

> **Where we left off (2026-11-25):** Test plan done (`test-plan.md`). Repo is public at https://github.com/FragariaChemist/qa-case-study, and Session 1's bugs and questions are logged as Issues #1-#6. Next: recheck the 3 bugs in Firefox and add screenshots to their Issues, then Session 2 (the other test users).

App: https://www.saucedemo.com (practice store built for testing)
Session 1, 2026-11-23. Kristina's first-look notes, kept close to her own words.

## Login page

| # | What I tried | What happened | My take |
|---|---|---|---|
| 1 | Clicked Login with username and password empty | Both fields turned red with red X icons on the right. Red message under them: "Epic sadface: Username is required" | Works, but both fields are required and the message only names username. **Observation:** should it say both are required? |
| 2 | Username only, then Login | Message changed to "Password is required" | Looks OK |
| 3 | Password only, then Login | Message said "Username is required" | Looks OK |
| 4 | Correct username and password (without closing the error first) | Logged in | Looks OK |
| 5 | `standard_user` with a wrong password | "Username and password do not match any user in this service" | Looks correct |
| 6 | Clicked the X on the red message | Message cleared, fields went back to their normal color | Looks OK |
| 7 | `locked_out_user` / `secret_sauce` | "Sorry, this user has been locked out." | Looks correct: blocked, with a clear reason |

## Product list and product page (`standard_user`)

| # | What I tried | What happened | My take |
|---|---|---|---|
| 1 | Every option in the sort dropdown | Each sorted correctly | Looks OK |
| 2 | Clicked a product's name, and its photo | Both open a page for just that item, with description and price | Looks OK |
| 3 | Add to cart on the product page | Button turned into a red Remove button; a number in a red circle appeared on the cart icon (top right) | Looks OK |

## Cart

| # | What I tried | What happened | My take |
|---|---|---|---|
| 1 | Clicked the cart icon | Cart page lists the item with quantity and price; Remove and Continue Shopping (lower left), Checkout in green (right) | Looks OK |
| 2 | Remove | Works | Looks OK |
| 3 | Continue Shopping | Works | Looks OK |

## Checkout: your information

| # | What I tried | What happened | My take |
|---|---|---|---|
| 1 | Clicked Checkout | Page asks for first name, last name and zip code | Looks OK |
| 2 | Typed letters in the zip code field | Accepted, no error | **Observation:** no zip code format check (does a later step catch it?) |
| 3 | Left one field blank | Red error names the missing field | Looks OK |
| 4 | Left two fields blank | Error names only one of them | Same pattern as login |

## Checkout: overview and finish

| # | What I tried | What happened | My take |
|---|---|---|---|
| 1 | Continue to the overview page | Right item and price; tax and total correct ($15.99 + $1.28 tax = $17.27); free shipping; payment info already filled in | Looks OK |
| 2 | Finish | "Thank you for your order", cart icon empty | Looks OK |
| 3 | PDF receipt | Generated with the correct name, item, price, tax and total. Zip code shows as "abc" | Confirms letters in the zip code are accepted all the way through |
| 4 | Back Home | Returns to the product list | Looks OK |

## Menu

| # | What I tried | What happened | My take |
|---|---|---|---|
| 1 | Reset App State with an item in the cart | Item removed from the cart, cart number gone. But the item's button still says "Remove" even though the cart is empty | **Bug:** page and cart are out of sync |
| 2 | Logout | Works | Looks OK |
| 3 | Browser Back after logout | Stays on the login page (good). Shows "Epic sadface: You can only access '/inventory.html' when you are logged in". Text is cut off at the top and bottom, and both fields turn red | Access is blocked correctly. **Bug:** message text is clipped. **Observation:** wording isn't user friendly |

## Possible bugs (logged as GitHub Issues #1-#3, plus #7)
- **Password "view" eye is blocked by the error X.** After a wrong-password error, the red X icon sits on top of the eye, so you can't click it to reveal the password. (`standard_user`, login page) **Update 2026-11-25:** the eye is Edge's own show-password button (it only shows while typing in the field), so this is Edge only. Reproduced; see Issue #1.
- **Reset App State doesn't reset the "Remove" buttons.** The cart empties, but the item's button still says "Remove". (`standard_user`, product list)
- **Logged-out message is cut off.** After logout + browser Back, the error text is clipped at the top and bottom. (`standard_user`, login page)
- **Wrong-password error is cut off in Firefox.** (Found 2026-11-25 during the Firefox recheck.) The message wraps to three lines and the top and bottom are clipped. Fine in Edge. Probably the same fixed-height error box as the logged-out message. The logged-out message is cut off in Firefox too. See Issues #7 and #3.

## Observations (not bugs, questions for the product owner; Issues #4-#6)
- Form validation reports one missing field at a time, not all of them (login page and checkout page).
- Zip code accepts letters, and they print on the receipt ("abc"). Is any format expected?
- Logged-out message shows a technical path ("/inventory.html") and turns the fields red although the user typed nothing wrong. Suggest plain wording like "Please log in to continue."

## Still to explore (Session 2)
- `problem_user`, `error_user`, `performance_glitch_user`, `visual_user`
- Add to cart / Remove from the product list page (so far tested from the product page)
