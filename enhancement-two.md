---
layout: default
title: "Enhancement Two: Algorithms and Data Structures"
permalink: /enhancement-two
---

## Overview

The second enhancement added a Weight Analytics Dashboard that transforms raw weight data into meaningful insights using several distinct algorithms and efficient data structures. This enhancement demonstrates algorithmic problem-solving, computational complexity awareness, and comprehensive testing practices.

<div class="artifact-links">
  <a href="https://github.com/Niewski/CS360WeightTracker/tree/76e659e5cc046e28d372e604099ec7a84ce89eb1">&#128193; Original Code</a>
  <a href="https://github.com/Niewski/CS360WeightTracker/tree/analytics-dashboard">&#128640; Enhanced Code</a>
</div>

## What Changed

### Algorithms Implemented

| Algorithm | Purpose | Complexity |
|-----------|---------|------------|
| Sliding-Window Moving Average | Smooths daily fluctuations to reveal true weight trend | O(n) time, O(1) space per step |
| Linear Regression (Least Squares) | Computes weight-loss rate in lbs/week | O(n) time |
| Goal Date Projection | Extrapolates when user will reach goal weight | O(n) — uses regression result |
| Single-Pass Min/Max | Finds lowest and highest recorded weights with dates | O(n) time, O(1) space |
| Streak Counter | Detects consecutive days logged (current + longest) | O(n) time |
| Binary Search Support | `Comparable<WeightEntry>` with `equals()`/`hashCode()` for sorted collections | O(log n) lookup |

### Analytics Dashboard UI
- Summary cards displaying: current weight, goal weight, total weight change, rate of change (lbs/week)
- Statistics section: minimum, maximum, average, current streak, longest streak
- Projected goal date (or status message if goal reached or trend is flat/upward)
- 7-day moving average values displayed as a scrollable list

### Data Structure Improvements
- `WeightEntry` implements `Comparable<WeightEntry>` with natural date ordering
- Proper `equals()` and `hashCode()` based on primary key for reliable use in `Set` and `Map` collections
- `AnalyticsViewModel` with `LiveData` following the MVVM pattern established in Enhancement One

### Database Optimizations
- Added compound index on `(userId, date)` for efficient date-range queries
- Added `getWeightsInRange()` and date-filtered query methods

### Testing
- 28+ unit tests for `WeightAnalytics` covering: normal operation, edge cases (null/empty/single entry), boundary conditions (fewer entries than window size), and mathematical correctness
- ViewModel tests verifying analytics computation through the MVVM layer

## Skills Demonstrated
- Algorithm design and implementation (sliding window, regression, single-pass scan)
- Computational complexity analysis and trade-off management
- Comprehensive unit testing with edge case coverage
- Data structure design with proper Java contracts (`Comparable`, `equals`, `hashCode`)
- Database query optimization with compound indexes

---

## Narrative

<p>For my algorithms and data structures enhancement, I chose my CS360 Weight Tracker app, which I originally created in the CS 360 Mobile Architecture and Programming course. The app was built in Java using SQLite and Material Components, and it let users create accounts, log daily weights, view their history, delete entries, and get a one-time SMS notification when they reached their goal weight. I picked this artifact because it was already a complete and functional mobile app, but it did not really include much algorithmic complexity in its original form. Most of the logic was simple CRUD functionality, with just a basic comparison to check whether a user had reached their goal weight and a flat `ArrayList<WeightEntry>` to store the data. Because of that, it felt like a good project to enhance in a way that clearly showed growth in my ability to work with algorithms and data structures.</p>
<p>To improve the artifact, I added an analytics layer that made the app much more useful for the user. I created a WeightAnalytics class that included several different methods, such as a sliding-window moving average, linear regression for weight change over time, min/max scans, running average calculations, projected goal date logic, and consecutive-day streak counting. I also updated WeightEntry so it could be sorted more naturally by implementing Comparable, wrote JUnit tests to cover the analytics features and edge cases, and used an AnalyticsViewModel with LiveData so the new feature stayed consistent with the MVVM structure of the app. I also improved the database by adding a compound index on weights(userId, date) to help performance. These changes helped me meet Outcome 3 and Outcome 4 the most, since they show algorithmic problem-solving, testing, and the use of industry-relevant tools and practices.</p>
<p>Working on this enhancement taught me that even features that seem simple at first can involve a lot of careful decision-making. For example, the moving average logic had to be lined up with the correct dates, and the projected goal date feature needed protection against near-zero slopes so it would not return unrealistic results. I also ran into smaller but important challenges, like making sure the UI looked right across different screen densities, handling singular versus plural text correctly for streaks, and writing tests for unusual cases like empty lists or malformed dates. Overall, this enhancement made the app more useful, more polished, and a much stronger portfolio artifact because it shows both technical improvement and a better understanding of how to design software thoughtfully.</p>
