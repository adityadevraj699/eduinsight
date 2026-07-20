# 01 — Problem Statement

## Project
**EduInsight V2 — AI Powered Feedback Intelligence Platform**

## Background
Colleges collect student feedback about faculty and courses, but almost every
existing system stops at **data collection**. A student fills a form, the
data lands in a spreadsheet or a dashboard full of stars/ratings, and nobody
acts on it.

## The Problem
1. Feedback forms are collected but **never analyzed deeply**.
2. Faculty receive a numeric average (e.g. "4.2/5") with **no actionable
   insight** — they don't know *what* to improve or *why*.
3. HODs/Admins have **no aggregated view** across departments or semesters
   to spot trends (e.g. a subject consistently rated low across 3 years).
4. Sentiment hidden in free-text comments is **completely ignored** — most
   systems only compute averages of numeric ratings.
5. No **notification loop** — faculty don't know feedback exists until
   someone manually shares it.

## Why Existing Tools Fall Short

| Tool | What it does | What it lacks |
|---|---|---|
| Google Forms | Collects responses | No analysis, no roles, no AI |
| Microsoft Forms | Collects + basic charts | No sentiment analysis, no RBAC |
| Moodle Feedback | Course-level feedback | No AI insight, dated UI/UX |
| Canvas LMS | Feedback + LMS integration | Heavy, not feedback-focused, no AI reports |

**Common gap across all of them:** raw data in, raw data out. No
intelligence layer that converts feedback → insight → action.

## Solution
**EduInsight V2** is an AI-powered platform that:
- Collects structured + free-text feedback.
- Runs **AI sentiment & theme analysis** on open-ended responses.
- Generates **actionable reports** for Faculty, HOD, and Admin — not just
  averages, but *"students are consistently unhappy with pacing in Week 3
  labs"* type insights.
- Notifies stakeholders automatically when new insights are ready.

## Goal of V2
Move from a **form-filling tool** to a **feedback intelligence platform**
that closes the loop between "student said something" and "faculty/admin
did something about it."

## Success Criteria
- Faculty can view AI-generated summary + sentiment trend per subject.
- HOD can view department-wide comparative report.
- System scales to multiple universities (multi-tenant ready in design,
  even if V1 launch is single-tenant).
