# Test plan: Sauce Demo

App: https://www.saucedemo.com
Author: Kristina
Status: Draft, started 2026-11-25

## 1. Purpose

Test this online store so that its customers can log in, browse the products, choose what they want, pay for it, and then log out properly.

## 2. Scope

### In scope
- **Login:** valid and invalid logins, error messages, and the locked-out user
- **Product list:** sorting, and adding to or removing from the cart, both from the list and from the product page
- **Product page:** details, price, and Add to cart / Remove
- **Cart:** contents, Remove, Continue Shopping, Checkout
- **Checkout:** form checks on the information page; item, tax and total on the overview page; Finish; the receipt
- **Menu:** All Items, Reset App State, Logout, and what happens if you go Back after logging out
- **Test users:** `standard_user` and `locked_out_user` (done in Session 1); `problem_user`, `error_user`, `performance_glitch_user` and `visual_user` (Session 2)

### Out of scope
- Real payments and shipping (the site fakes them)
- Load and performance testing (beyond noting the delays `performance_glitch_user` has)
- Security testing
- Mobile apps, and a full accessibility review (anything noticed along the way gets logged as an observation)
- Footer social links and the "About" link, which go to other websites

### Browsers
- **Main:** Microsoft Edge (latest), on Windows 11
- **Cross-check:** Mozilla Firefox (latest): repeat the main shopping path and recheck any bug found in Edge

## 3. Approach

Testing combines three methods, each suited to a different job.

### Exploratory testing (manual)
- Time-boxed sessions of 30 to 45 minutes, each with a short goal (a "charter"). For example: "Explore checkout as `problem_user` to find where it behaves differently from `standard_user`."
- Notes are kept in a table: what I tried, what happened, my take. Session 1 is in `exploration-notes.md`.
- Finds the unexpected, and gives the written test cases their starting point.

### Written test cases (manual)
- For the paths customers can't do without: login, adding to and removing from the cart, checkout and its totals, and logout.
- Each case has an ID, title, priority, preconditions, steps, and an expected result for each step.
- Run in Edge first, then the high-priority cases again in Firefox.

### Automated tests
- Tool: Playwright (free, widely used), written in TypeScript.
- Automates the high-priority written cases: successful login, the login error messages, the locked-out user, the full purchase path, and the checkout totals.
- Runs in both Edge and Firefox, so each run is also the browser cross-check.
- Each automated test names the written case it covers, so it's clear which cases are automated and which are manual only.

### Retesting
- Every bug found is rechecked in the second browser before it's reported.
- The automated tests are run again after any change to them, and before the case study is finalized.

## 4. Test users and data
_To do_

## 5. Entry and exit criteria
_To do_

## 6. Risks
_To do_

## 7. Defect tracking and severity
_To do_
