# RSVP Bookkeeping Plan (Supabase + Magic Link)

## Goal
Create a lightweight reservation/RSVP bookkeeping system for wedding guests without running a full custom backend server.

## Why This Approach
- Keeps hosting/ops simple.
- Uses secure, proven auth flow (magic links).
- Supports guest self-service with low friction.
- Allows admin reporting and tracking.

## Recommended Stack
- Astro site (existing frontend)
- Supabase Postgres (data storage)
- Supabase Auth (magic-link sign-in)
- Supabase Row Level Security (RLS)
- Optional Supabase Edge Functions (admin-only or bulk actions)

## Core User Flow
1. Host imports/maintains guest list with email addresses.
2. Host sends RSVP access email with one-click secure link.
3. Guest opens link, gets authenticated session, and lands on RSVP page.
4. Guest views and updates their RSVP details.
5. Admin views aggregate totals and exports data.

## Email Strategy (Given Current Context)
We already sent guest emails via personal Gmail using YAMM.

Use that to our advantage:
- Send RSVP emails from the same sender/account for continuity.
- Keep subject and copy consistent with prior communication.
- Use clear wedding context in every message (names/date/location).
- Send in manageable batches to avoid throttling.

## Deliverability Notes
Potential issues can still occur, but risk is manageable:
- Gmail/YAMM daily limits still apply.
- Large bursts can trigger temporary throttling.
- Some guest inbox providers may still filter links.

Mitigations:
- Send in smaller batches.
- Avoid spammy phrasing.
- Include clear fallback: "Need a fresh link?"
- Track bounces/non-responders.

## Data Model (MVP)
### profiles
- id (uuid, maps to auth user id)
- email
- full_name
- is_admin (boolean)

### guests
- id
- name
- email (unique)
- can_bring_plus_one (boolean)
- dietary_notes

### rsvps
- id
- guest_id
- event_code (e.g., welcome_dinner, wedding_day)
- status (attending, declined, pending)
- party_size
- notes
- updated_at

Optional later:
- households table for families/plus-ones
- invites table for invite lifecycle tracking

## Security Model
Use RLS on all data tables.

Baseline policies:
- Authenticated users can only read/update records tied to their own identity/email.
- Admin users can read/update all records.
- Anonymous users cannot write RSVP data.

Hardening option:
- After magic-link login, request one extra check (e.g., last name or invite code) before showing full details.

## "No Full Backend" Clarification
This plan avoids a custom always-on backend.

You may still add tiny serverless functions for:
- Bulk link generation/sending
- Admin CSV export
- Seat cap validation and conflict-safe writes

## Implementation Phases
## Phase 1: Foundation
- Create Supabase project.
- Define schema and constraints.
- Enable and test RLS policies.
- Add environment variables to Astro project.

## Phase 2: Auth + Guest RSVP
- Build magic-link login flow.
- Build RSVP read/update form.
- Add success/error states and guardrails.

## Phase 3: Admin + Operations
- Add admin summary page (counts/statuses by event).
- Add CSV export.
- Add resend/fallback link workflow for guests.

## Phase 4: Reliability
- Add logging/audit for admin changes.
- Add reminder cadence for non-responders.
- Add basic validation for party size and required fields.

## Decisions Captured
- Keep the guide page/content work as-is.
- Use Supabase-first architecture.
- Prefer host-initiated magic-link email flow.
- Leverage existing Gmail/YAMM sender trust and list.

## Open Questions for Later
- Do we need household-level RSVP editing or individual-only?
- Is plus-one always fixed per guest, or guest-selectable?
- Which events need RSVP tracking (single event vs multi-event)?
- Do we want automatic reminders from the system or manual YAMM sends?

## Next Session Starting Checklist
- Confirm final RSVP fields and events.
- Finalize schema + RLS SQL.
- Decide email generation/sending path (Supabase email vs YAMM merge flow).
- Implement MVP UI flow in Astro.
