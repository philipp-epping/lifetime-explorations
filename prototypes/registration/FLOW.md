# Flow: Registration

## Screens

1. **Register Start** — email entry
2. **Register Start (typing)** — email field active, user typing
3. **Register Start (error)** — email validation error
4. **Company Exists** — email domain matches an existing company
5. **Join Company** — enter name to request access to existing company
6. **Request Sent** — join request confirmation
7. **Details Entry** — enter name + company name (new company)
8. **Details Entry (with referral)** — same as above + referral code field
9. **Verification Sent** — modal confirming verification email was sent
10. **Welcome** — onboarding screen after email verification

### Emails

11. **Email: Registration Verification** — sent after company creation, contains link to finish setup
12. **Email: Invite** — sent when a team member is invited to join an existing company
13. **Email: Login Magic Link** — sent when any user requests a sign-in link

## Transitions

| From | Action | To |
|------|--------|----|
| Login entry | Click "Don't have an account? Get started" | Register Start |
| Register Start | Submit valid email (domain has existing company) | Company Exists |
| Register Start | Submit valid email (domain is new) | Details Entry |
| Register Start | Submit invalid email | Register Start (error) |
| Register Start | Click "Already have an account? Sign In" | Login entry |
| Company Exists | Click "Request to join [Company]" | Join Company |
| Company Exists | Click "Start a new company instead" | Details Entry |
| Join Company | Submit name | Request Sent |
| Join Company | Click "Back" | Company Exists |
| Request Sent | Click "Back to sign in" | Login entry |
| Details Entry | Submit name + company name | Verification Sent |
| Details Entry | Click "Back" | Register Start |
| Details Entry (with referral) | Submit name + company + referral code | Verification Sent |
| Details Entry (with referral) | Click "Put me on the waiting list" | Waitlist path (TBD) |
| Details Entry (with referral) | Click "Back" | Register Start |
| Verification Sent | Click "Back" | Details Entry |
| Verification Sent | Click "Resend link" | Stays on Verification Sent (re-sends email) |
| Verification Sent | User clicks link in email | Welcome |
| Welcome | Click "Connect bank account" | App (bank connection onboarding) |
| **Email triggers** | | |
| Verification Sent (screen) | System sends email | Email: Registration Verification |
| Email: Registration Verification | Click "Finish setup" | Welcome |
| Join Company → Request Sent | Admin approves request, system sends email | Email: Invite |
| Email: Invite | Click "Sign in to Aren" | Welcome (invited user variant) |
| Login entry → Check Inbox | System sends email | Email: Login Magic Link |
| Email: Login Magic Link | Click "Sign in to Aren" | App (authenticated) |

---

## Screen: Register Start

**Figma:** https://www.figma.com/design/dUqLCdZT2r2flWwPTQ8hSR/-Lifetime--Sandbox?node-id=1169-30894

- **Layout:** Split-screen. Left side is a narrow column with the registration form, wordmark, and footer. Right side shows a blurred/preview dashboard with KPI cards (Cashflow chart, Runway indicator, Profit after Delivery) — purely decorative, meant to give the user a taste of the product.
- **Content (left column, top to bottom):**
  - Headline: "Stop guessing your numbers."
  - Subline: "See your real profit, cash, and what's leaking in one place — so you can make decisions with confidence"
  - Text field: "Email address" (placeholder: "name@company.com")
  - Primary button: "Get started"
  - Below form: "Already have an account?" with "Sign In" action link
  - Footer area: Wordmark "Aren", legal links (Data protection, Imprint, AVV), language selector ("English")
- **Content (right column):**
  - Dashboard preview showing Cashflow card (149k €, +22% vs last month, chart with Total Cash / Spendable Cash lines), Runway indicator (42 days, Critical badge, "Cut cost or increase revenue"), Profit after Delivery card (112k € / 82%, 12.4% avg. P3 months)
- **Active states:** Button shows loading spinner and "Checking..." text while the email is being verified server-side
- **Empty state:** N/A — this is the entry point

## Screen: Register Start (typing)

**Figma:** https://www.figma.com/design/dUqLCdZT2r2flWwPTQ8hSR/-Lifetime--Sandbox?node-id=1169-32577

- Same layout as Register Start. Shows the email field in its focused/active state with text being typed ("kevin@greenfield.io").
- The "Get started" button remains in its default (enabled) state.
- Dashboard preview on the right shows slightly different KPI values (49k € Cashflow, +2%) — this is just a design variation, not a functional difference.

## Screen: Register Start (error)

**Figma:** https://www.figma.com/design/dUqLCdZT2r2flWwPTQ8hSR/-Lifetime--Sandbox?node-id=1359-55083

- Same layout as Register Start. The email field is in its **error** state.
- The field shows an incomplete/invalid value ("kevid") with a red error border.
- Below the field: inline error message "Please enter a valid email address".
- The "Get started" button is **disabled** while the email is invalid.

## Screen: Company Exists

**Figma:** none provided

- **Layout:** Centered card on a neutral background (same container as login screens).
- **Content:**
  - Icon: building icon in a blue/accent circle
  - Title: "[Company name] is already on Aren" (dynamic, based on matched company)
  - Description: "Someone with a @[domain] email already set up this company."
  - Primary button: "Request to join [Company name]"
  - Secondary button: "Start a new company instead"
- **Active states:** None beyond button hover/press
- **Empty state:** N/A

## Screen: Join Company

**Figma:** none provided

- **Layout:** Centered card.
- **Content:**
  - Title: "Join [Company name]"
  - Description: "We'll let the admin know you'd like to join."
  - Text field: "Your name" (placeholder: "Sarah Chen")
  - Primary button: "Send request" (shows spinner + "Sending..." while submitting)
  - Below: "Back" link
- **Active states:** Button shows loading state on submit
- **Empty state:** N/A

## Screen: Request Sent

**Figma:** none provided

- **Layout:** Centered card.
- **Content:**
  - Icon: green checkmark in a circle
  - Title: "Request sent"
  - Description: "We've notified the team at [Company name]. You'll get an email at [email] when you're approved."
  - Link: "Back to sign in"
- **Active states:** None
- **Empty state:** N/A

## Screen: Details Entry

**Figma:** https://www.figma.com/design/dUqLCdZT2r2flWwPTQ8hSR/-Lifetime--Sandbox?node-id=1169-33252

- **Layout:** Split-screen (same as Register Start). Left column has the details form, right column shows the dashboard preview.
- **Content (left column):**
  - Title: "Add your details"
  - Subtitle: "One last step, before you get your sign-in link."
  - Text field: "Your name" (filled: "Kevin Dukkon")
  - Text field: "Company name" (filled/focused: "Greenfield Co")
  - Header bar with two buttons: "Back" (outline) and "Send sign-in link" (primary)
  - Same footer as Register Start (wordmark, legal links, language selector)
- **Active states:** The focused text field shows a pressed/active border style. Button shows loading state on submit.
- **Empty state:** Fields start empty when the user hasn't typed; name may be pre-filled if guessable from the email address (e.g. "kevin.dukkon@..." → "Kevin Dukkon")

## Screen: Details Entry (with referral)

**Figma:** https://www.figma.com/design/dUqLCdZT2r2flWwPTQ8hSR/-Lifetime--Sandbox?node-id=1169-47068

- **Layout:** Same split-screen as Details Entry.
- **Content (left column):** Same as Details Entry, plus:
  - Additional text field: "Referral Code" (placeholder: "000-000-000", empty/unfilled state)
  - Below the referral field: "Don't have a code?" label with "Put me on the waiting list" action link
  - Header bar: "Back" (outline) and "Send sign-in link" (primary)
- **Active states:** Same as Details Entry
- **Empty state:** Referral code field is empty by default; the other fields behave the same as Details Entry

## Screen: Verification Sent

**Figma:** https://www.figma.com/design/dUqLCdZT2r2flWwPTQ8hSR/-Lifetime--Sandbox?node-id=1169-35111

- **Layout:** Modal dialog on top of the Details Entry screen. A dimming layer covers the background. The modal is centered.
- **Content (modal):**
  - Illustration at the top (decorative/email-themed)
  - Title: "Verification Sent"
  - Subtitle: "We sent a link to [email], to finish setting up [Company name]."
  - Primary button: "Resend link (30s)" — starts **disabled** with a countdown timer. Becomes enabled after the timer expires ("Resend link").
  - Secondary button: "Back" (outline) — returns to the details form
  - Close button (X icon) in the top-right corner of the modal
- **Active states:** Resend button transitions from disabled (with countdown) to enabled. Shows loading state when clicked.
- **Empty state:** N/A

## Screen: Welcome

**Figma:** https://www.figma.com/design/dUqLCdZT2r2flWwPTQ8hSR/-Lifetime--Sandbox?node-id=1169-35862

- **Layout:** Full app shell. Sidebar navigation is visible on the left (collapsed or expanded). Main content area shows the onboarding welcome view.
- **Content (sidebar):** Standard app navigation with company name ("Runway Inc." in the mockup), nav items.
- **Content (main area):**
  - Pill badge: "GET STARTED"
  - Title: "Welcome to Aren, let's get you up and running"
  - Section title: "STEPS"
  - Numbered step list:
    1. Connect your first bank account
    2. Transactions import
    3. We analyze your transactions
    4. You get your first financial overview
  - Bank logo icons (Deutsche Bank, Commerzbank, Sparkasse, Volksbank) — showing supported banks
  - Footer: Primary button "Connect bank account"
- **Active states:** None on initial load
- **Empty state:** This IS the empty/first-run state of the app

---

## Email: Registration Verification

**Figma:** none provided

- **Trigger:** Sent immediately after the user submits the Details Entry form (creating a new company).
- **Layout:** Standard transactional email. Sender avatar (Aren "A" circle), header with sender/recipient metadata, subject line, body, CTA button, footer.
- **Content:**
  - From: Aren \<hello@aren.app\>
  - Subject: "Finish setting up [Company name]"
  - Greeting: "Hi [First name],"
  - Body: "You're one click away from getting **[Company name]** set up on Aren. Click the link below to finish creating your account."
  - Primary CTA button: "Finish setup"
  - Expiry note: "This link expires in 24 hours. If it expires, go to aren.app and register again to get a new one."
  - Footer: "Aren · Your company's real numbers" + company address (Dorfstrasse 60, 8835 Feusisberg, Switzerland)
- **Link behavior:** Clicking "Finish setup" verifies the email, activates the account and company, and redirects to the Welcome screen.

## Email: Invite

**Figma:** none provided

- **Trigger:** Sent when an existing user invites a new team member to their company (or when an admin approves a join request).
- **Layout:** Same transactional email layout as above.
- **Content:**
  - From: Aren \<hello@aren.app\>
  - Subject: "[Inviter name] invited you to [Company name]"
  - Greeting: "Hi [Recipient name],"
  - Body: "**[Inviter name]** invited you to join **[Company name]** on Aren — where your team tracks the real financial numbers behind the business."
  - Primary CTA button: "Sign in to Aren"
  - Expiry note: "This link expires in 24 hours. If it expires, go to aren.app and enter your email to get a new one."
  - Context footer: "You received this email because [Inviter name] invited you to Aren. If you didn't expect this, you can safely ignore it."
  - Footer: Company address
- **Link behavior:** Clicking "Sign in to Aren" creates the user's account (if first time), links them to the company, and redirects to the Welcome screen showing "You've joined [Company name]."

## Email: Login Magic Link

**Figma:** none provided

- **Trigger:** Sent when any user (new or returning) submits their email on the login screen.
- **Layout:** Same transactional email layout.
- **Content:**
  - From: Aren \<login@aren.app\>
  - Subject: "Your login link for Aren"
  - Greeting: "Hi [First name],"
  - Body: "Here's your login link for Aren. It expires in 15 minutes."
  - Primary CTA button: "Sign in to Aren"
  - Safety note: "If you didn't request this link, you can safely ignore this email. No one can access your account without clicking the link above."
  - Footer: "Aren · Your company's real numbers" + company address
- **Link behavior:** Clicking "Sign in to Aren" authenticates the user and redirects to the app.
- **Key difference from other emails:** Shorter expiry (15 minutes vs. 24 hours), different sender address (login@ vs. hello@), no mention of a company name in the body.
