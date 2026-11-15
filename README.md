# study-break-rsvp

Live site: https://amritaa66.github.io/study-break-rsvp/

## Current features

- The default page shows the Study Break Night event details and RSVP form.
- After a successful RSVP, the attendee is taken to a separate confirmation view at `#confirmation`.
- The confirmation explains that an RSVP helps estimate food but does not reserve a cup.
- Organisers use the door page at `#door`.
- Door staff enter an attendee name and select **Check in**.
- The check-in function matches an RSVP by name when possible and supports walk-ins.
- The result shows the cup number when a cup is available and identifies the person as RSVP'd or a walk-in.
- People can still be checked in after cups run out; those check-ins are recorded with no cup.
- Organisers can open the `#check-ins` page to search and view everyone checked in so far.
- The check-in list shows each person's RSVP/walk-in status, cup status, and check-in time.
- The app is designed for phone use at the door.

## Routes

- Public RSVP page: `https://amritaa66.github.io/study-break-rsvp/`
- Confirmation view: `https://amritaa66.github.io/study-break-rsvp/#confirmation`
- Organiser door page: `https://amritaa66.github.io/study-break-rsvp/#door`
- Check-in list: `https://amritaa66.github.io/study-break-rsvp/#check-ins`

## Supabase setup

The frontend uses the Supabase project configured in `index.html` and the following database objects:

- `rsvps`: stores submitted names and timestamps.
- `cup_counter`: stores the number of cups given and the maximum cup limit.
- `check_ins`: stores RSVP or walk-in check-ins, timestamps, and whether a cup was received.
- `get_stats()`: returns RSVP, check-in, and cup counts for the door screen.
- `check_in(text)`: matches a name, records the arrival, and atomically gives a cup when available.
- `get_check_ins()`: returns the check-in list for the organiser view.

## Counting behavior

- RSVPs are planning estimates; they do not reserve a cup.
- Before cups run out, checked-in people and cups given should usually match.
- After cups run out, checked-in people can be greater than cups given because later arrivals are still recorded without receiving a cup.
- A check-in with `received_cup = false` means the person arrived after the cup supply was exhausted.

## What could break

- A typo in a name may fail to match the RSVP and be recorded as a walk-in.
- Two attendees with the same name are matched to separate RSVPs one at a time; staff should confirm identity verbally.
- The same person may be difficult to distinguish from another person with the same name.
- Weak Wi-Fi can delay or prevent RSVP and check-in requests; staff should wait for the result before submitting again.
- The current check-in flow does not yet have a strong attendee identity check.

## Security limitation

The public Supabase anon key can currently call the check-in RPC, and the `#door` route is not protected. Someone who discovers the route could submit fake names or consume cups. Before a real event, add Supabase organiser authentication and require an authenticated organiser session for the door functions.

Do not add a PIN directly to `index.html`: a client-side PIN would be visible in the page source. A shared PIN should only be implemented through a server-side verification flow if organiser accounts are not practical.
