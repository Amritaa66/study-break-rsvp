# Study Break Night

## Use the website

For attendees: [RSVP here](https://amritaa66.github.io/study-break-rsvp/)

For organisers: [Open door check-in](https://amritaa66.github.io/study-break-rsvp/#door)

From the door page, organisers can open the check-in list using the **View check-ins** button.

## What it does

- Attendees submit an RSVP and see a separate confirmation page.
- Organisers check in attendees or walk-ins at the door.
- Each available cup receives a sequential cup number.
- Organisers can search and view the people checked in so far.
- RSVPs help estimate food and do not reserve cups.
- Cups are first come, first served.

## Technical notes

The app is a static `index.html` site hosted on GitHub Pages and connected to Supabase.

Supabase objects:

- `rsvps`: submitted names and timestamps.
- `cup_counter`: cups given and the maximum cup limit.
- `check_ins`: attendee or walk-in check-ins, timestamps, and cup numbers.
- `get_stats()`: returns RSVP, check-in, and cup counts.
- `check_in(text)`: matches a name, records the arrival, and atomically gives a cup when available.
- `get_check_ins()`: returns the check-in list for organisers.

## Counting behavior

- Before cups run out, checked-in people and cups given should usually match.
- After cups run out, people can still be checked in, so checked-in people may be greater than cups given.
- A check-in with no cup means the person arrived after the supply was exhausted.
- A name that does not match an RSVP is recorded as a walk-in.

## Known limitations

- Name matching is approximate. A typo or duplicate name can be treated as a walk-in or matched to the wrong RSVP.
- The same walk-in can technically be checked in more than once because the app does not collect a unique identifier.
- Weak Wi-Fi can delay or prevent RSVP and check-in requests; staff should wait for the result before submitting again.
- The public Supabase anon key can currently call the check-in RPC, and the door route is not protected.
- For a real event, add Supabase organiser authentication and require an authenticated organiser session for the door functions. Do not hardcode a PIN in `index.html`, since it would be visible in the page source.
