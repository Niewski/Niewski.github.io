---
layout: default
title: Professional Self-Assessment
permalink: /
---

## Professional Self-Assessment

Going through the Computer Science program at Southern New Hampshire University has been a really meaningful experience for me. It helped me grow not only in my technical skills, but also in the way I think about myself as a software developer. Looking back, I can see how much I have improved in areas like coding, problem-solving, testing, documentation, and thinking more carefully about security and design. For my capstone I had the chance to return to an earlier project, the CS360 Weight Tracker Android app, and improve it using the knowledge and experience I gained throughout the program. That made this project feel like a strong reflection of my overall growth. In this ePortfolio I used that artifact to show development in three important areas of computer science: software design and engineering, algorithms and data structures, and databases.

## Collaborating in a Team Environment

One thing I really came to value during this program is the importance of collaboration in software development. Even though I completed this capstone on my own, I tried to approach it in a way that reflects how work gets done on a real team. I used feature branches in Git, followed a structured review process, and broke larger changes into smaller pieces. Before starting each enhancement I created detailed implementation plans that mapped out the work step by step. This helped me stay organized, but it also helped me think more like a developer working in a team setting where other people may need to understand, review, or continue the work.

The code review portion of this project especially showed me how valuable it is to slow down and evaluate software carefully. As I reviewed the original Weight Tracker project I focused on structure, naming, logic, testing, error handling, and security. That process helped me identify important problems, like plain-text password storage, a bug in the weight-saving logic, and a lack of meaningful test coverage. Finding those issues gave me a clear direction for my enhancements. It also reminded me that collaboration is not just about writing code alongside others. It is also about creating code and documentation that other people can understand, review, and improve. I also added Javadoc documentation throughout the project, because maintainability is a huge part of teamwork. Good code should not only work well, but also be readable and easy to build on later.

## Communicating with Stakeholders

Another area where I saw a lot of growth during the program was communication. In computer science it is not enough to just build something that works. You also have to be able to explain what you built, why you built it that way, and how it solves a problem. For this capstone I created a code review video that walked through the original artifact, explained its current functionality, pointed out its weaknesses, and described the improvements I planned to make. That experience pushed me to communicate technical ideas in a clear way that could make sense to different audiences, including people who may not be deeply technical.

The written implementation plans and enhancement narratives also helped me practice this skill. I had to clearly explain my design choices, the purpose of each enhancement, and how those changes connected back to the course outcomes. Writing those reflections made me think more about the reasons behind my technical decisions. Overall, this program helped me become more confident in explaining my work, which is an important skill when working with teammates, managers, clients, or other stakeholders.

## Bringing the Portfolio Together

What I like most about this ePortfolio is that it shows more than just finished code. It shows the process behind the work, the thinking that went into the changes, and the growth that happened along the way. Each enhancement builds on the same original application, but each one highlights a different area of computer science. Together they show how I can take an existing project, evaluate it critically, and improve it in a thoughtful and professional way. I feel like this portfolio represents both where I started and how far I have come during the program, and it gives a much clearer picture of the skills I am taking with me moving forward.

## Software Design and Engineering

The Software Design and Engineering enhancement demonstrated my ability to apply modern architectural patterns to an existing codebase. I refactored the application from a tightly coupled Activity-based MVC pattern to the Model-View-ViewModel (MVVM) architecture using Android's `ViewModel` and `LiveData` components. This involved creating a Repository layer (`WeightRepository` and `UserRepository`) that decouples the data source from the UI, making the codebase testable, maintainable, and resilient to Android lifecycle events like screen rotation. The refactor also added Edit Weight and User Profile features, and fixed a goal-notification SMS bug that had 'failed to save' shown to the user after they successfully added the weight where they hit their goal.

## Data Structures and Algorithms

My Algorithms and Data Structures enhancement transformed the Weight Tracker's raw data display into an analytics dashboard with meaningful insights. I implemented several distinct algorithms in a `WeightAnalytics` utility class:

- A **sliding-window moving average** that smooths daily weight fluctuations using a running sum — achieving O(n) time complexity and O(1) space per window computation, compared to the naive O(n × k) approach of recalculating each window from scratch.
- **Linear regression** (least-squares method) to compute the user's weight-loss rate in pounds per week and project a goal-achievement date based on the trend line.
- **Single-pass min/max** algorithms that find extreme values and their associated dates in one traversal of the data.
- A **streak counter** that detects consecutive days of logged entries using date arithmetic, with both current streak and longest streak variants.
- Implementation of `Comparable<WeightEntry>` with proper `equals()` and `hashCode()` contracts to support sorted collections and binary search operations.

Each algorithm was validated with comprehensive unit tests covering edge cases: null inputs, empty lists, single entries, fewer entries than the sliding-window size, flat data, and increasing/decreasing trends. This testing discipline ensures the analytics functions handle real-world data gracefully rather than crashing on unexpected input.

## Database
The Database enhancement pushed the data layer well beyond basic CRUD operations. I implemented incremental schema migrations that preserve user data across updates — replacing a destructive drop-and-recreate strategy that would have erased all user data on every version bump. I added FTS5 full-text search with synchronized triggers for searching weight entry notes, compound indexes for efficient date-range queries, and SQL aggregation queries for weekly and monthly averages. A CSV export/import system with transactional bulk operations provides data portability, using Android's Storage Access Framework for secure file access.

To guarantee consistent SQLite behavior across Android devices, I migrated from the framework's built-in `SQLiteOpenHelper` to AndroidX's `BundledSQLiteDriver`, ensuring FTS5 is always available regardless of the device's native SQLite version. I also added foreign key constraints with `ON DELETE CASCADE` and a proper table-recreation migration to enforce referential integrity so that weight entries cannot become orphaned if a user is deleted.

## Security

Developing a security mindset has been one of the most important outcomes of my education. During my code review, I identified several security vulnerabilities in the original artifact, most critically the storage of passwords in plain text. For the Database enhancement, I implemented bcrypt password hashing. The implementation includes:

- A secure migration that hashes existing plain-text passwords within a database transaction, ensuring either all passwords are hashed or none are.
- A legacy-fallback mechanism that handles partially migrated databases and upgrades passwords on successful login, so no user is locked out.
- Proper error handling for malformed hash strings to prevent application crashes.

I also added foreign key constraints (`FOREIGN KEY (userId) REFERENCES users(id) ON DELETE CASCADE`) with proper schema migration, ensuring referential integrity so that weight entries cannot become orphaned if a user is deleted. The FTS5 search implementation uses safe query escaping to prevent injection through search terms, and all new database methods throughout every enhancement use parameterized queries.

## Portfolio Summary

The three enhancements presented in this portfolio work together to tell a cohesive story of professional growth. [Enhancement One]({{ '/enhancement-one' | relative_url }}) *(Software Design and Engineering)* established a professional architectural foundation with MVVM, providing the structural quality needed for a maintainable codebase. [Enhancement Two]({{ '/enhancement-two' | relative_url }}) *(Algorithms and Data Structures)* built on that foundation by adding analytical capabilities that transform raw data into actionable user insights, demonstrating algorithmic problem-solving with attention to computational complexity. [Enhancement Three]({{ '/enhancement-three' | relative_url }}) *(Databases)* hardened the data layer with advanced SQLite features, data portability, and security — addressing the critical vulnerabilities identified during the [code review]({{ '/code-review' | relative_url }}).

These enhancements transformed a basic student CRUD application into a high-quality Android application. The artifact demonstrates competency across the full spectrum of computer science fundamentals: architecture and design patterns, algorithmic analysis, database engineering, and security. Each enhancement was developed through a process of planning, implementation, review, and iteration — just like the software development lifecycle used in industry.
