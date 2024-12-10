
# WebSocket Integration Documentation

This document explains how to use WebSocket in the frontend to handle real-time updates for clients, employees, and their related data.

## WebSocket URL
To connect to the WebSocket server, use the following URL format:

```
wss://<your-server-domain>/ws/
```
---

## Events and Payloads

The WebSocket server sends different types of events related to clients, employees, and their activities. Below is the list of possible events and their payload structures.

### 1. **Client Events**

#### Event: `client_create` / `client_update`
Triggered when a client is created or updated.

**Payload:**
```json
{
  "event": "client_create",
  "data": {
    "id": 1,
    "first_seen": "2023-12-10T10:00:00Z",
    "last_seen": "2023-12-10T15:00:00Z",
    "visit_count": 5,
    "gender": "male",
    "age": 30,
    "image": "https://<your-server-domain>/media/client_images/image.jpg",
    "visit_histories": [
      {
        "datetime": "2023-12-10T10:30:00Z",
        "device_id": "1"
      }
    ]
  }
}
```

#### Event: `client_delete`
Triggered when a client is deleted.

**Payload:**
```json
{
  "event": "client_delete",
  "data": {
    "id": 1
  }
}
```

---

### 2. **Client Visit History Events**

#### Event: `client_visit_create` / `client_visit_update`
Triggered when a visit history record is created or updated.

**Payload:**
```json
{
  "event": "client_visit_create",
  "data": {
    "datetime": "2023-12-10T12:00:00Z",
    "device_id": "XYZ789",
    "client": 1
  }
}
```

#### Event: `client_visit_delete`
Triggered when a visit history record is deleted.

**Payload:**
```json
{
  "event": "client_visit_delete",
  "data": {
    "datetime": "2023-12-10T12:00:00Z",
    "device_id": "XYZ789",
    "client": 1
  }
}
```

---

### 3. **Employee Events**

#### Event: `employee_create` / `employee_update`
Triggered when an employee is created or updated.

**Payload:**
```json
{
  "event": "employee_create",
  "data": {
    "id": 1,
    "first_name": "John",
    "last_name": "Doe",
    "email": "john.doe@example.com",
    "phone_number": "1234567890",
    "image": "http://<your-server-domain>/media/employee_images/john_doe.jpg",
    "working_graphic": 2
  }
}
```

#### Event: `employee_delete`
Triggered when an employee is deleted.

**Payload:**
```json
{
  "event": "employee_delete",
  "data": {
    "id": 1
  }
}
```

---

### 4. **Employee Attendance Events**

#### Event: `employee_attendance`
Triggered when an attendance record is created.

**Payload:**
```json
{
  "event": "employee_attendance",
  "data": {
    "employee_id": 1,
    "device_id": "123ABC",
    "image": "http://<your-server-domain>/media/attendance_images/attendance.jpg",
    "datetime": "2023-12-10T08:00:00Z",
    "score": 95
  }
}
```

#### Event: `employee_attendance_delete`
Triggered when an attendance record is deleted.

**Payload:**
```json
{
  "event": "employee_attendance_delete",
  "data": {
    "employee_id": 1,
    "datetime": "2023-12-10T08:00:00Z"
  }
}
```

---

## Handling Events in the Frontend

Below is an example of how to handle WebSocket events in JavaScript:

```javascript
const socket = new WebSocket('wss:/<your-server-domain>/ws/');

socket.onopen = function() {
    console.log('WebSocket connection established.');
};

socket.onmessage = function(event) {
    const message = JSON.parse(event.data);
    console.log('Received event:', message.event);
    console.log('Event data:', message.data);

    switch (message.event) {
        case 'client_create':
            // Handle client creation
            break;
        case 'client_update':
            // Handle client update
            break;
        case 'client_delete':
            // Handle client deletion
            break;
        case 'employee_create':
            // Handle employee creation
            break;
        // Add more cases for other events
    }
};

socket.onclose = function() {
    console.log('WebSocket connection closed.');
};
```

---

## Notes
- Replace `<your-server-domain>` with the actual domain or IP address of your WebSocket server.
- Ensure the WebSocket server is running and properly configured in Django Channels.
