# inventory-reservation-lab
Oversell protection compared: Redis + MySQL ledger vs. MySQL SKIP LOCKED, with reproducible failure cases and lock analysis. Java 21, Testcontainers.

A hands-on reproduction of Shopify's "We replaced Redis with MySQL for inventory reservations" (2026). Three implementations of reserve/claim under concurrent load:
- naive: single-row UPDATE (contention baseline)
- redis: Lua DECR + MySQL ledger (shows the non-atomic claim failure)
- skiplocked: one row per unit with SELECT ... FOR UPDATE SKIP LOCKED

Each comes with a concurrency test asserting no oversell, plus notes on observed InnoDB lock behavior.
