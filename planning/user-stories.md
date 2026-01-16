# Room Booking User Stories

## Story #0 Basic room booking (employee)

**As a** company employee
**I want to** view available basic rooms by time and date
**So that** find a room that meets all my meeting requirements

### Story #1 Acceptance Criteria

- [ ] Given I am logged in, When I select time and date, Then see a list of available rooms

- [ ] Given no rooms are available, When I search, Then it means no rooms are available

- [ ] Given rooms are available, When search results are displayed, Then rooms are displayed with pricing

### Story Points

3

### Priority

High

### Dependencies

- User must be logged in or authenticated

### Technical Notes

- Availability of data from existing scheduling system

### Design Notes

- Use a calendar style

## Story #1 Recurring meetings setup (Employee)

**As a** employee
**I want to** schedule a meeting that occurs frequently on a defined pattern
**So that** I do not have to manually create meetings the next time

### Acceptance Criteria

- [ ] Given I am creating a meeting, When I select a recurring option, Then I can choose a recurrence pattern (daily, weekly or monthly)

- [ ] Given required fields are missing, When I save the meeting, Then all instances are created automatically

- [ ] Given required fields are missing, When I save, Then I must see a validation error

### Story Points

5

### Priority

High

### Dependencies

- Basic creation of meeting

### Technical Notes

- Recurrence rules should follow standard calendar logic

### Design Notes

- Recurrence options should be simple and clearly labelled

## Story #2 Room capacity filtering (employee)

**As a** employee
**I want to** filter available rooms based on required seating capacity
**So that** I can find a room that fits all meeting participants

### Acceptance Criteria

- [ ] Given I am viewing available rooms, When I enter the number of attendees, Then only rooms with sufficient capacity are displayed

- [ ] Given no rooms are available that meet capacity, When results are displayed, Then I must see a clear message warning "No suitable rooms available"

- [ ] Given I change the count, When the filter is updated, Then room list refreshes accordingly

### Story Points

3

### Priority

High

### Dependencies

- View available rooms

### Technical Notes

- Room capacity data must be stored and searchable

- Filtering should be performed in real time

### Design Notes

- Display room capacity clearly in results

## Story #3 Booking cancellation (employee)

**As a** employee
**I want to** cancel an existing booking
**So that** the room is released for others

### Acceptance Criteria

- [ ] Given I have an existing booking, When I choose to cancel it, Then I am asked to confirm the cancellation

- [ ] Given I confirm cancellation, When the booking is cancelled, Then the room is released and no longer reserved

- [ ] Given a booking is cancelled, When I view my bookings, Then the cancelled booking is no longer listed

### Story Points

3

### Priority

High

### Dependencies

- Cancel room

### Technical Notes

- Room capacity data must be stored and searchable

- Filtering should be performed in real time

### Design Notes

- Display room capacity clearly in results

## Story #4 Room equipment requirements (employee)

**As a** employee
**I want to** filter available rooms based on required equipment
**So that** I can select a room that supports my meeting needs

### Acceptance Criteria

- [ ] Given I am viewing available rooms, When I select required equipment, Then only rooms with that equipment are shown

- [ ] Given multiple equipment options, When the results are displayed, Then rooms matching all selected equipment are shown

- [ ] Given no rooms meet the equipment requirements, When results are displayed, Then I see a clear "No suitable rooms available" message

### Story Points

3

### Priority

High

### Dependencies

- View available rooms

### Technical Notes

- Room equipment attributes must be stored and searchable

- Filtering logic should support multiple equipment selections

### Design Notes

- Use checkboxes or multi-select for equipment in room results

## Story #5 Admin dashboard viewing (Admin)

**As a** admin
**I want to** view a dashboard
**So that** I can monitor usage, spot trends and make data-driven decisions

### Acceptance Criteria

- [ ] Given I am logged in as an admin, When I open the dashboard, Then I see key metrics (room usage, bookings, cancellations)

- [ ] Given I am viewing the dashboard, When I filter by date or room, Then metrics update accordingly

- [ ] Given system events occur (new booking/cancellation), When the dashboard refreshes, Then updated data is displayed

### Story Points

5

### Priority

High

### Dependencies

- Admin login/auth for room booking data

### Technical Notes

- Dashboard should pull from existing data

- Ensure data is accurate

### Design Notes

- Visual charts

## Story #6 Room maintenance scheduling (Facilities Manager)

**As a** Facilities Manager
**I want to** schedule maintenance for conference rooms
**So that** rooms remain in good condition

### Acceptance Criteria

- [ ] Given a room requires maintenance, When I schedule it, Then the room is marked as unavailable during that time

- [ ] Given scheduled maintenance, When a user tries to book the room, Then user gets a notification "the room is unavailable"

- [ ] Given maintenance is completed or cancelled, When the schedule updates, Then the room becomes available immediately

- [ ] Given maintenance is scheduled, When the time overlaps an existing booking, Then the booking is prevented or flagged for admin resolution

### Story Points

5

### Priority

High

### Dependencies

- Room booking system

### Technical Notes

- Maintenance scheduling should integrate with room availability database
- Maintenance bookings override employee bookings

### Design Notes

- UI should allow selection of date, time, and affected rooms

## Story #7 Visitor booking assistance (Receptionist)

**As a** Receptionist
**I want to** create and manage visitor bookings
**So that** visitors can properly schedule and check in

### Acceptance Criteria

- [ ] Given a visitor request, When I enter visitor details and assign a host, Then the visitor booking is created successfully

- [ ] Given a visitor is scheduled, When the host or receptionist edits or cancels the booking, Then the changes are reflected immediately

- [ ] Given a visitor booking is created, When the visitor arrives, Then visitor name, host, time, and room are visible at reception

### Story Points

5

### Priority

High

### Dependencies

- Booking system

- Receptionist login/auth

### Technical Notes

- Ensure privacy

### Design Notes

- Form should be simple and guide receptionist through necessary fields

- Display upcoming visitor list clearly on check-in

## Story #8 Booking conflict resolution (Admin)

**As a** Admin
**I want to** resolve booking conflicts when multiple users request the same room
**So that** room utilization is optimized and double-bookings are prevented

### Acceptance Criteria

- [ ] Given multiple conflicting bookings, When I view the admin conflict panel, Then I see all overlapping requests clearly

- [ ] Given a conflict is identified, When I resolve it, Then the affected bookings are updated and notifications sent to users

- [ ] Given resolution actions are taken, When the system updates, Then the final room schedule is accurate and consistent

### Story Points

5

### Priority

High

### Dependencies

- Admin dashboard access

- Booking system

### Technical Notes

- Conflict detection logic must run in real time
- Conflict detection depends on accurate time synchronization

### Design Notes

- Conflict resolution interface should allow easy selection of which booking to keep

- Conflicts should be visually clear

## Story #9 Usage reports generation (Admin)

**As a** Admin
**I want to** generate usage reports for rooms and bookings
**So that** I can identify usage patterns and support data-driven decisions about room management 

### Acceptance Criteria

- [ ] Given I am logged in as an admin, When I access the reports section, Then I can select date ranges, rooms, and metrics to generate a report

- [ ] Given report criteria are selected, When I generate the report, Then it accurately reflects bookings, cancellations, and usage trends

- [ ] Given a report is generated, When I export it, Then it is available in PDF or CSV format

### Story Points

5

### Priority

High

### Dependencies

- Admin dashboard

- Booking and room usage info

### Technical Notes

- Reports should be generated

- Support multiple export formats

### Design Notes

- Provide charts and tables for quick insights