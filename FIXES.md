# HAQMS Engineering Evaluation — Fixes & Improvements

## Overview

This project involved identifying and resolving multiple intentionally introduced issues in the HAQMS (Hospital Appointment and Queue Management System) application.

The work focused on:

* Security hardening
* Performance optimization
* Database efficiency
* Frontend stability
* Concurrency handling
* UX improvements
* Production deployment readiness

---

# Issues Identified & Fixes Implemented

## 1. Authentication & Security Issues

### Problems

* JWT secret fallback hardcoded in source code
* JWT expiration validation bypassed
* Sensitive password logging in authentication routes
* Password hashes returned in API responses
* Admin authorization middleware bypass
* Detailed internal errors leaked to clients

### Fixes

* Removed insecure JWT fallback secret
* Enforced proper JWT expiration verification
* Removed sensitive console logs
* Sanitized authentication responses
* Implemented proper admin authorization checks
* Reduced sensitive error leakage

---

## 2. Queue System Concurrency Issue

### Problem

Queue token generation suffered from race conditions under concurrent requests, causing duplicate token numbers.

### Fix

* Reworked queue check-in flow using Prisma transactions
* Ensured atomic token generation
* Removed unsafe sequential token assignment logic

---

## 3. Database Performance Problems

### Problems

* N+1 query issue in appointments endpoint
* Sequential aggregation queries in doctor statistics
* In-memory pagination/filtering for patients

### Fixes

* Replaced N+1 queries with Prisma relation includes
* Parallelized independent database aggregations using `Promise.all`
* Implemented database-level pagination and filtering

---

## 4. Frontend Stability Issues

### Problems

* Memory leak caused by uncleared polling intervals
* State updates after component unmount
* Crash when patient medical history was null
* Missing Next.js `Link` import
* Missing React dependency warnings

### Fixes

* Added proper interval cleanup
* Prevented async state updates after unmount
* Added safe optional chaining and fallback rendering
* Added missing imports
* Corrected dependency arrays

---

## 5. Frontend Performance Improvements

### Problems

* API requests triggered on every keystroke
* Unnecessary dashboard rerenders
* Duplicate API calls

### Fixes

* Added debounced patient search
* Optimized effect dependencies
* Reduced redundant network calls

---

## 6. UX & Validation Improvements

### Problems

* Invalid phone numbers accepted
* Invalid ages accepted
* Duplicate form submissions possible

### Fixes

* Added frontend validation for phone and age
* Added loading states and disabled buttons
* Prevented duplicate patient registrations
* Prevented duplicate appointment bookings
* Prevented duplicate queue token generation

---

# Deployment

## Frontend

* Deployed on Vercel

## Backend

* Deployed on Render

## Database

* PostgreSQL hosted on Render

---

# Remaining Known Issues / Future Improvements

The following improvements can still be implemented in future iterations:

* More granular backend role-based authorization
* Additional report endpoint optimizations
* Better frontend notification system
* Missing patient history records route
* Production logging system
* Rate limiting and API throttling

---

# Engineering Approach

The primary goal was prioritizing:

1. Security vulnerabilities
2. Concurrency/data corruption issues
3. Performance bottlenecks
4. Frontend crashes
5. UX improvements

The fixes focused on maintainability, scalability, and production-readiness while minimizing unnecessary architectural changes.
