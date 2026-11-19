# TicketTamer: SLASlayer Hack

A customer support ticketing web app that sorts tickets by priority, counts down SLA deadlines in real time, and gives managers a performance dashboard. Built with plain HTML, CSS and JavaScript, with no build step.

## Features

### Must-have
- **Ticket submission portal**: subject, service category (Billing, Technical, Account, Feature Request), priority and description.
- **SLA countdown clock**: every ticket shows the time left before its SLA is breached.
- **Kanban workflow board**: New / Unassigned, In Progress, Awaiting Customer, Resolved.
- **Agent assignment and triage**: assign any ticket to an engineer, with per-agent workload bars.
- **Performance analytics**: KPI cards, SLA compliance donut, tickets by category, hourly ticket volume and an agent efficiency table.
- **Breach warning badges**: green (safe), amber (approaching breach), red (breached). Amber and red badges pulse.

### Stretch
- **Automatic priority**: subject keywords raise a ticket's priority.
- **Canned responses**: pre-written billing, technical and account replies.
- **CSAT rating**: a 5-star prompt appears when a ticket is resolved.

## Getting started

1. Put `index.html`, `style.css` and `script.js` in the same folder.
2. Open `index.html` in a modern browser (Chrome, Edge, Firefox or Safari).

No install is needed. An internet connection is required for the Chart.js library and the Google Fonts, which load from CDNs. Without it the app still works, but the analytics charts will not draw and the fonts fall back to system fonts.

## How to use it

Use the buttons in the header to switch roles. You can also open a view directly with `#customer`, `#agent` or `#analytics` at the end of the page address.

| Role | What you can do |
|---|---|
| Customer Portal | Submit tickets, see recent tickets and their SLA state, rate resolved tickets |
| Support Agent Workspace | Claim tickets, assign agents, move tickets between stages, open a ticket to reply and add notes |
| Manager Analytics | Read the KPIs, charts and agent efficiency table |

Other controls: **Toggle theme** switches between light and dark, and **Reset demo data** restores the 20 sample tickets.

### Suggested demo
1. In Customer Portal, submit a ticket titled "Payment Gateway Crash".
2. It is raised to Urgent and a 2-hour clock starts.
3. Switch to Support Agent Workspace. The ticket appears in New with an SLA badge.
4. Click **Claim**, then open the ticket, add resolution notes and set it to Resolved.
5. Rate the ticket in the pop-up, then open Manager Analytics to see the numbers update.

## How it works

### SLA rules
| Priority | Resolution deadline |
|---|---|
| Urgent | 2 hours |
| High | 4 hours |
| Normal | 24 hours |
| Low | 48 hours |

- **Green**: more than half of the allowed time is left.
- **Amber**: less than half is left.
- **Red**: the deadline has passed.
- A resolved ticket shows "Resolved on time" or "Resolved late".
- One timer refreshes all badges every 10 seconds.

### Automatic priority
Priority is only ever raised, never lowered.

| Subject contains | Priority becomes |
|---|---|
| crash, outage, data loss, payment failed | Urgent |
| cannot login, can't log in, locked out, refund | High |

### Ticket lifecycle
New / Unassigned, then In Progress, then Awaiting Customer, then Resolved. Moving a ticket out of New records the first response time. If no agent is set, the least-loaded agent is assigned. Resolving a ticket records the resolution time and opens the CSAT prompt.

### Metrics
- **Total Tickets**: all tickets.
- **SLA Compliance %**: tickets resolved on time, out of resolved tickets plus open breached tickets.
- **Mean Response Time**: average time from creation to first response, in minutes.
- **Open Breaches**: unresolved tickets past their deadline.
- **Efficiency index** (per agent): on-time resolutions × 10 ÷ (1 + average hours to resolve).

## Project structure

```
tickettamer/
├── index.html   Page structure: header, three views, dialogs
├── style.css    Layout, light and dark themes, board, badges, responsive rules
├── script.js    SLA engine, state machine, rendering, analytics, demo data
└── README.md
```

`script.js` is organized in numbered sections: config, data, SLA engine, lifecycle state machine, customer view, agent board, ticket dialog, CSAT, analytics and event wiring.

## Data and storage

- The app starts with 20 sample tickets across all priorities and stages.
- Changes are saved in your browser's `localStorage` under the key `tt-v1`. Data stays on your device and is not shared between browsers.
- To start over, use **Reset demo data**.

## Customizing

Edit the config block at the top of `script.js`:
- `SLA`: deadline minutes per priority.
- `CATS`: service categories.
- `AGENTS`: the support team roster.
- `KEYWORDS`: patterns that raise priority.
- `CANNED`: canned reply library.

Theme colors are CSS variables at the top of `style.css`.

## Tech stack

- HTML5 with semantic elements
- CSS Grid and Flexbox, with light and dark themes
- Vanilla JavaScript (ES6+)
- [Chart.js](https://www.chartjs.org/) 4.4.1 for charts
- `localStorage` for persistence

## Limitations and next steps

- There is no sign-in; the role switch is for demonstration.
- Data lives in one browser. A Node.js and Express backend with SQLite would allow shared data and server-side SLA timers.
- Email or chat alerts when a ticket nears breach are not included.

## Credits

Built for the TicketTamer: SLASlayer Hack hackathon brief.
