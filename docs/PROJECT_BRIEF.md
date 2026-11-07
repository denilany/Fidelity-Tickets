# Fidelity Tickets: Product Brief

## Purpose

Fidelity Tickets is an event ticketing platform for popular concerts with assigned seats. Fans browse event information, view a venue seat map, find events, and reserve and buy specific seats.

The project is intended to make the important engineering decisions visible: keeping a seat from being sold twice, surviving bursts of demand, handling checkout and payment outcomes, and making browsing usable when many fans are looking at the same event.

## Product scenario

A concert is announced and tickets go on sale at a known time. Many fans may arrive at once. They browse the concert and its seat map, choose one or more seats, and complete checkout. A seat may be held for a short period while its fan pays. If checkout is abandoned or fails, the seat should become available again. If payment succeeds, the booking is confirmed and tickets are issued.

## Core user requirements

1. Fans can view concert details, venue information, and a seat map with ticket availability.
2. Fans can search for concerts by keyword and date range.
3. Fans can select and book available seats.

## Core system qualities

- **Booking correctness:** the same event seat must not be sold to two fans.
- **Demand handling:** popular on-sales can attract a large simultaneous audience without overwhelming the booking path.
- **Fast discovery:** event viewing and search should remain responsive under heavy read load.
- **Availability and consistency:** browsing may briefly show stale availability; a committed booking must be correct.
- **Recoverability:** failed, delayed, repeated, or out-of-order payment and background events must not silently create duplicate bookings or tickets.

Initial scalability hypotheses are 10 million users interested in one popular event, sub-500 ms search latency, and a 100:1 read-to-write ratio. These are planning targets, not measured requirements; concrete capacity goals must be defined and validated before a production launch.

## Initial scope

### In scope

- Concert event pages and seat availability
- Event search
- Assigned-seat inventory per concert
- Temporary seat holds during checkout
- Booking and payment lifecycle
- Ticket issuance after confirmed payment
- High-demand traffic management and operational visibility as the design matures

### Below the line for the first implementation

- Event organizer and venue administration tools
- Resale, ticket transfer, and refunds beyond the compensation needed for a failed booking
- Dynamic pricing
- Season tickets, memberships, and presales
- Full anti-bot and identity/fairness systems
- Multiple ticket types such as general admission

## Key product invariant

For a given event and physical seat, there can be at most one active reservation and at most one completed sale. Seat availability displayed to a fan is informative; the booking path must re-check and enforce the invariant when a hold or purchase is made.

## Design approach

The design is organized into these stages:

1. Requirements and scope
2. Core entities
3. APIs
4. High-level design
5. Deep dives: reservation and contention; read scaling; popular-event traffic and real-time updates; search
6. Updated final design

The current checkpoint is after step 4's baseline high-level design. The next topic is temporary seat reservations and no-double-booking behavior. Technical choices should be validated against platform documentation and load measurements before production use.
