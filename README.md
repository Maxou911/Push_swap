# 🔄 Push Swap

> *"Sort data on a stack, with a limited set of instructions, using the lowest possible number of actions."*

A sorting algorithm project from [42 School](https://42.fr) — sort a stack of integers using two stacks and a restricted set of operations, with the **minimum number of moves possible**.

---

## 📚 About

**Push Swap** challenges you to implement an efficient sorting algorithm under strict constraints. You are given:

- **Stack A** — initially containing a random list of integers
- **Stack B** — initially empty

Your goal: sort stack A in **ascending order** using only the allowed operations, in as few moves as possible.

---

## 🕹️ Allowed Operations

| Operation | Description |
|---|---|
| `sa` | Swap the first two elements of stack A |
| `sb` | Swap the first two elements of stack B |
| `ss` | `sa` and `sb` simultaneously |
| `pa` | Push the top element of B onto A |
| `pb` | Push the top element of A onto B |
| `ra` | Rotate A upward (first element becomes last) |
| `rb` | Rotate B upward (first element becomes last) |
| `rr` | `ra` and `rb` simultaneously |
| `rra` | Reverse rotate A (last element becomes first) |
| `rrb` | Reverse rotate B (last element becomes first) |
| `rrr` | `rra` and `rrb` simultaneously |

---

## 🚀 Usage

### Compilation

```bash
make
```

### Run

```bash
./push_swap [list of integers]
```

### Examples

```bash
# Simple sort
./push_swap 3 1 2
# pb
# ra
# pa

# Random list
./push_swap 42 -7 0 15 -3

# Check number of operations
./push_swap 5 2 4 1 3 | wc -l

# Verify correctness with checker (bonus)
./push_swap 5 2 4 1 3 | ./checker 5 2 4 1 3
```

---

## ⚙️ Algorithm

### Small stacks (≤ 5 elements)

- **2 elements** — one `sa` if needed
- **3 elements** — hardcoded decision tree (at most 2 operations)
- **5 elements** — push 2 elements to B, sort 3 in A, then reinsert from B

### Large stacks (> 5 elements) — Turk Algorithm

The main algorithm used is the **Turk / Cost-based greedy algorithm**:

1. **Push all but 3 elements** from A to B in a semi-sorted order (chunks or cheapest cost)
2. **Sort the remaining 3** elements in A
3. **Re-insert each element** from B to A at the cheapest possible position, combining rotations (`rr`, `rrr`) when beneficial
4. **Rotate A** to bring the minimum element to the top

#### Cost Calculation

For each element in B, calculate the **total cost** to:
- Bring that element to the top of B
- Bring its target position to the top of A

Pick the element with the **lowest combined cost** and execute the move.

---

## 📊 Performance

Expected operation counts (typical benchmarks):

| Stack size | Max operations | Target |
|---|---|---|
| 3 | 3 | ≤ 3 |
| 5 | 12 | ≤ 12 |
| 100 | 700 | ≤ 700 |
| 500 | 5000 | ≤ 5500 |

> Grade at 42 depends on staying under the operation thresholds for 100 and 500 elements.

---

## 🛡️ Error Handling

The program writes `Error\n` to `stderr` and exits in the following cases:

- Non-integer argument
- Integer overflow (outside `INT_MIN` / `INT_MAX`)
- Duplicate values
- No arguments → no output (already sorted)

```bash
./push_swap 1 2 abc
# Error

./push_swap 1 2 2
# Error

./push_swap 2147483648
# Error
```

---

## ✅ Testing

```bash
# Generate random input and count operations
ARG=$(shuf -i 1-500 -n 100 | tr '\n' ' ') ; ./push_swap $ARG | wc -l

# Verify correctness
ARG="3 1 2 5 4" ; ./push_swap $ARG | ./checker $ARG

```

---
```

---

## 👤 Author

| Name | GitHub |
|------|--------|
| Your Name | [@Maxou911](https://github.com/Maxou911) |

---

## 📝 License

This project is part of the 42 School curriculum and is for educational purposes only.
