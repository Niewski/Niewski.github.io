---
layout: default
title: "Enhancement Three: Databases"
permalink: /enhancement-three
---

## Overview

The third enhancement significantly expanded the database layer with advanced SQLite features, security hardening, and data portability. This enhancement addresses the critical vulnerabilities and design flaws identified during the code review while demonstrating mastery of database design, migration strategies, and security best practices.

<div class="artifact-links">
  <a href="https://github.com/Niewski/CS360WeightTracker/tree/76e659e5cc046e28d372e604099ec7a84ce89eb1">&#128193; Original Code</a>
  <a href="https://github.com/Niewski/CS360WeightTracker/tree/database-updates">&#128640; Enhanced Code</a>
</div>

## What Changed

### Schema Evolution and Migration
- **Incremental migrations** replaced the destructive drop-and-recreate `onUpgrade` strategy. The database evolved from version 3 through version 8, with each migration preserving existing user data. Migrations include `ALTER TABLE` additions, FTS5 virtual table creation, password hashing, and a full table-recreation for adding foreign key constraints.
- **Version-range checks** ensure that upgrading from any previous version applies only the necessary migrations in sequence.

### FTS5 Full-Text Search
- Added a `notes` column to weight entries, allowing users to annotate each daily weigh-in
- Created an FTS5 virtual table (`weights_fts`) with synchronized triggers on INSERT, UPDATE, and DELETE
- Search uses FTS5's `MATCH` syntax with proper query escaping, far more capable and secure than `LIKE '%query%'`
- Migrated to `BundledSQLiteDriver` (AndroidX) to guarantee FTS5 availability across all Android devices, removing the need for LIKE-based fallback

### SQL Aggregation and Indexing
- Weekly and monthly average queries computed entirely in SQL using `strftime()` and `GROUP BY`
- Compound index on `(userId, date)` for efficient date-filtered lookups
- Date-range query methods for filtered data retrieval

### CSV Data Export/Import
- `DataExporter` writes weight entries (date, weight, notes) to CSV format via `OutputStream`
- `DataImporter` parses and validates CSV data, performing bulk inserts within a single database transaction for atomicity
- Integrated with Android's Storage Access Framework (`ACTION_CREATE_DOCUMENT` / `ACTION_OPEN_DOCUMENT`) for secure file access

### Password Hashing (Security)
- Replaced plain-text password storage with **bcrypt hashing** (cost factor 12) following OWASP guidelines
- Migration hashes all existing passwords within a database transaction
- Legacy fallback: if a plain-text password is detected during login (from a partial migration), it is verified and upgraded to bcrypt on the spot
- Character arrays cleared after hashing to minimize sensitive data lifetime in memory

### Foreign Key Constraints (Data Integrity)
- Added `FOREIGN KEY (userId) REFERENCES users(id) ON DELETE CASCADE` to the weights table
- `PRAGMA foreign_keys = ON` set on every database connection before any operations
- Migration uses the standard SQLite table-recreation pattern: create new table → copy data (skipping orphaned rows via `INNER JOIN`) → drop old → rename

## Skills Demonstrated
- Advanced SQLite: FTS5, triggers, compound indexes, aggregation, PRAGMA directives
- Incremental database migration strategies
- Security: bcrypt password hashing, parameterized queries, input validation, OWASP compliance
- Data integrity: foreign key constraints, transactional operations
- Data portability: CSV export/import with validation
- Platform engineering: BundledSQLiteDriver for consistent cross-device behavior

---

## Narrative

<p>For my database enhancement I chose the Weight Tracker app that I originally created in CS 360: Mobile Architecture and Programming in August of 2025. This artifact is an Android app built in Java with SQLite and Material Components. It lets users create an account, log their daily weight, view their weight history, delete entries, and receive a one-time SMS notification when they reach their goal weight. When I first made the app, it was functional, but the database side was still pretty basic. It used a simple Activity-based MVC structure, stored passwords in plain text, only had two tables, and didn’t have much testing. Because of that I felt like it had a lot of room for improvement, and would be a good artifact to show how much my skills have grown.</p>
<p>I picked this artifact because it gave me the chance to make improvements that were actually meaningful, instead of just cleaning up small issues. I updated the schema from version 3 to version 8 through a series of migrations, added compound indexes to improve query performance, and implemented FTS5 full-text search for weight notes. I also changed password storage from plain text to bcrypt hashing, which made the app much more secure. Another major change was replacing SQLiteOpenHelper with BundledSQLiteDriver so FTS5 support would be available no matter what device the app runs on. I also added foreign key constraints with cascade delete, transactional CSV export and import, and SQL aggregation queries for weekly and monthly averages. Overall these changes made the app feel much more like a real-world application instead of just a basic student project.</p>
<p>This enhancement lined up especially well with Outcome 4 and Outcome 5. It showed that I can use solid, well-established database practices like parameterized queries, indexing, transaction handling, and secure password hashing. It also showed growth in my security mindset because I addressed actual weaknesses in the original app, especially the plain-text password issue and the lack of referential integrity in the database. I think this enhancement does a good job of showing that I can take something simple and improve it in a way that adds real value and better reflects industry practices.</p>
<p>One of the biggest things I learned from this process is that database migrations take a lot of planning. Adding FTS wound up being the most challenging part, and the largest time sink. I didn’t realize that I would need to use a different driver until after I had implemented a lot of features and tests, with fallbacks to handle my emulator not having FTS, which all needed to be refactored. The foreign key migration was also very challenging because I had to preserve earlier changes like the notes column, the FTS tables and triggers, and the password hashes. Overall this enhancement helped me better understand database design, migrations, security, and testing, and it made this artifact much stronger than it was originally.</p>
