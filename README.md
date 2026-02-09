# Smart Study Planner

A modern productivity web application built using **HTML, CSS, and Vanilla JavaScript** that helps students manage subjects, organize schedules, track tasks, and analyze performance — all powered by browser LocalStorage.

#Live Demo:  
https://your-username.github.io/smart-study-planner/


## Overview

Smart Study Planner is a fully frontend-based study management system designed to demonstrate strong JavaScript fundamentals, structured state handling, and intelligent scheduling logic.

Unlike basic CRUD apps, this project includes conflict detection, real-time analytics, time calculations, and persistent state management — all implemented without any frameworks or libraries.


## Key Features

### Subject Management
- Add and delete subjects
- Priority assignment (High / Medium / Low)
- Automatic removal of related tasks when subject is deleted

### Task Manager
- Add tasks with deadlines
- Mark tasks as completed
- Overdue detection
- Filter (All / Pending / Completed)
- Search functionality
- Subject-based task mapping

### Schedule Planner
- Add weekly time slots
- Conflict detection algorithm to prevent overlaps
- Daily total study hours calculation
- Weekly study hour aggregation
- Delete schedule slots dynamically

### Analytics Dashboard
- Overall task completion percentage
- Subject-wise performance breakdown
- Weekly study hours calculation
- Overdue task count
- Most productive subject detection
- Productivity score (0–100 weighted calculation)

### Settings
- Apple-style Dark Mode
- Export planner data as JSON
- Reset all data
- Theme persistence using LocalStorage


## Technical Highlights

### Centralized State Management

```javascript
let state = {
  subjects: [],
  tasks: [],
  schedules: []
};
