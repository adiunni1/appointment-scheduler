# Appointment Scheduler

## What This Is

A Python implementation of an appointment scheduling system that tracks a list of appointments and automatically detects any **scheduling conflicts** — cases where two appointments overlap in time.

The project is built around two classes: `Appt` (a single appointment) and `Agenda` (a collection of appointments), and demonstrates object-oriented design using Python's special methods to make the objects work naturally with comparison operators and built-in functions.

---

## How It Works

### `Appt` — A Single Appointment

Each appointment has a start time, an end time, and a description. The class defines comparison operators so appointments can be compared and sorted intuitively:

| Operator | Meaning |
|----------|---------|
| `appt1 < appt2` | `appt1` ends before or exactly when `appt2` starts (no overlap) |
| `appt1 > appt2` | `appt1` starts after or exactly when `appt2` ends (no overlap) |
| `appt1 == appt2` | Both appointments cover the exact same time period |

Two helper methods build on these:

- **`overlaps(other)`** — returns `True` if there is any time overlap between the two appointments
- **`intersect(other)`** — returns a new `Appt` representing the overlapping time window, or `None` if there is no overlap

### `Agenda` — A Collection of Appointments

An `Agenda` holds a list of `Appt` objects and provides two key operations:

- **`sort()`** — sorts all appointments by start time
- **`conflicts()`** — returns a new `Agenda` containing only the overlapping portions of any conflicting appointments

The conflict detection algorithm sorts appointments first, then uses a nested loop with an early exit: once it finds an appointment that starts after the current one ends, it can skip the rest (since all subsequent appointments will start even later).

---

## Example

```python
from datetime import datetime
from appointments import Appt, Agenda

appt1 = Appt(datetime(2024, 3, 15, 13, 30), datetime(2024, 3, 15, 15, 30), "Early afternoon nap")
appt2 = Appt(datetime(2024, 3, 15, 15, 0),  datetime(2024, 3, 15, 16, 0),  "Coffee break")

agenda = Agenda()
agenda.append(appt1)
agenda.append(appt2)

conflicts = agenda.conflicts()
print(conflicts)
# 2024-03-15 15:00 15:30 | Overlap
```

---

## Key Design Decisions

- **Comparison operators** (`__lt__`, `__gt__`, `__eq__`) are defined in terms of the *time period*, not the description — two appointments at the same time are "equal" regardless of what they're called
- **`overlaps` is derived from `__lt__` and `__gt__`** rather than implementing its own logic, keeping the definition clean and consistent
- **Conflict detection uses an early exit** — after sorting, once a non-overlapping later appointment is found, the inner loop breaks, making the algorithm more efficient than a naive double loop

---

## Dependencies

- Python 3.10+
- Standard library only (`datetime`)

---

## Files

- **`appointments.py`** — contains both the `Appt` and `Agenda` classes, plus a short demo in the `__main__` block

---

## Author

Adi Unni — github.com/adiunni1
