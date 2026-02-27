---
title: Two Pointers
theme:
  name: catppuccin-mocha
  override:
    code:
      alignment: left
      margin:
        percent: 0
---

# Two Pointers

The **two-pointer** technique uses two indices to traverse a data structure, reducing time complexity by avoiding nested loops.

<!-- end_slide -->

## Opposite Direction

Both pointers start at opposite ends and move toward each other.

```mermaid
flowchart TD
    subgraph Step1["Step 1"]
        direction LR
        A1["[L=0]"] --- A2["1"] --- A3["2"] --- A4["3"] --- A5["[R=4]"]
    end

    subgraph Step2["Step 2"]
        direction LR
        B1["0"] --- B2["[L=1]"] --- B3["2"] --- B4["[R=3]"] --- B5["4"]
    end

    subgraph Step3["Step 3"]
        direction LR
        C1["0"] --- C2["1"] --- C3["[L=R=2]"] --- C4["3"] --- C5["4"]
    end

    Step1 --> Step2 --> Step3
```

<!-- end_slide -->

**Algorithm:**
1. Set `left = 0` and `right = len - 1`.
2. While `left < right`:
   - Evaluate the condition at `arr[left]` and `arr[right]`.
   - If a match is found, return the result.
   - If the value is too small, increment `left`.
   - If the value is too large, decrement `right`.
3. Return the result.

<!-- end_slide -->

## Same Direction

Both pointers move in the same direction, often at different speeds or conditions.

```mermaid
flowchart TD
    subgraph Step1["Step 1"]
        direction LR
        A1["[i=0,j=0]"] --- A2["1"] --- A3["2"] --- A4["3"]
    end

    subgraph Step2["Step 2"]
        direction LR
        B1["[i=0]"] --- B2["[j=1]"] --- B3["2"] --- B4["3"]
    end

    subgraph Step3["Step 3"]
        direction LR
        C1["0"] --- C2["[i=1]"] --- C3["[j=2]"] --- C4["3"]
    end

    Step1 --> Step2 --> Step3
```

<!-- end_slide -->

**Algorithm:**
1. Set `i = 0` and `j = 0`.
2. While `j < len`:
   - If `arr[j]` satisfies the condition, process or swap `arr[i]` with `arr[j]`, then increment `i`.
   - Increment `j`.
3. Return the result.

<!-- end_slide -->

## Fast & Slow

One pointer moves faster than the other — commonly used for cycle detection and finding midpoints.

```mermaid
flowchart TD
    subgraph Step1["Step 1"]
        direction LR
        A1["[S=0,F=0]"] --- A2["1"] --- A3["2"] --- A4["3"] --- A5["4"]
    end

    subgraph Step2["Step 2"]
        direction LR
        B1["0"] --- B2["[S=1]"] --- B3["[F=2]"] --- B4["3"] --- B5["4"]
    end

    subgraph Step3["Step 3"]
        direction LR
        C1["0"] --- C2["1"] --- C3["[S=2]"] --- C4["3"] --- C5["[F=4]"]
    end

    Step1 --> Step2 --> Step3
```

<!-- end_slide -->

**Algorithm:**
1. Set `slow = head` and `fast = head`.
2. While `fast` and `fast.next` exist:
   - Move `slow` one step forward.
   - Move `fast` two steps forward.
   - If `slow == fast`, a cycle is detected — return.
3. Return `slow` (midpoint if no cycle).

<!-- end_slide -->

## Complexity

| Variant | Time | Space |
|---------|------|-------|
| Opposite Direction | O(n) | O(1) |
| Same Direction | O(n) | O(1) |
| Fast & Slow | O(n) | O(1) |
