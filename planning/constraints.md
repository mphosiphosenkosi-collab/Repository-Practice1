# Project Constraints Document

**Project:** Conference Room Booking System  
**Document Type:** Constraints Specification  
**Version:** 1.1  
**Created:** 2026/01/15  
**Last Updated:** 2026/01/15  
**Author:** Siphosenkosi  
**Approvers:** Romio (Scrum Master), Zanke (Product Owner)  
**Status:** Approved

---

## Table of Contents

1. [Document Purpose](#1-document-purpose)
2. [Technical Constraints](#2-technical-constraints)
   2.1 [Development Environment](#21-development-environment)
   2.2 [Architecture Constraints](#22-architecture-constraints)
   2.3 [Integration Constraints](#23-integration-constraints)
3. [Business Constraints](#3-business-constraints)
   3.1 [Timeline & Budget](#31-timeline--budget)
   3.2 [Resource Constraints](#32-resource-constraints)
   3.3 [Compliance Requirements](#33-compliance-requirements)
4. [Operational Constraints](#4-operational-constraints)
   4.1 [Performance Requirements](#41-performance-requirements)
   4.2 [Scalability Limits](#42-scalability-limits)
   4.3 [Maintenance Constraints](#43-maintenance-constraints)
5. [Quality Constraints](#5-quality-constraints)
   5.1 [Security Requirements](#51-security-requirements)
   5.2 [Accessibility Standards](#52-accessibility-standards)
   5.3 [Documentation Requirements](#53-documentation-requirements)
6. [Risk Mitigation](#6-risk-mitigation)
   6.1 [Identified Constraints](#61-identified-constraints)
   6.2 [Mitigation Strategies](#62-mitigation-strategies)
7. [Change Control](#7-change-control)

---

## 1. Document Purpose

This document defines the constraints, limitations, and boundaries within which the Conference Room Booking System must be developed and operated. All project decisions must align with these constraints.

## 2. Technical Constraints

### 2.1 Development Environment

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Technology Stack** | Django (Python) backend, PostgreSQL database, JavaScript/HTML/CSS frontend | Team expertise, organizational standards | Limits technology choices but ensures maintainability |
| **Development Tools** | Git for version control, JIRA for project tracking, Confluence for documentation | BitCube organizational standards | Consistent workflow across teams |
| **Browser Support** | Chrome 90+, Firefox 85+, Safari 14+ (last 2 versions) | User base analysis, testing capacity | Reduces cross-browser testing complexity |
| **Mobile Responsiveness** | Desktop-first, tablet responsive, mobile basic | Primary users are office employees on desktops | Optimizes development effort for primary use case |

### 2.2 Architecture Constraints

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Database** | PostgreSQL 13+ required, NoSQL not permitted | Data integrity requirements, existing infrastructure | Ensures ACID compliance for booking transactions |
| **API Design** | RESTful API with JSON serialization, GraphQL not allowed | Integration with existing systems, team expertise | Simplifies integration but limits flexibility |
| **Authentication** | JWT tokens with Django authentication backend | Security standards, existing user management | Requires token management implementation |
| **Deployment** | Docker containerization required, Kubernetes optional | DevOps standards, scalability requirements | Increases initial setup complexity |

### 2.3 Integration Constraints

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Calendar Systems** | Must support iCal export for external calendar integration | User requirement for personal calendar sync | Requires iCal format implementation |
| **Email Notifications** | SMTP integration required, third-party services optional | Internal email infrastructure | Limits notification delivery options |
| **Corporate Directory** | Read-only LDAP/Active Directory integration for user data | Single source of truth for employee information | Requires directory service integration |

## 3. Business Constraints

### 3.1 Timeline & Budget

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Project Duration** | 3 months total (12 sprints of 1 week each) | Business need for Q1 implementation | Limits feature scope, requires prioritization |
| **Budget** | Fixed budget of R500,000 for development and testing | Financial planning constraints | Requires careful resource allocation |
| **Team Size** | Maximum 4 developers, 1 Product Owner, 1 Scrum Master | Resource availability | Limits parallel work streams |

### 3.2 Resource Constraints

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Development Team** | 2 full-time developers, 2 part-time (including learning time) | Organizational staffing | Extends development timeline for complex features |
| **Testing Resources** | Developers responsible for unit/integration testing, separate QA for UAT | Resource optimization | Increases developer responsibility for quality |
| **Infrastructure** | Shared development/staging environment, dedicated production | Cost optimization | Requires careful environment management |

### 3.3 Compliance Requirements

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Data Privacy** | POPIA compliance required for South African user data | Legal requirement | Adds data protection implementation requirements |
| **Audit Trail** | All booking changes must be logged for 12 months | Corporate governance | Increases database storage requirements |
| **Access Control** | Role-based access control (Employee, Admin, Facilities) | Security standards | Requires permission system implementation |

## 4. Operational Constraints

### 4.1 Performance Requirements

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Response Time** | Page load < 3 seconds, API response < 500ms | User experience standards | Requires performance optimization |
| **Concurrent Users** | Support 200 concurrent users during peak hours (9-11 AM) | Organizational size | Requires load testing and optimization |
| **Database Performance** | Booking queries must complete within 100ms | User experience for frequent operations | Requires database indexing and optimization |

### 4.2 Scalability Limits

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Room Capacity** | Support up to 50 conference rooms initially | Current office infrastructure | Limits system complexity |
| **User Base** | Support 1000 employees with potential 20% annual growth | Organizational growth projections | Requires scalable architecture |
| **Booking Volume** | Support 5000 monthly bookings with peak of 50 simultaneous | Historical data analysis | Informs database design decisions |

### 4.3 Maintenance Constraints

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Uptime** | 99.5% availability during business hours (7 AM - 7 PM) | Business continuity requirements | Requires monitoring and redundancy planning |
| **Support Hours** | IT support available 8 AM - 5 PM weekdays only | Resource constraints | Limits immediate issue resolution |
| **Update Windows** | System updates only on Sundays 2 AM - 6 AM | Minimal business impact | Requires scheduled maintenance procedures |

## 5. Quality Constraints

### 5.1 Security Requirements

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Authentication** | Multi-factor authentication for admin users | Security standards | Increases login complexity for admins |
| **Data Encryption** | TLS 1.2+ for all communications, encryption at rest | Data protection requirements | Requires certificate management |
| **Input Validation** | All user inputs must be validated server-side | Security best practices | Increases development time for forms |

### 5.2 Accessibility Standards

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **WCAG Compliance** | WCAG 2.1 AA level compliance required | Legal and inclusivity requirements | Increases UI development complexity |
| **Screen Reader** | Support for JAWS and NVDA screen readers | Accessibility standards | Requires semantic HTML and ARIA labels |
| **Keyboard Navigation** | Full functionality via keyboard only | Accessibility requirements | Requires focus management implementation |

### 5.3 Documentation Requirements

| Constraint Category | Specific Constraint | Rationale | Impact |
|-------------------|-------------------|-----------|--------|
| **Code Documentation** | 80% code coverage minimum, all public APIs documented | Maintainability standards | Increases development time |
| **User Documentation** | Complete user guide and admin manual required | User support requirements | Requires technical writing effort |
| **API Documentation** | OpenAPI/Swagger specification for all endpoints | Integration requirements | Requires ongoing documentation maintenance |

## 6. Risk Mitigation

### 6.1 Identified Constraints

| Risk Area | Constraint Impact | Probability | Severity |
|-----------|------------------|-------------|----------|
| **Team Experience** | Limited Django/PostgreSQL experience among junior developers | High | Medium |
| **Integration Complexity** | LDAP integration unknown territory for team | Medium | High |
| **Performance Requirements** | Response time targets aggressive for complex queries | Medium | Medium |
| **Timeline Pressure** | Fixed deadline with learning curve consideration | High | High |

### 6.2 Mitigation Strategies

1. **Training Allocation:** Dedicate 20% of sprint time for skill development
2. **Proof of Concepts:** Build integration prototypes early in project
3. **Performance Testing:** Implement from Sprint 1, not as afterthought
4. **Scope Management:** Rigorous prioritization, MVP focus initially

## 7. Change Control

All changes to these constraints must follow the formal change control process:

1. Submit change request via JIRA with impact analysis
2. Review by Scrum Master and Product Owner
3. Approval required from project sponsor for significant changes
4. Document all approved changes in revision history
5. Communicate changes to all stakeholders

---

## Revision History

| Version | Date | Author | Changes | Approver |
|---------|------|--------|---------|----------|
| 1.0 | 2026/01/10 | Zanke | Initial constraints document | Romio |
| 1.1 | 2026/01/15 | Siphosenkosi | Updated based on Sprint 1 learnings, added technical specifics | Romio |