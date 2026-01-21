# API Usage Examples - Conference Room Booking System
**Developer**: Siphosenkosi | **Assignment**: BitCube 4.3 - API Documentation & Validation

This document provides practical, runnable examples for the Conference Room Booking System API. Use these examples to understand how to interact with the API endpoints defined in the [OpenAPI specification](./api-documentation.yaml).

## 📋 Prerequisites
- A valid **JWT Bearer Token** (obtain from your authentication provider)
- **Base URL**: `https://api.staging.bitcube.ac.za/v1` (staging environment)
- All requests require the `Authorization: Bearer <token>` header
- All dates/times must be in **ISO 8601 format**: `YYYY-MM-DDTHH:MM:SSZ`

---

## 1. 🔐 Authentication Examples
*While the authentication endpoint isn't detailed in v1, here's how to use the required Bearer token.*

### Using the Authorization Header
```bash
# cURL example with Bearer token
curl -X GET "https://api.staging.bitcube.ac.za/v1/rooms" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlNpcGhvc2Vua29zaSIsImlhdCI6MTUxNjIzOTAyMn0.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c" \
  -H "Content-Type: application/json"

# JavaScript Fetch API example
const token = "your_jwt_token_here";
const response = await fetch('https://api.staging.bitcube.ac.za/v1/rooms', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  }
});
Important: Replace the example token with your actual JWT. Without a valid token, you'll receive a 401 Unauthorized error.

2. 🏢 Room Management Examples
2.1 List All Rooms (with optional filters)
Endpoint: GET /rooms

bash
# Get all rooms
curl -X GET "https://api.staging.bitcube.ac.za/v1/rooms" \
  -H "Authorization: Bearer YOUR_TOKEN"

# Get rooms with capacity ≥ 10 and projector
curl -X GET "https://api.staging.bitcube.ac.za/v1/rooms?minCapacity=10&hasProjector=true" \
  -H "Authorization: Bearer YOUR_TOKEN"
Example Successful Response (200 OK):

json
[
  {
    "id": 101,
    "name": "Boardroom Alpha",
    "capacity": 12,
    "isActive": true,
    "hasProjector": true,
    "hasWhiteboard": true,
    "location": "Floor 5, North Wing"
  }
]
2.2 Get Specific Room Details
Endpoint: GET /rooms/{id}

bash
curl -X GET "https://api.staging.bitcube.ac.za/v1/rooms/101" \
  -H "Authorization: Bearer YOUR_TOKEN"
2.3 Create a New Room (Admin)
Endpoint: POST /rooms

bash
curl -X POST "https://api.staging.bitcube.ac.za/v1/rooms" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Innovation Hub",
    "capacity": 15,
    "hasProjector": true,
    "hasWhiteboard": false,
    "location": "Floor 3, East Wing"
  }'
Response (201 Created):

json
{
  "id": 105,
  "name": "Innovation Hub",
  "capacity": 15,
  "isActive": true,
  "hasProjector": true,
  "hasWhiteboard": false,
  "location": "Floor 3, East Wing"
}
3. 📅 Booking Management Examples
3.1 Create a Booking
Endpoint: POST /bookings

bash
curl -X POST "https://api.staging.bitcube.ac.za/v1/bookings" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "roomId": 101,
    "title": "BitCube Team Stand-up",
    "startTime": "2024-03-25T09:00:00Z",
    "endTime": "2024-03-25T09:30:00Z"
  }'
Successful Response (201 Created) includes Location header:

text
HTTP/1.1 201 Created
Location: /bookings/5001
Content-Type: application/json

{
  "id": 5001,
  "roomId": 101,
  "userId": "user_siphosenkosi",
  "title": "BitCube Team Stand-up",
  "startTime": "2024-03-25T09:00:00Z",
  "endTime": "2024-03-25T09:30:00Z",
  "status": "CONFIRMED",
  "createdAt": "2024-03-20T14:30:00Z"
}
3.2 List All Bookings (with filters)
Endpoint: GET /bookings

bash
# Get all bookings
curl -X GET "https://api.staging.bitcube.ac.za/v1/bookings" \
  -H "Authorization: Bearer YOUR_TOKEN"

# Get bookings for specific room
curl -X GET "https://api.staging.bitcube.ac.za/v1/bookings?roomId=101" \
  -H "Authorization: Bearer YOUR_TOKEN"
3.3 Cancel a Booking
Endpoint: DELETE /bookings/{id}

bash
curl -X DELETE "https://api.staging.bitcube.ac.za/v1/bookings/5001" \
  -H "Authorization: Bearer YOUR_TOKEN"
Response: 204 No Content (successful cancellation)

4. ✅ Availability Checking Examples
4.1 Find Available Rooms for a Time Slot
Endpoint: GET /availability

bash
# Check for rooms available from 2 PM to 4 PM on March 25th
curl -X GET "https://api.staging.bitcube.ac.za/v1/availability?startTime=2024-03-25T14:00:00Z&endTime=2024-03-25T16:00:00Z&minCapacity=8" \
  -H "Authorization: Bearer YOUR_TOKEN"
Response (200 OK):

json
[
  {
    "id": 102,
    "name": "Conference Room Beta",
    "capacity": 10,
    "isActive": true,
    "hasProjector": true,
    "hasWhiteboard": true,
    "location": "Floor 6"
  }
]
4.2 Check Specific Room Availability
Endpoint: GET /availability/rooms/{id}

bash
# Check if Room 101 is available from 9 AM to 10 AM
curl -X GET "https://api.staging.bitcube.ac.za/v1/availability/rooms/101?startTime=2024-03-25T09:00:00Z&endTime=2024-03-25T10:00:00Z" \
  -H "Authorization: Bearer YOUR_TOKEN"
If available:

json
{
  "isAvailable": true,
  "conflictingBooking": null
}
If booked:

json
{
  "isAvailable": false,
  "conflictingBooking": {
    "id": 5001,
    "title": "BitCube Team Stand-up",
    "startTime": "2024-03-25T09:00:00Z",
    "endTime": "2024-03-25T09:30:00Z"
  }
}
🚨 Common Error Scenarios & Responses
400 Bad Request (Invalid Data)
bash
# Missing required 'title' field
curl -X POST "https://api.staging.bitcube.ac.za/v1/bookings" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "roomId": 101,
    "startTime": "2024-03-25T09:00:00Z",
    "endTime": "2024-03-25T10:00:00Z"
  }'
Response:

json
{
  "errorCode": "VALIDATION_ERROR",
  "message": "Missing required field: title",
  "timestamp": "2024-03-20T14:35:00Z"
}
401 Unauthorized (Invalid/Missing Token)
json
{
  "errorCode": "AUTH_REQUIRED",
  "message": "Valid authentication token is required",
  "timestamp": "2024-03-20T14:36:00Z"
}
404 Not Found (Resource Doesn't Exist)
bash
curl -X GET "https://api.staging.bitcube.ac.za/v1/rooms/999" \
  -H "Authorization: Bearer YOUR_TOKEN"
Response:

json
{
  "errorCode": "ROOM_NOT_FOUND",
  "message": "Room with ID 999 does not exist",
  "timestamp": "2024-03-20T14:37:00Z"
}
409 Conflict (Double Booking Attempt)
json
{
  "errorCode": "BOOKING_CONFLICT",
  "message": "Room is already booked for the requested time period",
  "details": "Conflict with booking ID 5001 (09:00-09:30)",
  "timestamp": "2024-03-20T14:38:00Z"
}
📝 Best Practices
Always check HTTP status codes before processing response bodies

Use the staging environment (api.staging.bitcube.ac.za) for testing

Implement retry logic for 5xx server errors

Cache room data to reduce API calls

Validate dates locally before sending to API (endTime > startTime)

Examples valid for API version 1.0.0 | Generated for BitCube Assignment 4.3 | Last updated: 21 January 2026