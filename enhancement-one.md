---
layout: default
title: "Enhancement One: Software Design and Engineering"
permalink: /enhancement-one
---

## Overview

The first enhancement refactored the CS360 Weight Tracker from a tightly coupled Activity-based MVC architecture to the Model-View-ViewModel (MVVM) pattern using Android's `ViewModel` and `LiveData` components. This modernization introduced a Repository layer, added new user-facing features, and fixed a critical SMS notification bug.

<div class="artifact-links">
  <a href="https://github.com/Niewski/CS360WeightTracker/tree/76e659e5cc046e28d372e604099ec7a84ce89eb1">&#128193; Original Code</a>
  <a href="https://github.com/Niewski/CS360WeightTracker/tree/mvvm-refactor">&#128640; Enhanced Code</a>
</div>

## What Changed

### Architecture Refactor
- **Repository Layer**: Created `WeightRepository` and `UserRepository` classes that wrap `DatabaseHelper` and expose clean interfaces. This decouples ViewModels from the database implementation, making future changes (caching, async operations, or migrating to Room) straightforward.
- **ViewModels**: Created `WeightHistoryViewModel`, `AddWeightViewModel`, and `LoginViewModel` that encapsulate all business logic previously embedded in Activities. Activities became thin view layers that observe `LiveData` and respond to user input.
- **LiveData Observables**: UI updates are driven by observable `LiveData` objects, making the app resilient to Android lifecycle events like screen rotation — ViewModel data survives configuration changes.

### New Features
- **Edit Weight**: Added `EditWeightActivity` with an `updateWeight()` method in `DatabaseHelper`, allowing users to correct entries without deleting and re-entering them.
- **User Profile Screen**: Added `ProfileActivity` where users can view and update their goal weight and phone number after account creation.

### Data Layer
- Added `UserProfile` POJO for profile data
- Moved `WeightEntry` from the features layer to the data layer to eliminate a reverse dependency
- Added `updateWeight()`, `getUserProfile()`, and `updateUser()` methods to `DatabaseHelper`

## Skills Demonstrated
- Modern Android architecture patterns (MVVM, Repository, LiveData)
- Separation of concerns and lifecycle awareness
- Refactoring legacy code without breaking existing functionality
- Iterative development with code reviews and plan-review-fix cycles

---

## Narrative

<p>For my ePortfolio artifact, I chose the CS 360 Weight Tracker application. This was an Android app I originally created during the CS 360 Mobile Architecture and Programming course. The app lets users create an account, log daily weights, view their weight history, delete entries, and receive a one-time SMS notification when they reach their goal weight. In the original version, the project used a basic Activity-based MVC structure, so most of the user interface code, business logic, and database calls were all handled in the same place. Looking back, it worked for a class project, but it also showed a lot of areas where the design could be improved.</p>
<p>I picked this artifact because it gave me a great chance to challenge myself as a developer. One of the biggest improvements I made was refactoring the app to use an MVVM architecture. Originally the Activities handled almost everything, including database access, business logic, and UI updates. In the enhanced version I separated those responsibilities by adding ViewModels, LiveData, and repository classes. That made the code much cleaner and easier to maintain. I also added two new features. An Edit Weight screen and a User Profile screen. These were meaningful improvements because they fixed real usability issues. For example, users no longer have to delete and re-enter a weight just to fix a mistake, and they now have a dedicated place to update profile information like their goal weight and phone number.</p>
<p>Another major improvement was in testing and documentation. The original project basically had no meaningful tests, so adding unit tests for multiple ViewModels and other supporting classes made the project much stronger. It gave me more confidence that future changes would not break existing functionality. I also added Javadoc comments throughout the code and updated the README so the project would be easier for someone else to understand. That part was important to me because it made the app feel more like a professional project instead of just a school assignment. These changes also connect well to the course outcomes, especially in the areas of collaboration, communication, and using well-founded development practices. The new structure makes the project easier for another developer to follow, and the added documentation helps explain how the application works.</p>
<p>This enhancement process taught me a lot about real software engineering challenges. One lesson I learned was that the order of changes during a refactor really matters. Some early steps in the plan depended on files that had not been created yet, which would have caused the project not to compile. That showed me how important it is to plan refactors carefully so the project stays buildable at each stage. I also ran into issues with ViewModel initialization and screen recreation, which helped me better understand why proper state management matters in Android development.</p>
<p>Overall, this was a strong artifact for my ePortfolio because it shows both where I started and how much I improved. The enhanced version of the Weight Tracker is more organized, more maintainable, and more realistic as a professional application. It clearly demonstrates growth in software design, testing, and documentation. At the same time, it also helped me recognize areas that still need improvement. For example, the app still stores passwords in plain text, so even though I improved some security-related areas, I know that this part of the application would still need more work to fully meet security expectations.</p>
