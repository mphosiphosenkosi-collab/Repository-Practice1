Conference Room Booking System - User Stories (C#/.NET & React Stack)
Project: Conference Room Booking System
Document Type: Requirements Specification (Technical Stack: ASP.NET Core / React)
Version: 3.0
Created: 2026/01/16
Author: BitCube Development Team
Approved By: Technical Architecture Board
Status: Approved for Sprint 1 Development

Architecture Overview
Backend Stack: ASP.NET Core Web API (C# 11), Entity Framework Core, SQL Server, JWT Authentication
Frontend Stack: React 18, TypeScript, Material-UI, Axios, React Query
Deployment: Docker containers, Azure App Service, CI/CD via GitHub Actions

STORY #0: BASIC ROOM BOOKING
ID: STORY-0
Persona: Employee
Business Value: Critical (P0)
Technical Complexity: Medium
Story Points: 5
Dependencies: Authentication System
Sprint Target: 1

User Story:

As a company employee, I want to view available rooms by time and date and book one, so that I can secure a space for my meeting.

BACKEND REQUIREMENTS (ASP.NET Core / C#)
API Endpoints:

GET /api/rooms/availability?date=2026-01-16&duration=60

Controller: RoomsController

Service: RoomAvailabilityService

Response: List<RoomAvailabilityDto>

Logic: Query SQL Server via EF Core, apply time conflict detection

POST /api/bookings

Controller: BookingsController

Validation: CreateBookingValidator (FluentValidation)

Request Body: CreateBookingCommand

Response: BookingCreatedResponse (201 Created)

Key .NET Concepts Demonstrated:

ASP.NET Core Controllers with Action Results

Entity Framework Core migrations and LINQ queries

Repository Pattern with Unit of Work

MediatR for CQRS pattern implementation

FluentValidation for request validation

AutoMapper for DTO transformations

Database Schema:

sql
CREATE TABLE Rooms (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(100) NOT NULL,
    Capacity INT NOT NULL,
    Floor INT NOT NULL,
    CreatedAt DATETIME2 DEFAULT GETDATE()
);

CREATE TABLE Bookings (
    Id UNIQUEIDENTIFIER PRIMARY KEY DEFAULT NEWID(),
    RoomId INT FOREIGN KEY REFERENCES Rooms(Id),
    EmployeeId NVARCHAR(450) FOREIGN KEY REFERENCES AspNetUsers(Id),
    StartTime DATETIME2 NOT NULL,
    EndTime DATETIME2 NOT NULL,
    Status INT NOT NULL, -- 1=Active, 2=Cancelled
    CreatedAt DATETIME2 DEFAULT GETDATE()
);
FRONTEND REQUIREMENTS (React / TypeScript)
Components:

<RoomBookingPage /> - Main container component

<DateTimePicker /> - Material-UI date/time selection

<RoomGrid /> - Display available rooms

<BookingModal /> - Confirmation dialog

State Management:

typescript
// Types
interface Room {
  id: number;
  name: string;
  capacity: number;
  floor: number;
  equipment: string[];
}

interface BookingFormState {
  selectedDate: Date;
  selectedTime: string;
  duration: number;
  attendees: number;
}

// Custom Hooks
const useRoomAvailability = (date: Date, duration: number) => {
  return useQuery(['rooms', date, duration], 
    () => apiClient.get<Room[]>(`/api/rooms/availability`, {
      params: { date: format(date, 'yyyy-MM-dd'), duration }
    })
  );
};
Key React Concepts:

React Hooks (useState, useEffect, useMemo)

React Query for server state management

Context API for global state (User context)

Custom hooks for business logic

TypeScript interfaces for type safety

Material-UI theming and components

ACCEPTANCE CRITERIA (Full Stack)
ID	Scenario	Backend Test (.NET)	Frontend Test (React)
AC-0.1	View available rooms	Integration test verifies 200 OK with correct room count	Component renders room cards from API response
AC-0.2	No rooms available	Returns empty array with 200 OK	Displays "No rooms available" message
AC-0.3	Successful booking	POST returns 201 Created with booking ID	Shows success toast, clears form
AC-0.4	Time conflict	Returns 409 Conflict with error details	Displays conflict error message
AC-0.5	Invalid input	Returns 400 Bad Request with validation errors	Shows validation errors inline
STORY #1: RECURRING MEETINGS SETUP
ID: STORY-1
Persona: Employee
Business Value: High (P1)
Technical Complexity: High
Story Points: 8
Dependencies: STORY-0
Sprint Target: 2

User Story:

As an employee, I want to schedule meetings that repeat on a defined pattern, so that I don't have to create each occurrence manually.

BACKEND REQUIREMENTS
API Endpoint: POST /api/bookings/recurring
Request Body:

json
{
  "roomId": 101,
  "startDate": "2026-01-20",
  "endDate": "2026-03-20",
  "startTime": "09:00",
  "endTime": "10:00",
  "recurrenceRule": "FREQ=WEEKLY;INTERVAL=1;BYDAY=MO,WE,FR",
  "exceptions": ["2026-02-10"]
}
C# Implementation:

csharp
public class RecurringBookingService
{
    public async Task<RecurringBookingResult> CreateRecurringBooking(CreateRecurringBookingCommand command)
    {
        var rrule = RRule.Parse(command.RecurrenceRule);
        var occurrences = rrule.Between(command.StartDate, command.EndDate);
        
        // Remove exceptions
        occurrences = occurrences.Where(d => !command.Exceptions.Contains(d.Date));
        
        // Create individual bookings
        var bookings = occurrences.Select(date => new Booking
        {
            RoomId = command.RoomId,
            StartTime = date.Date.Add(command.StartTime),
            EndTime = date.Date.Add(command.EndTime),
            EmployeeId = command.EmployeeId
        });
        
        await _context.Bookings.AddRangeAsync(bookings);
        await _context.SaveChangesAsync();
        
        return new RecurringBookingResult(bookings.Count());
    }
}
Key .NET Patterns:

RRule library for recurrence pattern parsing

Bulk operations with Entity Framework

Transaction management with Unit of Work

Background job scheduling (Hangfire for conflict resolution)

FRONTEND REQUIREMENTS
Components:

<RecurrencePatternPicker /> - Interactive rule builder

<CalendarPreview /> - Visual preview of occurrences

<ExceptionDatePicker /> - Manage excluded dates

Implementation:

typescript
// Recurrence rule builder
const RecurrenceBuilder: React.FC = () => {
  const [rule, setRule] = useState({
    frequency: 'WEEKLY',
    interval: 1,
    byDay: ['MO', 'WE', 'FR'],
    endType: 'UNTIL',
    until: null,
    count: null
  });

  const buildRRuleString = (): string => {
    // Convert to RRULE format: FREQ=WEEKLY;INTERVAL=1;BYDAY=MO,WE,FR
    const parts = [`FREQ=${rule.frequency}`, `INTERVAL=${rule.interval}`];
    if (rule.byDay.length > 0) parts.push(`BYDAY=${rule.byDay.join(',')}`);
    return parts.join(';');
  };
};
ACCEPTANCE CRITERIA
ID	Scenario	Backend Test	Frontend Test
AC-1.1	Weekly pattern	Creates 12 bookings for 12-week period	Pattern builder shows correct preview
AC-1.2	Monthly pattern	Creates bookings on 15th of each month	Month selector works correctly
AC-1.3	Pattern exceptions	Skips specified dates in series	Exception dates show as excluded in preview
AC-1.4	Conflict in series	Rolls back transaction, returns partial failure	Shows which dates failed with reasons
STORY #2: ROOM CAPACITY FILTERING
ID: STORY-2
Persona: Employee
Business Value: High (P1)
Technical Complexity: Low
Story Points: 3
Dependencies: STORY-0
Sprint Target: 1

User Story:

As an employee, I want to filter rooms by seating capacity, so I can find rooms that fit all participants.

BACKEND REQUIREMENTS
API Enhancement: GET /api/rooms/availability?minCapacity=10

C# Implementation:

csharp
public class RoomRepository : IRoomRepository
{
    public async Task<List<Room>> GetAvailableRooms(DateTime date, int duration, int? minCapacity)
    {
        var query = _context.Rooms
            .Where(r => !r.Bookings.Any(b => 
                b.StartTime < date.AddMinutes(duration) && 
                b.EndTime > date));            
        if (minCapacity.HasValue)
        {
            query = query.Where(r => r.Capacity >= minCapacity.Value);
        }
            
        return await query
            .OrderBy(r => r.Capacity)
            .ToListAsync();
    }
}

FRONTEND REQUIREMENTS
Component: <CapacityFilter />

typescript

const CapacityFilter: React.FC<CapacityFilterProps> = ({ onChange }) => {
  const [capacity, setCapacity] = useState<number>(5);
  
  return (
    <Box>
      <Typography>Minimum Capacity: {capacity}</Typography>
      <Slider
        value={capacity}
        min={1}
        max={50}
        step={1}
        onChange={(_, value) => {
          setCapacity(value as number);
          onChange(value as number);
        }}
      />
      <Chip label={`${capacity}+ people`} />
    </Box>
  );
};

STORY #3: BOOKING CANCELLATION
ID: STORY-3
Persona: Employee
Business Value: Critical (P0)
Technical Complexity: Medium
Story Points: 3
Dependencies: STORY-0
Sprint Target: 1

User Story:

As an employee, I want to cancel my bookings, so rooms become available for others.

BACKEND REQUIREMENTS
API Endpoint: DELETE /api/bookings/{id}

Business Logic:

csharp
public class CancelBookingCommandHandler : IRequestHandler<CancelBookingCommand, bool>
{
    public async Task<bool> Handle(CancelBookingCommand request, CancellationToken ct)
    {
        var booking = await _context.Bookings
            .FirstOrDefaultAsync(b => b.Id == request.BookingId && b.EmployeeId == request.EmployeeId);
        if (booking == null) return false;
        if (booking.StartTime < DateTime.UtcNow.AddHours(1))
            throw new BusinessException("Cannot cancel within 1 hour of start");
        booking.Status = BookingStatus.Cancelled;
        await_context.SaveChangesAsync();
        // Publish domain event
        await _mediator.Publish(new BookingCancelledEvent(booking.Id));
          return true;
    }
}
STORY #4: ROOM EQUIPMENT FILTERING
ID: STORY-4
Persona: Employee
Business Value: Medium (P2)
Technical Complexity: Medium
Story Points: 5
Dependencies: STORY-0
Sprint Target: 2

API Enhancement: GET /api/rooms/availability?equipment=Projector,Whiteboard

Database Schema Addition:

sql
CREATE TABLE Equipment (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(50) NOT NULL,
    Category NVARCHAR(50)
);

CREATE TABLE RoomEquipment (
    RoomId INT FOREIGN KEY REFERENCES Rooms(Id),
    EquipmentId INT FOREIGN KEY REFERENCES Equipment(Id),
    Quantity INT NOT NULL DEFAULT 1,
    PRIMARY KEY (RoomId, EquipmentId)
);
STORY #5: ADMIN DASHBOARD VIEWING
ID: STORY-5
Persona: Administrator
Business Value: High (P1)
Technical Complexity: High
Story Points: 8
Dependencies: STORY-0
Sprint Target: 2

API Endpoints:

GET /api/admin/metrics/daily

GET /api/admin/metrics/rooms/{id}/utilization

GET /api/admin/reports/bookings

React Dashboard:

typescript
const AdminDashboard: React.FC = () => {
  const { data: metrics } = useQuery('admin-metrics', 
    () => apiClient.get<DashboardMetrics>('/api/admin/metrics/daily'));
  
  return (
    <Grid container spacing={3}>
      <Grid item xs={12} md={6}>
        <BookingChart data={metrics?.dailyBookings} />
      </Grid>
      <Grid item xs={12} md={6}>
        <RoomUtilization data={metrics?.roomUtilization} />
      </Grid>
      <Grid item xs={12}>
        <RecentBookingsTable />
      </Grid>
    </Grid>
  );
};
STORY #6: ROOM MAINTENANCE SCHEDULING
ID: STORY-6
Persona: Facilities Manager
Business Value: Medium (P2)
Technical Complexity: High
Story Points: 8
Dependencies: STORY-0, STORY-3
Sprint Target: 3

Business Rule: Maintenance bookings override and cancel existing bookings.

Implementation:

csharp
public async Task<MaintenanceScheduleResult> ScheduleMaintenance(ScheduleMaintenanceCommand cmd)
{
    using var transaction = await _context.Database.BeginTransactionAsync(); 
    try
    {
        // Cancel overlapping bookings
        var overlappingBookings = await _context.Bookings
            .Where(b => b.RoomId == cmd.RoomId &&
                       b.StartTime < cmd.EndTime &&
                       b.EndTime > cmd.StartTime &&
                       b.Status == BookingStatus.Active)
            .ToListAsync();
            
        foreach (var booking in overlappingBookings)
        {
            booking.Status = BookingStatus.Cancelled;
            booking.CancellationReason = "Maintenance Scheduled";
            
            // Send notification
            await _notificationService.SendBookingCancelledNotification(booking);
        }
        
        // Create maintenance record
        var maintenance = new MaintenanceSchedule
        {
            RoomId = cmd.RoomId,
            StartTime = cmd.StartTime,
            EndTime = cmd.EndTime,
            Reason = cmd.Reason,
            ScheduledBy = cmd.ScheduledBy
        };
        
        await _context.MaintenanceSchedules.AddAsync(maintenance);
        await _context.SaveChangesAsync();
        await transaction.CommitAsync();
        
        return new MaintenanceScheduleResult(maintenance.Id, overlappingBookings.Count);
    }
    catch
    {
        await transaction.RollbackAsync();
        throw;
    }
}
STORY #7: VISITOR BOOKING ASSISTANCE
ID: STORY-7
Persona: Receptionist
Business Value: Medium (P2)
Technical Complexity: Medium
Story Points: 5
Dependencies: STORY-0
Sprint Target: 3

Special Requirements: Visitor records, host assignment, check-in/out tracking.

Database Schema:

sql
CREATE TABLE Visitors (
    Id UNIQUEIDENTIFIER PRIMARY KEY DEFAULT NEWID(),
    FirstName NVARCHAR(100) NOT NULL,
    LastName NVARCHAR(100) NOT NULL,
    Company NVARCHAR(200),
    Email NVARCHAR(200),
    Phone NVARCHAR(20),
    HostEmployeeId NVARCHAR(450),
    CheckedInAt DATETIME2,
    CheckedOutAt DATETIME2
);

CREATE TABLE VisitorBookings (
    VisitorId UNIQUEIDENTIFIER FOREIGN KEY REFERENCES Visitors(Id),
    BookingId UNIQUEIDENTIFIER FOREIGN KEY REFERENCES Bookings(Id),
    CreatedBy NVARCHAR(450), -- Receptionist ID
    PRIMARY KEY (VisitorId, BookingId)
);
STORY #8: BOOKING CONFLICT RESOLUTION
ID: STORY-8
Persona: Administrator
Business Value: Medium (P2)
Technical Complexity: High
Story Points: 8
Dependencies: STORY-5
Sprint Target: 3

Automated Conflict Detection:

csharp
// Background job (Hangfire/Quartz)
public class ConflictDetectionJob
{
    public async Task DetectAndResolveConflicts()
    {
        var conflicts = await _context.Bookings
            .Where(b => b.Status == BookingStatus.Active)
            .GroupBy(b => new { b.RoomId, b.StartTime.Date })
            .Where(g => g.Count() > 1)
            .Select(g => new ConflictGroup(g.Key.RoomId, g.Key.Date, g.ToList()))
            .ToListAsync();
        foreach (var conflict in conflicts)
        {
            await_mediator.Send(new ResolveConflictCommand(conflict));
        }
    }
}
STORY #9: USAGE REPORTS GENERATION
ID: STORY-9
Persona: Administrator
Business Value: Medium (P2)
Technical Complexity: High
Story Points: 8
Dependencies: STORY-5
Sprint Target: 3

Reporting API:

[HttpGet("reports/room-utilization")]
public async Task<IActionResult> GetRoomUtilizationReport(
    [FromQuery] DateTime startDate,
    [FromQuery] DateTime endDate)
{
    var report = await _reportService.GenerateRoomUtilizationReport(startDate, endDate);
  
    // Return as CSV
    var csv = _csvService.ConvertToCsv(report);
    return File(Encoding.UTF8.GetBytes(csv), 
                "text/csv", 
                $"room-utilization-{startDate:yyyy-MM-dd}-to-{endDate:yyyy-MM-dd}.csv");
}
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
