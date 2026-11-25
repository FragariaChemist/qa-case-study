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

### Test users
Sauce Demo provides these users on its login page. They all use the password `secret_sauce`.

| User | What it's for |
|---|---|
| `standard_user` | The normal customer. The baseline the other users are compared against |
| `locked_out_user` | Checks that a blocked account can't log in and is told why |
| `problem_user` | Built to show broken behaviour in places. Checks which features fail |
| `performance_glitch_user` | Built to be slow. Checks that the site still works, and how long it takes |
| `error_user` | Built to hit errors on some actions. Checks how the site handles them |
| `visual_user` | Built with layout and display differences. Checks what looks wrong |

### Checkout data
| Set | First name | Last name | Zip code | Purpose |
|---|---|---|---|---|
| Valid | `Test` | `Customer` | `12345` | Normal checkout |
| Name with punctuation or accents | `José` | `O'Brien-Smith` | `12345` | Real names with special characters are accepted |
| Letters in zip | `Test` | `Customer` | `abc` | Zip format check (Session 1 found none) |
| Spaces only | (space) | (space) | (space) | Blank-looking values are rejected |
| Very long values | 100+ characters | 100+ characters | 20+ digits | Long values don't break the page or receipt |
| Missing fields | Leave each field empty, one at a time and all together | | | Error messages name the right field |

### Expected prices
- Tax is 8% of the item total, rounded to the cent. Example from Session 1: $15.99 + $1.28 tax = $17.27.
- Checkout totals are checked with one item and with several items, so that rounding on a larger total is covered too.

## 5. Entry and exit criteria

### Ready to start testing an area when
- The site is up and the test users can log in
- The written test cases for that area are drafted
- Edge and Firefox are up to date
- For automated tests: Playwright is installed and a sample test runs

### Pause testing if
- The site is down or very slow for everyone (not just `performance_glitch_user`). Note the time and try again later

### Testing is done when
- Every high-priority written test case has been run in Edge and Firefox, with results recorded
- Each of the six users has had at least one exploratory session
- Every bug found is logged as a GitHub Issue, with a severity and steps to reproduce
- The automated tests pass, apart from any that fail because of a known, logged bug (those are marked with the issue number)
- A short summary of what was tested, what was found, and what's left is written up

## 6. Risks

| Risk | Effect | How it's handled |
|---|---|---|
| The site is a public demo I don't control | It could change or go down, and bugs won't be fixed | Note the date and browser on every result and bug; re-run before finalizing |
| The test users are built to misbehave | Hard to tell a deliberate fault from a real bug | Compare each user against `standard_user` and report the difference either way |
| `performance_glitch_user` is slow | Automated tests may time out and fail for the wrong reason | Give those tests longer waits, and record how long the slow steps take |
| No requirements document | The expected behaviour is sometimes a judgment call | Log those as questions for the product owner, not bugs |
| Testing in two browsers only | Problems in Chrome, Safari or on mobile could be missed | Stated in scope; Edge is built on the same engine as Chrome |
| One tester | Things I don't think of go untested | Mix written cases with exploratory sessions, which go beyond the scripts |

## 7. Defect tracking and severity

### Where
Bugs and questions are logged as GitHub Issues in this repository, using the bug report template.

Each bug includes:
- A short title that says what's wrong and where
- The user, browser and date
- Steps to reproduce
- Expected and actual results
- A screenshot or recording
- A severity label

### Labels
- `bug`: the site does something wrong
- `question`: behaviour that might be intended, to check with the product owner (Session 1's "observations")
- Severity: `severity: critical`, `severity: high`, `severity: medium`, `severity: low`

### Severity
| Severity | Meaning | Example |
|---|---|---|
| Critical | Customers can't log in, buy, or log out, or they're charged the wrong amount | Checkout total doesn't match the items plus tax |
| High | A main feature fails, but there's a way around it | Add to cart doesn't work from the product list, but does from the product page |
| Medium | A feature works incorrectly in a way customers would notice | Reset App State empties the cart but leaves the "Remove" buttons |
| Low | Cosmetic, or a minor problem that's easy to work around | Logged-out message is cut off at the top and bottom |
