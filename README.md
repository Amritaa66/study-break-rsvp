# study-break-rsvp

## Current behavior and known limitations

- RSVPs are planning estimates; they do not reserve a cup.
- Door check-in records the person as attended and gives them a cup when one is available.
- Before cups run out, checked-in people and cups given should match.
- After cups run out, checked-in people can be greater than cups given because later arrivals are still recorded without receiving a cup.
- A name that does not match an RSVP is recorded as a walk-in.
- The public Supabase anon key can currently call the check-in RPC, so someone who discovers the door route could submit fake names or consume cups. Add a door PIN or organiser authentication before the event.
