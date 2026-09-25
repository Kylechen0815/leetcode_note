# LeetCode Algorithms & Engineering Notes

A structured repository containing algorithmic problem breakdowns, fundamental data structure patterns, and practical firmware/BSP engineering logs.

---

## Repository Index

### 1. Data Structures & Algorithms

| Topic | Note File | Key Concepts & Patterns |
| :--- | :--- | :--- |
| **Monotonic Stack** | [`monotonic stack.md`](monotonic%20stack.md) | Next Greater Element, Histogram Area, Range Minimums, O(N) Traversal |
| **Trees & Graphs** | [`Binary Tree,Hash table, dfs.md`](Binary%20Tree%2CHash%20table%2C%20dfs.md) | Tree Traversals, Lowest Common Ancestor, DFS Recursion, Subtree Evaluation |
| **Dynamic Programming** | [`DP, matrix.md`](DP%2C%20matrix.md)<br>[`dynamic_programming(1).md`](dynamic_programming(1).md)<br>[`dynamic_programming(2).md`](dynamic_programming(2).md) | State Transitions, 1D/2D Memoization, Matrix Paths, Subsequence Optimization |
| **Binary Search** | [`binary_search(1).md`](binary_search(1).md) | Boundary Condition Handling, Search Space Reduction, Rotated Arrays |
| **Advanced Structures** | [`segment tree.md`](segment%20tree.md) | Range Query and Update, Tree Decomposition |

### 2. Embedded & System Engineering Logs

| Category | Note File | Focus Areas |
| :--- | :--- | :--- |
| **BSP & Tooling** | [`working_diary.md`](working_diary.md) | MediaTek SP Flash Tool configuration, Android ADB debugging, terminal workflows |

---

## Algorithmic Solution Framework

Each algorithmic problem entry is structured around a reproducible problem-solving methodology:

1. **Problem Statement & Constraints**:
   - Extraction of input/output requirements, edge cases, and constraint bounds.
2. **Visual & Intuitive Analysis**:
   - Diagrammatic representation of pointers, tree structures, or stack states.
3. **Complexity Guarantees**:
   - Optimal time complexity (e.g., reducing $O(N^2)$ to $O(N)$ using Monotonic Stacks).
   - Space complexity evaluation for recursion stacks and auxiliary memory.
4. **Implementation & Refactoring**:
   - Clean, idiomatic source code with considerations for boundary checks.

---

## Highlighted Problem Categories

### Monotonic Stack Pattern
Focuses on problems requiring linear-time solutions for finding the nearest larger or smaller element. Key examples include calculating the largest rectangular area under a histogram and processing stock spans.

### Dynamic Programming & Matrix Traversal
Covers optimal substructure identification, recurrence relations, and memory optimization (space reduction from 2D arrays to 1D sliding windows).

### Binary Trees and DFS
Explores tree structure manipulation, depth evaluation, path tracing, and recursive partitioning strategies.

---

## Embedded Engineering Logs

The `working_diary.md` tracks hands-on firmware and system bring-up notes, including:
- **MediaTek (MTK) Flashing Workflow**:
  - Image flashing procedures using `SP_Flash_Tool_Selector`.
  - Execution permission setup and file system access handling on Linux environments.
- **Android User-Build Debugging**:
  - Enabling developer mode and USB/wireless debugging on target devices.
  - Device verification and shell access via `adb devices` and `adb shell`.

---

## Usage

Browse the corresponding Markdown files directly on GitHub or clone the repository locally for reference:

```bash
git clone https://github.com/Kylechen0815/leetcode_note.git
cd leetcode_note
