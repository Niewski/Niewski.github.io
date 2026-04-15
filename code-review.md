---
layout: default
title: Code Review
permalink: /code-review
---

Before beginning any enhancements, I conducted a comprehensive code review of the original CS360 Weight Tracker artifact. This review used a structured checklist to evaluate the codebase across multiple dimensions: structure and organization, naming conventions, error handling, security, testing, and efficiency.

## Code Review Video

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YV2uX4OUh6Q" title="CS499 Code Review — CS360 Weight Tracker" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## Artifact Overview

The CS360 Weight Tracker is an Android application built in Java using Android SDK 36, SQLite, and Material Components. It allows users to register an account, log daily weights, view their history, delete entries, and receive a one-time SMS notification when they reach their goal weight.

<div class="artifact-links">
  <a href="https://github.com/Niewski/CS360WeightTracker">&#128193; Original Source Code</a>
</div>

## Key Findings

### Structure and Organization
The project had a reasonable package structure with `data/` for database access and `features/` split into `login/` and `weight/` packages. However, all business logic was tightly coupled to the Activity lifecycle. Goal-check logic, SMS decision-making, and database calls were all embedded directly in button click listeners, violating separation of concerns and making the code difficult to test or reuse.

### Security — Critical
**Passwords were stored in plain text** in the SQLite database. The `createUser()` method inserted the raw password string directly into the `password` column. The authentication method compared raw passwords via SQL query. While parameterized queries protected against SQL injection, the lack of password hashing meant credential theft was trivial if the device or database file were compromised.

### Testing — Critical
The test suite was effectively nonexistent. Only two placeholder tests existed: one asserting `2 + 2 == 4` and one checking the application package name. Zero tests covered any actual business logic, database operations, or UI flows — meaning there was no safety net for the refactoring work ahead.

### Error Handling
A control-flow bug existed in `AddWeightActivity`'s save logic where a missing `return` statement after the successful-insert path could cause both success and failure Toasts to display. Input validation was minimal with no password strength requirements and no phone number format validation.

### Efficiency
Every data change triggered a full list reload — clearing the entire `ArrayList`, re-querying all entries from the database, and calling `notifyDataSetChanged()`. No indexed queries existed beyond implicit primary key indexes, and no data aggregation occurred in SQL.

### Database Design
The `weights.userId` column referenced `users.id` but had no `FOREIGN KEY` constraint declared, allowing orphaned data. The `onUpgrade` method used a destructive drop-and-recreate strategy that would erase all user data on any schema change. There were no `UPDATE` operations for weight entries or user profile data.

## How Findings Informed Enhancements

| Finding | Enhancement |
|---------|-------------|
| Tight Activity coupling, no separation of concerns | [Enhancement 1: MVVM Refactor]({{ '/enhancement-one' | relative_url }}) |
| No data analysis or algorithmic computation | [Enhancement 2: Analytics Dashboard]({{ '/enhancement-two' | relative_url }}) |
| Plain-text passwords, destructive migrations, no FK constraints | [Enhancement 3: Database Upgrades]({{ '/enhancement-three' | relative_url }}) |
| Missing `return` in save logic, SMS bug | Fixed in Enhancement 1 |
| No test coverage | Unit tests added in Enhancements 2 and 3 |
