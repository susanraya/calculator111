# Product Requirements

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
## Summary

### REQ-TECHSTACK · approved · source: requirements:technology-stack

Approved technology stack for a lightweight, browser-only calculator with minimal backend and no real data-storage needs.

- Backend: FastAPI 0.115 -- platform default, used only to serve the page since all computation happens client-side.
- Cloud: AWS -- platform default, hosts a single public page with no login, low/steady traffic.
- Database: PostgreSQL 16 -- platform default, included per standard but will sit unused since no data is persisted or kept between sessions.
- Frontend: React 19 -- platform default, builds the full single-page calculator UI (digits, operators, clear, equals, display, error state) and handles keyboard and on-screen input.

### REQ-SCOPE · approved · source: requirements:scope

Defines what the calculator will and will not do in this delivery (lightweight scope, requirements v1).

In scope: addition, subtraction, multiplication, division; equals to evaluate; numeric result display; clear to reset; on-screen number/operator buttons; keyboard entry of digits and operators; decimal point entry; division-by-zero and overflow error state; responsive desktop/mobile layout; anonymous, login-free use; free use with no payments; in-browser calculation with nothing saved between sessions.
Out of scope -- explicitly excluded: scientific functions (trig, logs, powers, parentheses), percentages, square root, and memory keys.

### REQ-FEAT-CALC · approved · source: requirements:functional:Doing calculations

Core arithmetic behavior: the four basic operations, equals evaluation, decimal entry, and a clear error state.

- Add, subtract, multiply and divide: take a first number, an operator, and a second number, show the result on equals; after a result, a new operator + number continues the calculation from the displayed result.
- Evaluate with equals: pressing equals evaluates the entered calculation and shows the numeric result; one operation handled at a time, like a standard pocket calculator.
- Enter decimal numbers: numbers may include a decimal point; only a single decimal point is kept per number regardless of repeated presses; results show with standard floating-point precision and a decimal point.
- Show a clear error state: dividing by zero or producing a result too large to represent shows a clear error state instead of a misleading number; pressing clear removes the error and returns the display to its initial value.

### REQ-FEAT-ENTRY · approved · source: requirements:functional:Entering & reading

How users enter numbers/operators and read the result: on-screen buttons, keyboard entry, and the numeric display with clear control.

- On-screen buttons: digits 0-9, the four operators, decimal point, equals and clear are all provided; tapping/clicking a digit adds it to the current number, tapping an operator applies it to the pending calculation -- usable by touch.
- Keyboard entry: digits, operators, decimal point and equals can be typed; each key behaves the same as the matching on-screen button.
- Numeric display and clear: the display shows the current number being entered or the evaluated result; clear resets the display to its initial value and discards any pending calculation.

### REQ-FEAT-ACCESS · approved · source: requirements:functional:Access & privacy

Access, device-compatibility, and privacy behavior: anonymous and free use, responsive layout, and no data retained between sessions.

- Anonymous and free to use: open to the general public, no login, account or user identification, no payment/billing/subscription ever requested; users can start calculating immediately.
- Responsive single-page layout: adapts to device screen -- display and buttons fit within the viewport on desktop, layout adjusts to keep buttons legible and tappable on mobile.
- No data saved between sessions: all calculation happens in the browser, nothing sent to or stored on a server; closing or reloading the page keeps no previous entries, results or history -- the calculator always starts fresh.

### REQ-NFR-001 · approved · source: requirements:NFR-001

Privacy: all calculation work stays in the user's browser.

Every calculation runs client-side; no calculation entries, results or history are sent to or stored on a server; nothing is retained after the page is closed or reloaded, so the calculator always reopens in its initial state.

### REQ-NFR-002 · approved · source: requirements:NFR-002

Compatibility: the calculator must work on both desktop and mobile browsers.

Single-page web app whose layout adapts to screen size: display and all buttons are laid out and usable within the viewport on desktop, and remain legible and tappable on mobile.

### REQ-NFR-003 · approved · source: requirements:NFR-003

Usability: no barriers to immediate use.

Anyone can open the page and start calculating right away -- no login, account creation, user identification or payment required at any point; both on-screen buttons and the physical keyboard are accepted as input.
<!-- sdlc:auto:end narrative -->

<!-- sdlc:auto:start summary -->
**Requirements version:** v1  
**Delivery scope:** lightweight  
**Approved:** 2026-11-27

**Capabilities in scope:** Addition, subtraction, multiplication and division, Equals to evaluate the entered calculation, Numeric result display, Clear to reset the display, On-screen number and operator buttons, Keyboard entry of digits and operators, Decimal point entry, Division-by-zero and overflow error state, Responsive layout for desktop and mobile browsers, Anonymous use with no login or accounts, Free to use with no payments, In-browser calculation with nothing saved between sessions

**Approved technology stack:**

| Layer | Choice | Rationale |
|---|---|---|
| backend | FastAPI | Your calculator does all its work in the browser and stores nothing on a server, so there is almost no backend job here beyond serving the page. FastAPI is the platform default and is more than enough to host the app; you are not asking it to process data, so a light backend is the right fit and keeps running costs near zero. |
| cloud | AWS | The app needs to be reachable on the public web for anyone, on any device, with no login — a small, steady hosting job. AWS is the platform default and comfortably handles serving a single-page calculator to the general public at low cost, scaling up only if usage spikes. |
| database | PostgreSQL | You were clear that nothing is stored and no history is kept between sessions, so the product genuinely needs no database. PostgreSQL is listed and is the platform default; it will simply sit unused, which is the honest answer when a product has no data to persist. |
| frontend | React | Your whole product lives in the browser: a responsive single-page calculator that works on desktop and mobile, takes keyboard and on-screen button presses, and does every calculation locally with nothing saved. React is built exactly for this kind of single-page interface, and it is the platform's default, so the four digits, operator keys, clear, equals, display and error state can all be built and tested here without anything else. |
<!-- sdlc:auto:end summary -->

<!-- sdlc:auto:start epics -->
## EPIC-001: Basic Arithmetic Calculation

The calculator evaluates basic arithmetic — addition, subtraction, multiplication and division — entered one operation at a time like a standard pocket calculator, with decimal-point support and a clear error state for division by zero and overflow. Scientific functions, percentages, square root and memory keys are explicitly excluded from this version. All calculation happens in the browser using standard floating-point precision.

### US-001: Perform addition, subtraction, multiplication and division

**Priority:** critical

> As a member of the public using the calculator, I want to enter a number, an operator, and a second number and get the result, so that I can perform everyday arithmetic like a pocket calculator.

| ID | Given | When | Then |
|---|---|---|---|
| AC-001 | the display shows the first operand | I select the addition operator, enter a second operand, and press equals | the display shows the sum of the two operands |
| AC-002 | the display shows the first operand | I select the subtraction operator, enter a second operand, and press equals | the display shows the difference of the two operands |
| AC-003 | the display shows the first operand | I select the multiplication operator, enter a second operand, and press equals | the display shows the product of the two operands |
| AC-004 | the display shows a non-zero first operand | I select the division operator, enter a non-zero second operand, and press equals | the display shows the quotient of the two operands |

### US-002: Evaluate the entered calculation with equals

**Priority:** critical

> As a member of the public using the calculator, I want to press equals to evaluate the calculation I have entered, so that I see the result of my arithmetic.

| ID | Given | When | Then |
|---|---|---|---|
| AC-005 | I have entered an operand, an operator and a second operand | I press equals | the display shows the evaluated numeric result |
| AC-006 | the display shows a result from a previous equals | I select an operator and enter a new operand and press equals | the calculation continues from the previously displayed result |

### US-003: Enter decimal numbers

**Priority:** high

> As a member of the public using the calculator, I want to enter numbers with a decimal point, so that I can calculate with non-whole values.

| ID | Given | When | Then |
|---|---|---|---|
| AC-007 | the display shows a number with no decimal point | I press the decimal point and then enter further digits | the display shows the number with a single decimal point and the fractional digits |
| AC-008 | the current number already contains a decimal point | I press the decimal point again | no additional decimal point is added to the current number |
| AC-009 | two decimal operands have been entered | I press equals | the result is shown using standard floating-point precision with a decimal point |

### US-004: Clear error state for division by zero and overflow

**Priority:** high

> As a member of the public using the calculator, I want to see a clear error state when a calculation cannot produce a valid result, so that I understand the result is invalid rather than seeing a misleading number.

| ID | Given | When | Then |
|---|---|---|---|
| AC-010 | the display shows a number and division is selected | I enter zero as the divisor and press equals | the display shows a clear error state instead of a number |
| AC-011 | a calculation produces a value too large for the calculator to represent | I press equals | the display shows a clear error state instead of a number |
| AC-012 | the display is showing an error state | I press clear | the error state is cleared and the display returns to its initial value ready for a new calculation |

## EPIC-002: Calculator Input and Display

Users enter numbers and operators using on-screen buttons or the physical keyboard, and read results from a numeric display that can be reset with a clear control. On-screen buttons cover digits, the four operators, decimal point, equals and clear.

### US-005: Enter digits and operators with on-screen buttons

**Priority:** high

> As a member of the public using the calculator, I want on-screen buttons for digits, operators, decimal point, equals and clear, so that I can use the calculator by tapping or clicking, including on a touch device.

| ID | Given | When | Then |
|---|---|---|---|
| AC-013 | the calculator is displayed | I tap or click a digit button | that digit is appended to the current number on the display |
| AC-014 | a number is shown on the display | I tap or click one of the four operator buttons | that operator is applied to the pending calculation |
| AC-015 | the calculator is displayed | I view the on-screen controls | buttons for digits 0 to 9, addition, subtraction, multiplication, division, decimal point, equals and clear are available |

### US-006: Enter digits and operators with the keyboard

**Priority:** medium

> As a member of the public using the calculator on a device with a keyboard, I want to type digits and operators on my keyboard, so that I can enter calculations quickly without using the mouse or touch.

| ID | Given | When | Then |
|---|---|---|---|
| AC-016 | the calculator is displayed | I press a digit key on the keyboard | that digit is appended to the current number on the display |
| AC-017 | the calculator is displayed | I press a keyboard key for an operator, decimal point or equals | the calculator responds the same as pressing the corresponding on-screen button |

### US-007: Read and clear the numeric display

**Priority:** critical

> As a member of the public using the calculator, I want a numeric display showing my current entry or result, and a clear control to reset it, so that I can see what I am calculating and start over when I want.

| ID | Given | When | Then |
|---|---|---|---|
| AC-018 | I am entering a number or have evaluated a result | I view the calculator | the display shows the current number or result |
| AC-019 | the display shows a number, operator or result | I press clear | the display resets to its initial value and any pending calculation is discarded |

## EPIC-003: Access, Platform and Privacy

The calculator is a free, responsive single-page web application for anonymous general-public users, with no login or accounts. It runs calculations entirely in the browser and stores nothing between sessions, including no calculation history. There are no payments, billing or subscriptions.

### US-008: Use the calculator anonymously and free of charge

**Priority:** high

> As a member of the general public, I want to use the calculator immediately without signing in or paying, so that I can do a quick calculation with no barriers.

| ID | Given | When | Then |
|---|---|---|---|
| AC-020 | I open the calculator for the first time | the page loads | I can use it immediately with no login, account creation or user identification required |
| AC-021 | I am using the calculator | I complete any calculation | no payment, billing or subscription is requested at any point |

### US-009: Use the calculator on desktop and mobile browsers

**Priority:** high

> As a member of the public on a desktop or mobile device, I want a responsive single-page layout that adapts to my screen, so that the calculator is usable whatever device I open it on.

| ID | Given | When | Then |
|---|---|---|---|
| AC-022 | I open the calculator on a desktop browser | the page loads | the display and all buttons are laid out and usable within the viewport |
| AC-023 | I open the calculator on a mobile browser | the page loads | the layout adapts to the smaller screen and all buttons remain legible and tappable |

### US-010: Calculate in the browser with nothing persisted

**Priority:** medium

> As a member of the public using the calculator, I want all calculation to happen in my browser with nothing saved between sessions, so that my calculations stay private and nothing is stored about me.

| ID | Given | When | Then |
|---|---|---|---|
| AC-024 | I perform calculations in the calculator | the calculation runs | it is computed in the browser with no calculation data sent to or stored on a server |
| AC-025 | I have performed calculations and then close or reload the page | I open the calculator again | no previous entries, results or history are retained and the calculator starts in its initial state |
<!-- sdlc:auto:end epics -->
