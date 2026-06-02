# Task Rules and Data Model

## Goal

This document defines the core task rules for the MVP. It describes task fields, deadline handling, status changes, archive behavior, and sorting rules.

## Task Fields

### Required Fields

- `id`
  - Unique task identifier
- `title`
  - Main task title
- `status`
  - Current task status (e.g. success or failed if deadline is overdue)
- `createdAt`
  - Date and time when the task was created
- `startedAt`
  - Date and time used as the start point for deadline progress

### Optional Fields

- `description`
  - Additional task details
- `deadlineDate`
  - Optional deadline date
- `deadlineTime`
  - Optional deadline time
- `completedAt`
  - Date and time when the task was completed
- `deadlineAt`
  - Combined deadline date and time

## Status Rules

### Open Tasks

- New tasks start as `open`
- Open tasks are shown in the open task list
- Open tasks can be edited
- Open tasks can be marked as completed
- Open tasks can be deleted

### Completed Tasks

- Completed tasks are shown in the archive
- Completed tasks cannot be edited directly
- Completed tasks can be reopened
- Completed tasks can be deleted

## Completing a Task

When a task is completed:

- `status` changes from `open` to `completed`
- `completedAt` is set
- The task is removed from the open task list
- The task appears in the archive

## Reopening a Task

When a completed task is reopened:

- `status` changes from `completed` to `open`
- `completedAt` is reset
- The task is removed from the archive
- The task appears in the open task list again
- `startedAt` remains unchanged

## Deadline Rules

### Without Deadline

- A task can exist without a deadline
- Tasks without a deadline do not show deadline progress - It might still be possible to find a graphical solution here.
- Tasks without a deadline are sorted after tasks with a deadline

### Date Without Time

- If only a deadline date is set, the deadline time defaults to the end of the selected day
- The resulting `deadlineAt` is the selected date at `23:59`

### Date With Time

- If deadline date and deadline time are set, both values are combined into `deadlineAt`
- `deadlineAt` is used for sorting, remaining time, and progress calculation

### Progress

- Deadline progress is calculated between `startedAt` and `deadlineAt`
- If the deadline is in the future, the remaining time is shown
- If the deadline is exceeded, the task is overdue
- Overdue tasks keep the progress bar at 100% and the bar change the color to red

## Sorting Rules

### Open Task List

Open tasks are sorted as follows:

- Overdue tasks first
- Then tasks with the nearest upcoming deadline
- Tasks without a deadline last

### Archive

Completed tasks are sorted as follows:

- Recently completed tasks first
- Older completed tasks later