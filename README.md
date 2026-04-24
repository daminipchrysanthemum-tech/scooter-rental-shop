## 🛵 Damini's Scooter Rental Shop

A Python object-oriented program simulating a scooter rental business — with invoice generation, tiered pricing, promotional discounts, full input validation, and a unit test suite that runs across multiple shop scenarios.

---

## 📌 Overview

Customers can walk into Damini's Rental Shop and rent one or more scooters by the hour, day, or week. The system checks availability, automatically applies group discounts, prints a formatted invoice, and validates all inputs before processing. The main() function runs an interactive customer session with an emergency stop, and a full unit test suite validates all pricing logic and edge cases across multiple shop instances.

---

## 💰 Pricing

| Period | Rate per Scooter |
|--------|-----------------|
| Hour   | $7.00           |
| Day    | $25.00          |
| Week   | $70.00          |

**Promotional Discount:** Renting **3–5 scooters** at once receives a **30% discount** on the total price. Rentals of 6 or more scooters do **not** qualify for the discount.

---

## 🧱 Class Design

### `DaminisRental`

| Attribute | Value | Description |
|-----------|-------|-------------|
| `available_scooters` | set at init | Fleet size for this shop instance |
| `hourly_rate` | $7 | Price per scooter per hour |
| `daily_rate` | $25 | Price per scooter per day |
| `weekly_rate` | $70 | Price per scooter per week |
| `discount_threshold` | 3 | Minimum scooters to qualify for discount |
| `discount_rate` | 0.30 | 30% off total when discount applies |

| Method | Parameters | Description |
|--------|-----------|-------------|
| `calculate_total()` | `num_scooters`, `rental_period` | Computes total price with discount logic; raises `ValueError` on bad input |
| `issue_invoice()` | `customer_name`, `num_scooters`, `rental_period` | Prints a formatted invoice to console |

---

## ✅ Validation & Error Handling

| Condition | Error Message |
|-----------|--------------|
| `num_scooters < 1` | `"Number of scooters must be at least 1."` |
| `num_scooters > available_scooters` | `"Not enough scooters available."` |
| `rental_period` not in `['hour', 'day', 'week']` | `"Invalid rental period."` |
| Non-integer input for scooter count | Caught in `main()` with a user-friendly message |

---

## 🖥️ Sample Invoice Output

```
Invoice:
  Customer Name:           Jane Smith
  Number of scooters:      3
  Rental period:           day
  Total due:               $52.50

```
---

## 🧪 Unit Tests

The test suite runs 11 cases across two shop instances (`shop_a` with 10 scooters, `shop_b` with 3 scooters) to simulate a multi-location business:

```python
# Pricing — no discount
assert shop_a.calculate_total(1, 'hour') == 7       # $7.00
assert shop_a.calculate_total(1, 'day')  == 25      # $25.00
assert shop_a.calculate_total(1, 'week') == 70      # $70.00

# Discount applies (3–5 scooters)
assert shop_a.calculate_total(3, 'day')  == 52.5    # $75  → 30% off → $52.50
assert shop_a.calculate_total(5, 'week') == 245.0   # $350 → 30% off → $245.00

# No discount at 6+ scooters
assert shop_a.calculate_total(6, 'hour') == 42      # $42, no discount

# Constraint violations → ValueError
shop_a.calculate_total(11, 'day')    # "Not enough scooters available."
shop_a.calculate_total(3, 'month')   # "Invalid rental period."
shop_a.calculate_total(0, 'day')     # "Number of scooters must be at least 1."

# Small shop (shop_b, 3 scooters)
assert shop_b.calculate_total(3, 'hour') == 14.70   # full inventory + discount
shop_b.calculate_total(4, 'day')     # "Not enough scooters available."
```

---

## 🐛 Bug Fixed

The original submission contained a constructor mismatch that caused a `TypeError` at runtime:

```python
# ❌ Original — crashes: missing customer_name when initializing shop
class DaminisRental:
    def __init__(self, customer_name, available_scooters):
        ...

shop = DaminisRental(available_scooters=10)  # TypeError
```

**Root cause:** `customer_name` belongs to an individual transaction, not the shop itself. A shop serves many customers without being re-created for each one.

```python
# ✅ Fixed — shop initializes cleanly; customer_name passed per transaction
class DaminisRental:
    def __init__(self, available_scooters):
        ...

shop = DaminisRental(available_scooters=10)
shop.issue_invoice("Jane Smith", 3, 'day')    # same shop, different customers
shop.issue_invoice("John Doe",   1, 'hour')
```

Built with Python 3 — no external dependencies.

---

## 🚀 Getting Started

```bash
python DaminisRentalShop.py
```

The program launches an interactive session. Enter `quit` as the customer name at any time to exit. Unit tests run automatically after the session ends.

---

## 📁 Repository Structure

```
scooter-rental-shop/
├── DaminisRentalShop.py     # Main program — fixed and fully documented
└── README.md
```
---

[DaminisRentalShop.py](https://github.com/user-attachments/files/27040210/DaminisRentalShop.py)

---

## 📚 Course

ITSS 4381 — Spring 2025
ITSS 4381 — Spring 2025
