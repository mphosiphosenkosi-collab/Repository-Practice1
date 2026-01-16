# Project Prioritization Matrix

**Project:** Conference Room Booking System  
**Document Type:** Strategic Planning & Prioritization  
**Version:** 2.0  
**Created:** 2026/01/10  
**Last Updated:** 2026/01/15  
**Author:** Siphosenkosi  
**Contributors:** Zanke (Product Owner), Romio (Scrum Master), Wendy (Developer)  
**Approved By:** Zanke (Product Owner)  
**Status:** Active for Sprint 2 Planning

---

## Table of Contents

1. [Document Purpose](#1-document-purpose)
   1.1 [Prioritization Framework](#11-prioritization-framework)
   1.2 [Scoring Methodology](#12-scoring-methodology)
2. [Prioritization Matrix](#2-prioritization-matrix)
   2.1 [Critical Path Stories](#21-critical-path-stories-must-have-mvp)
   2.2 [Core Enhancement Stories](#22-core-enhancement-stories-high-value-additions)
   2.3 [Administrative Features](#23-administrative-features-operational-requirements)
   2.4 [Specialized Functionality](#24-specialized-functionality-extended-use-cases)
3. [Strategic Analysis](#3-strategic-analysis)
   3.1 [Risk-Adjusted Value Analysis](#31-risk-adjusted-value-analysis)
   3.2 [Dependency Considerations](#32-dependency-considerations)
   3.3 [Resource Allocation](#33-resource-allocation)
4. [Implementation Roadmap](#implementation-roadmap)
   4.1 [Sprint Allocation](#sprint-allocation)
   4.2 [Release Planning](#release-planning)
5. [Appendix](#appendix)
   5.1 [Prioritization Criteria](#prioritization-criteria)
   5.2 [Change Log](#change-log)

---

## 1. Document Purpose

### 1.1 Prioritization Framework

This matrix applies the **Weighted Shortest Job First (WSJF)** methodology adapted for agile development:

- **Business Value:** Strategic importance to stakeholders (1-5 scale)
- **Technical Complexity:** Implementation effort and risk (1-5 scale)
- **User Impact:** Number of users affected and frequency of use (1-5 scale)
- **Dependencies:** Pre-requisite stories and integration requirements

### 1.2 Scoring Methodology

| Score | Business Value | Technical Complexity | User Impact |
|-------|----------------|----------------------|-------------|
| 5     | Critical to MVP | Very High (Novel/Risky) | All Users Daily |
| 4     | High Value | High (Complex Integration) | Most Users Weekly |
| 3     | Medium Value | Medium (Standard Implementation) | Many Users Monthly |
| 2     | Low Value | Low (Simple Enhancement) | Some Users Occasionally |
| 1     | Nice-to-Have | Very Low (Minor Change) | Few Users Rarely |

Priority Score = (Business Value + User Impact) ÷ Technical Complexity

---

## 2. Prioritization Matrix

### 2.1 Critical Path Stories (Must-Have MVP)

| ID | Epic | Title | Business Value | Tech Complexity | User Impact | Priority Score | Sprint | Dependencies | Risk |
|----|------|-------|----------------|-----------------|-------------|----------------|--------|--------------|------|
| **US-101** | EPIC-100 | View Available Conference Rooms | 5 | 2 | 5 | **5.0** | 1 | None | Low |
| **US-201** | EPIC-200 | Basic Room Booking | 5 | 3 | 5 | **3.3** | 1 | US-101 | Medium |
| **US-202** | EPIC-200 | Booking Cancellation | 4 | 2 | 4 | **4.0** | 1 | US-201 | Low |
| **US-102** | EPIC-100 | Filter Rooms by Capacity | 4 | 2 | 4 | **4.0** | 1 | US-101 | Low |
| **US-103** | EPIC-100 | Filter Rooms by Equipment | 4 | 2 | 4 | **4.0** | 1 | US-101 | Low |

### 2.2 Core Enhancement Stories (High Value Additions)

| ID | Epic | Title | Business Value | Tech Complexity | User Impact | Priority Score | Sprint | Dependencies | Risk |
|----|------|-------|----------------|-----------------|-------------|----------------|--------|--------------|------|
| **US-203** | EPIC-200 | View My Bookings | 3 | 2 | 4 | **3.5** | 2 | US-201 | Low |
| **US-301** | EPIC-300 | Set Up Recurring Meeting | 4 | 3 | 4 | **2.7** | 2 | US-201 | Medium |
| **US-401** | EPIC-400 | Admin Dashboard Viewing | 4 | 3 | 3 | **2.3** | 2 | US-201 | Medium |
| **US-501** | EPIC-500 | Room Maintenance Scheduling | 4 | 3 | 3 | **2.3** | 2 | US-201 | Medium |

### 2.3 Administrative Features (Operational Requirements)

| ID | Epic | Title | Business Value | Tech Complexity | User Impact | Priority Score | Sprint | Dependencies | Risk |
|----|------|-------|----------------|-----------------|-------------|----------------|--------|--------------|------|
| **US-402** | EPIC-400 | Booking Conflict Resolution | 4 | 4 | 3 | **1.8** | 2 | US-201, US-401 | High |
| **US-403** | EPIC-400 | Usage Reports Generation | 3 | 3 | 2 | **1.7** | 2 | US-201, US-401 | Medium |
| **US-302** | EPIC-300 | Edit Recurring Meeting Series | 3 | 4 | 3 | **1.5** | 3 | US-301 | High |
| **US-303** | EPIC-300 | Cancel Recurring Meeting | 3 | 3 | 3 | **2.0** | 3 | US-301 | Medium |

### 2.4 Specialized Functionality (Extended Use Cases)

| ID | Epic | Title | Business Value | Tech Complexity | User Impact | Priority Score | Sprint | Dependencies | Risk |
|----|------|-------|----------------|-----------------|-------------|----------------|--------|--------------|------|
| **US-601** | EPIC-600 | Visitor Booking Assistance | 3 | 3 | 2 | **1.7** | 3 | US-201 | Medium |
| **US-304** | EPIC-300 | View Recurring Meetings | 2 | 2 | 2 | **2.0** | 3 | US-301 | Low |
| **US-204** | EPIC-200 | Booking Modifications | 3 | 3 | 3 | **2.0** | Future | US-201 | Medium |

---

## 3. Strategic Analysis

### 3.1 Risk-Adjusted Value Analysis

High Priority (Score > 3.0): ████████████ 5 stories
Medium Priority (2.0-3.0): ████████ 4 stories
Low Priority (< 2.0): ████ 3 stories

text

**Key Insights:**

1. **Immediate ROI:** First 5 stories deliver 80% of user value with 40% of effort
2. **Quick Wins:** US-101, US-102, US-103 offer high value with low complexity
3. **Risk Concentration:** US-402 (Conflict Resolution) has high complexity for moderate value

### 3.2 Dependency Considerations

```mermaid
graph TD
    A[US-101: View Rooms] --> B[US-201: Basic Booking]
    A --> C[US-102: Capacity Filter]
    A --> D[US-103: Equipment Filter]
    B --> E[US-202: Cancellation]
    B --> F[US-203: View My Bookings]
    B --> G[US-301: Recurring Meetings]
    B --> H[US-401: Admin Dashboard]
    H --> I[US-402: Conflict Resolution]
    H --> J[US-403: Usage Reports]
    G --> K[US-302: Edit Recurring]
    G --> L[US-303: Cancel Recurring]

3.3 Resource Allocation

Sprint	Developer Days	Stories	Estimated Points	Focus Area
Sprint 1	40	5	15	Core Booking MVP
Sprint 2	40	5	18	Enhancements & Admin
Sprint 3	40	4	16	Advanced Features
Total	120	14	49	Complete Foundation

4. Implementation Roadmap
4.1 Sprint Allocation
Sprint 1: Foundation (Completed)
Theme: Core Booking Functionality

Stories: US-101, US-201, US-202, US-102, US-103

Business Value Delivered: 22/35 points (63%)

Status: ✅ In Progress (US-101, US-202 completed)

Sprint 2: Enhancement (Current Planning)
Theme: Advanced Features & Administration

Stories: US-203, US-301, US-401, US-501, US-402

Business Value: Additional 13/35 points (37%)

Risk Mitigation: Pair programming on US-402, early stakeholder review on US-401

Sprint 3: Specialization (Future)
Theme: Extended Use Cases & Polish

Stories: US-403, US-302, US-303, US-601

Business Value: Remaining specialized functionality

Considerations: User feedback from Sprints 1-2 may adjust priorities

4.2 Release Planning
Release	Target Date	Included Stories	Success Criteria
MVP Release	2026/02/15	US-101 to US-203	80% employee adoption
V1.1 Enhancement	2026/03/15	US-301 to US-402	Admin tools operational
V1.2 Complete	2026/04/15	All prioritized stories	Full feature set live
5. Appendix
5.1 Prioritization Criteria
Business Value Factors Considered:

Strategic Alignment: Supports organizational efficiency goals

User Demand: Frequency and importance to daily operations

Revenue Impact: Indirect through productivity gains

Competitive Advantage: Differentiates from manual processes

Compliance Requirements: Meets audit and governance needs

Technical Complexity Factors:

Implementation Effort: Estimated developer days

Integration Points: Dependencies on other systems/components

Technical Risk: Novelty and uncertainty in implementation

Testing Requirements: Validation and verification effort

Maintenance Overhead: Ongoing support requirements

5.2 Change Log
Version	Date	Author	Changes	Rationale
1.0	2026/01/10	Zanke	Initial prioritization	Sprint 1 planning
1.1	2026/01/12	Romio	Adjusted based on team capacity	Resource optimization
2.0	2026/01/15	Siphosenkosi	Restructured with WSJF scoring, added strategic analysis	Post-Sprint 1 refinement
