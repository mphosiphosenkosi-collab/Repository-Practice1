# Conference Room Booking System - User Stories (C#/.NET & React Stack)

Project: Conference Room Booking System
Document Type: Requirements Specification (Technical Stack: ASP.NET Core / React)
Version: 1.0
Created: 2026/01/16
Author: BitCube Development Team
Approved By: Technical Architecture Board
Status: Approved for Sprint 1 Development

Architecture Overview
Backend Stack: ASP.NET Core Web API (C# 11), Entity Framework Core, SQL Server, JWT Authentication
Frontend Stack: React 18, TypeScript, Material-UI, Axios, React Query
Deployment: Docker containers, Azure App Service, CI/CD via GitHub Actions

## STORY #0: BASIC ROOM BOOKING

**Task Title:** [Story #0] As a company employee, I want to view available rooms by time and date and book one so that I can secure a space for my meeting

**Custom Fields:**

- Story Points: 5
- Priority: Critical (P0)
- Sprint: Sprint 1
- Story Type: Feature
- Component: Frontend, Backend, Database, API
- Dependencies: Authentication System

**Description:**

## User Story

As a company employee, I want to view available rooms by time and date and book one, so that I can secure a space for my meeting.

## Acceptance Criteria

✓ AC-0.1: View available rooms - Backend returns 200 OK with room count, Frontend renders room cards
✓ AC-0.2: No rooms available - Backend returns empty array 200 OK, Frontend displays "No rooms available"
✓ AC-0.3: Successful booking - Backend POST returns 201 Created with booking ID, Frontend shows success toast
✓ AC-0.4: Time conflict - Backend returns 409 Conflict, Frontend displays conflict error
✓ AC-0.5: Invalid input - Backend returns 400 Bad Request, Frontend shows validation errors

## Technical Notes - BACKEND (ASP.NET Core / C#)

- API Endpoints:
  • GET /api/rooms/availability?date={date}&duration={minutes}
  • POST /api/bookings
- Controllers: RoomsController, BookingsController
- Services: RoomAvailabilityService, BookingService
- Key Patterns: Repository Pattern, Unit of Work, CQRS (MediatR), FluentValidation, AutoMapper
- Database Schema (SQL Server):
  • Rooms: Id, Name, Capacity, Floor, CreatedAt
  • Bookings: Id (GUID), RoomId, EmployeeId, StartTime, EndTime, Status, CreatedAt

## Technical Notes - FRONTEND (React / TypeScript)

- Components: <RoomBookingPage>, <DateTimePicker>, <RoomGrid>, <BookingModal>
- State Management: React Query for server state, Context API for user context
- Custom Hooks: useRoomAvailability(), useBookingForm()
- TypeScript Interfaces: Room, BookingFormState
- UI Framework: Material-UI theming and components

## Dependencies

- Authentication System (must be implemented first)
- Database setup with seed data

## Definition of Done

- All acceptance criteria met and tested
- Backend: xUnit integration tests passing, API documented in Swagger
- Frontend: React components tested with Jest, Storybook documentation
- Code reviewed, SonarQube passing with 80%+ coverage
- Deployed to development environment
- Documentation updated (API docs, component docs)

---

## STORY #1: RECURRING MEETINGS SETUP

**Task Title:** [Story #1] As an employee, I want to schedule meetings that repeat on a defined pattern so that I don't have to create each occurrence manually

**Custom Fields:**

- Story Points: 8
- Priority: High (P1)
- Sprint: Sprint 2
- Story Type: Feature
- Component: Backend, Database
- Dependencies: STORY-0

**Description:**

## User Story

As an employee, I want to schedule meetings that repeat on a defined pattern, so that I don't have to create each occurrence manually.

## Acceptance Criteria

✓ AC-1.1: Weekly pattern - Creates 12 bookings for 12-week period, Pattern builder shows correct preview
✓ AC-1.2: Monthly pattern - Creates bookings on 15th of each month, Month selector works correctly
✓ AC-1.3: Pattern exceptions - Skips specified dates in series, Exception dates show as excluded in preview
✓ AC-1.4: Conflict in series - Rolls back transaction, returns partial failure, Shows which dates failed with reasons

## Technical Notes

- Backend: POST /api/bookings/recurring endpoint with RRule pattern parsing
- Request Body: JSON with roomId, date range, recurrenceRule, exceptions
- C# Service: RecurringBookingService with bulk EF Core operations
- Key Patterns: RRule library, Transaction management, Background job scheduling (Hangfire)
- Frontend Components: <RecurrencePatternPicker>, <CalendarPreview>, <ExceptionDatePicker>

## Dependencies

- STORY-0 (Basic booking system must be complete)

## Definition of Done

- All acceptance criteria met and tested
- Recurrence logic handles all standard patterns (weekly, bi-weekly, monthly)
- Transaction rollback works correctly for partial failures
- Frontend provides intuitive pattern builder with visual preview
- Performance optimized for large recurring series

---

## STORY #2: ROOM CAPACITY FILTERING

**Task Title:** [Story #2] As an employee, I want to filter rooms by seating capacity so I can find rooms that fit all participants

**Custom Fields:**

- Story Points: 3
- Priority: High (P1)
- Sprint: Sprint 1
- Story Type: Feature
- Component: Frontend, Backend
- Dependencies: STORY-0

**Description:**

## User Story

As an employee, I want to filter rooms by seating capacity, so I can find rooms that fit all participants.

## Acceptance Criteria
✓ Given I set a minimum capacity filter, When I view available rooms, Then only rooms with sufficient capacity are shown
✓ Given I adjust the capacity slider, When the filter updates, Then the room list refreshes in real-time
✓ Given no rooms meet my capacity requirement, When results display, Then I see "No rooms available for X people" message

## Technical Notes

- API Enhancement: GET /api/rooms/availability?minCapacity={number}
- Backend: RoomRepository with conditional capacity filtering in LINQ
- C# Implementation: Additional WHERE clause on Capacity column
- Frontend: <CapacityFilter> component with Material-UI Slider
- Database: Rooms table includes Capacity column for efficient filtering

## Dependencies

- STORY-0 (Room availability endpoint must exist)

## Definition of Done

- All acceptance criteria met and tested
- Filter integrates seamlessly with existing availability endpoint
- Real-time updates as slider moves
- Clear visual feedback for no results scenario
- Mobile-responsive slider component

---

## STORY #3: BOOKING CANCELLATION

**Task Title:** [Story #3] As an employee, I want to cancel my bookings so rooms become available for others

**Custom Fields:**

- Story Points: 3
- Priority: Critical (P0)
- Sprint: Sprint 1
- Story Type: Feature
- Component: Backend
- Dependencies: STORY-0

**Description:**

## User Story

As an employee, I want to cancel my bookings, so rooms become available for others.

## Acceptance Criteria

✓ Given I have an active booking, When I choose to cancel it, Then I'm asked for confirmation before proceeding
✓ Given I confirm cancellation, When it processes, Then the room becomes immediately available for new bookings
✓ Given I try to cancel within 1 hour of the meeting start, When I attempt cancellation, Then I receive an error message

## Technical Notes

- API Endpoint: DELETE /api/bookings/{id}
- Business Logic: CancelBookingCommandHandler (CQRS pattern)
- Validation: Cannot cancel within 1 hour of start time
- Database: Update Bookings.Status to 2 (Cancelled)
- Events: BookingCancelledEvent published via MediatR
- Integration: Notification system for cancellation alerts

## Dependencies

- STORY-0 (Booking system must exist to cancel bookings)

## Definition of Done

- All acceptance criteria met and tested
- Business rules enforced (1-hour cancellation window)
- Room availability updated immediately
- Audit trail of cancellations maintained
- Notifications sent to relevant parties

---

## STORY #4: ROOM EQUIPMENT FILTERING

**Task Title:** [Story #4] As an employee, I want to filter available rooms based on required equipment so that I can select a room that supports my meeting needs

**Custom Fields:**

- Story Points: 5
- Priority: Medium (P2)
- Sprint: Sprint 2
- Story Type: Feature
- Component: Backend, Database
- Dependencies: STORY-0

**Description:**

## User Story

As an employee, I want to filter available rooms based on required equipment, so that I can select a room that supports my meeting needs.

## Acceptance Criteria

✓ Given I select required equipment (e.g., projector, whiteboard), When I view available rooms, Then only rooms with that equipment are shown
✓ Given I select multiple equipment requirements, When results display, Then rooms matching all selected equipment are shown
✓ Given no rooms meet my equipment requirements, When I view results, Then I see "No rooms with selected equipment" message

## Technical Notes

- Database Additions:
  • Equipment table: Id, Name, Category
  • RoomEquipment junction table: RoomId, EquipmentId, Quantity
- API Enhancement: GET /api/rooms/availability?equipment=Projector,Whiteboard
- Backend: Complex LINQ joins for equipment filtering
- Frontend: Multi-select component with equipment icons
- Seed Data: Standard equipment (Projector, Whiteboard, Video Conferencing, etc.)

## Dependencies

- STORY-0 (Base room system must exist)

## Definition of Done

- All acceptance criteria met and tested
- Database schema extended with proper relationships
- Equipment filtering works with AND logic (all selected equipment)
- Intuitive UI for equipment selection
- Equipment icons displayed in room cards

---

## STORY #5: ADMIN DASHBOARD VIEWING

**Task Title:** [Story #5] As an administrator, I want to view a dashboard so that I can monitor usage, spot trends and make data-driven decisions

**Custom Fields:**

- Story Points: 8
- Priority: High (P1)
- Sprint: Sprint 2
- Story Type: Feature
- Component: Frontend, Backend, API
- Dependencies: STORY-0

**Description:**

## User Story

As a system administrator, I want to view a dashboard, so that I can monitor usage, spot trends and make data-driven decisions.

## Acceptance Criteria

✓ Given I am logged in as an admin, When I open the dashboard, Then I see key metrics (daily bookings, room utilization, cancellations)
✓ Given I filter the dashboard by date range or specific room, When I apply filters, Then the metrics update accordingly
✓ Given new bookings occur while viewing the dashboard, When data refreshes, Then I see updated metrics (or refresh indicator)

## Technical Notes

- API Endpoints:
  • GET /api/admin/metrics/daily
  • GET /api/admin/metrics/rooms/{id}/utilization
  • GET /api/admin/reports/bookings
- Frontend: <AdminDashboard> component with Material-UI Grid
- Components: <BookingChart>, <RoomUtilization>, <RecentBookingsTable>
- State Management: React Query for metrics data
- Visualization: Charting library for data presentation

## Dependencies

- STORY-0 (Need booking data to display)

## Definition of Done

- All acceptance criteria met and tested
- Dashboard provides actionable insights at a glance
- Performance optimized for large datasets
- Responsive design works on all screen sizes
- Secure admin-only access enforced

---

## STORY #6: ROOM MAINTENANCE SCHEDULING

**Task Title:** [Story #6] As a facilities manager, I want to schedule maintenance for conference rooms so that rooms remain in good condition

**Custom Fields:**

- Story Points: 8
- Priority: Medium (P2)
- Sprint: Sprint 3
- Story Type: Feature
- Component: Backend, Database
- Dependencies: STORY-0, STORY-3

**Description:**

## User Story

As a facilities manager, I want to schedule maintenance for conference rooms, so that rooms remain in good condition.

## Acceptance Criteria

✓ Given I schedule maintenance for a room, When maintenance is scheduled, Then the room is marked unavailable during that period
✓ Given maintenance overlaps with existing bookings, When I schedule it, Then those bookings are automatically cancelled with notifications
✓ Given maintenance is completed early, When I update the schedule, Then the room becomes immediately available

## Technical Notes

- Business Rule: Maintenance bookings override and cancel existing bookings
- Implementation: ScheduleMaintenance service with transaction management
- Database: MaintenanceSchedules table with foreign key to Rooms
- Integration: Uses STORY-3 cancellation logic with custom reason
- Notifications: Automated alerts for cancelled bookings
- Authorization: Facilities manager role required

## Dependencies

- STORY-0 (Booking system foundation)
- STORY-3 (Cancellation logic for overlapping bookings)

## Definition of Done

- All acceptance criteria met and tested
- Transaction ensures data consistency (all-or-nothing)
- Proper notifications sent for affected bookings
- Facilities manager role properly implemented
- Maintenance calendar view available

---

## STORY #7: VISITOR BOOKING ASSISTANCE

**Task Title:** [Story #7] As a receptionist, I want to create and manage visitor bookings so that visitors can properly schedule and check in

**Custom Fields:**

- Story Points: 5
- Priority: Medium (P2)
- Sprint: Sprint 3
- Story Type: Feature
- Component: Frontend, Backend, Database
- Dependencies: STORY-0

**Description:**

## User Story

As a receptionist, I want to create and manage visitor bookings, so that visitors can properly schedule and check in.

## Acceptance Criteria

✓ Given a visitor requests a booking, When I enter their details and assign a host, Then a visitor booking is created successfully
✓ Given a visitor booking exists, When the visitor arrives, Then their details are visible at the reception check-in screen
✓ Given a host needs to modify a visitor booking, When changes are made, Then updates are reflected immediately in the system

## Technical Notes

- Database Schema:
  • Visitors table: Id, FirstName, LastName, Company, Email, Phone, HostEmployeeId, CheckedInAt, CheckedOutAt
  • VisitorBookings junction table: VisitorId, BookingId, CreatedBy
- Special Requirements: GDPR-compliant visitor data handling
- Integration: Visitor records linked to host employees and regular bookings
- Frontend: Simplified form for receptionists with guided workflow
- Security: Receptionist-specific role with appropriate permissions

## Dependencies

- STORY-0 (Base booking system required)

Definition of Done

- All acceptance criteria met and tested
- Visitor data handled per privacy regulations
- Check-in/out process streamlined for reception
- Host notifications for visitor bookings
- Visitor management interface intuitive for reception staff

---

## STORY #8: BOOKING CONFLICT RESOLUTION

**Task Title:** [Story #8] As an administrator, I want to resolve booking conflicts when multiple users request the same room so that room utilization is optimized and double-bookings are prevented

**Custom Fields:**

- Story Points: 8
- Priority: High (P1)
- Sprint: Sprint 3
- Story Type: Feature
- Component: Backend, Database, API
- Dependencies: STORY-5

**Description:**

## User Story

As an administrator, I want to resolve booking conflicts when multiple users request the same room, so that room utilization is optimized and double-bookings are prevented.

## Acceptance Criteria

✓ Given multiple conflicting bookings exist, When I view the conflict resolution panel, Then I see all overlapping requests clearly displayed
✓ Given I select which booking to prioritize in a conflict, When I resolve it, Then the other bookings are cancelled with notifications
✓ Given automated conflict detection runs, When conflicts are found, Then they are flagged for administrative review

## Technical Notes

- Automated Detection: ConflictDetectionJob (Hangfire/Quartz background job)
- Logic: Groups bookings by room and date, identifies overlaps
- Resolution: ResolveConflictCommand with MediatR
- Integration: Requires STORY-5 admin dashboard for visualization
- Notifications: Automated emails to affected users
- Audit Trail: Full logging of conflict resolution decisions

## Dependencies

- STORY-5 (Admin dashboard required for conflict interface)

 Definition of Done

- All acceptance criteria met and tested
- Automated detection runs efficiently on large datasets
- Resolution interface intuitive for administrators
- Comprehensive audit trail maintained
- Notifications provide clear explanation to users

---

## STORY #9: USAGE REPORTS GENERATION

**Task Title:** [Story #9] As an administrator, I want to generate usage reports for rooms and bookings so that I can identify usage patterns and support data-driven decisions about room management

**Custom Fields:**

- Story Points: 8
- Priority: Medium (P2)
- Sprint: Sprint 3
- Story Type: Feature
- Component: Backend, API
- Dependencies: STORY-5

**Description:**

## User Story

As an administrator, I want to generate usage reports for rooms and bookings, so that I can identify usage patterns and support data-driven decisions about room management.

## Acceptance Criteria

✓ Given I select a date range and report type, When I generate a report, Then it accurately reflects booking patterns and utilization
✓ Given a report is generated, When I choose to export it, Then it downloads in my selected format (PDF/CSV)
✓ Given I filter a report by specific rooms or departments, When I apply filters, Then the report updates with relevant data only

## Technical Notes

- Reporting API: Endpoints under /api/admin/reports/ namespace
- Report Types: Room utilization, booking trends, cancellation analysis
- Export Formats: CSV and PDF generation with proper formatting
- Performance: Server-side generation with optimized queries
- Integration: Part of STORY-5 admin dashboard
- Data Aggregation: Complex SQL queries or stored procedures

## Dependencies

- STORY-5 (Admin dashboard required as reporting interface)

 Definition of Done

- All acceptance criteria met and tested
- Reports provide actionable insights for facility management
- Export functionality works reliably for large datasets
- Filters allow for flexible data analysis
- Performance optimized for enterprise-scale data

---

## SPRINT ROADMAP SUMMARY

### Sprint 1 (Foundation): 11 points

- STORY-0: Basic Room Booking (5 pts) - *Core system*
- STORY-2: Room Capacity Filtering (3 pts) - *User enhancement*
- STORY-3: Booking Cancellation (3 pts) - *Core functionality*

### Sprint 2 (Enhancement): 21 points

- STORY-1: Recurring Meetings (8 pts) - *Advanced booking*
- STORY-4: Equipment Filtering (5 pts) - *User enhancement*
- STORY-5: Admin Dashboard (8 pts) - *Administration*

### Sprint 3 (Advanced): 29 points

- STORY-6: Maintenance Scheduling (8 pts) - *Facilities management*
- STORY-7: Visitor Booking (5 pts) - *Reception functionality*
- STORY-8: Conflict Resolution (8 pts) - *Administration*
- STORY-9: Usage Reports (8 pts) - *Analytics*

**Total Project Scope:** 61 story points

APPENDIX A: PROJECT STRUCTURE
text
ConferenceRoomBooking/
├── ConferenceRoomBooking.API/          # ASP.NET Core Web API
│   ├── Controllers/
│   ├── Services/
│   ├── Models/
│   ├── Data/                          # EF Core Context
│   ├── Middleware/
│   └── Program.cs
├── ConferenceRoomBooking.Domain/       # Domain Layer
│   ├── Entities/
│   ├── ValueObjects/
│   ├── Enums/
│   └── Exceptions/
├── ConferenceRoomBooking.Application/  # Application Layer
│   ├── Features/
│   │   ├── Bookings/
│   │   │   ├── CreateBooking/
│   │   │   └── CancelBooking/
│   │   └── Rooms/
│   ├── Common/
│   └── Interfaces/
├── ConferenceRoomBooking.Infrastructure/
├── ConferenceRoomBooking.Tests/        # xUnit Tests
└── client-app/                         # React Frontend
    ├── src/
    │   ├── components/
    │   │   ├── booking/
    │   │   ├── rooms/
    │   │   └── admin/
    │   ├── pages/
    │   ├── hooks/
    │   ├── services/
    │   └── utils/
    └── package.json
APPENDIX B: IMPLEMENTATION ROADMAP
Sprint 1 (Foundation): STORY-0, STORY-2, STORY-3
Sprint 2 (Enhancement): STORY-1, STORY-4, STORY-5
Sprint 3 (Advanced): STORY-6, STORY-7, STORY-8, STORY-9

APPENDIX C: BITCUBE DEVELOPMENT STANDARDS
Code Quality: SonarQube integration, 80%+ test coverage

Git Workflow: GitFlow with PR reviews

CI/CD: GitHub Actions for build/test/deploy

Documentation: Swagger for API, Storybook for components

Monitoring: Application Insights integration
