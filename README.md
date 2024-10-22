# AssignmentAPI
1.User Authentication
1. User Authentication
User Registration
POST /auth/register

{
  "username": "string",
  "email": "string",
  "password": "string"
}

Response: { "message": "Verification email sent." }
Email Verification

GET /auth/verify/{token}
Response: { "message": "Email verified successfully." }
User Login

POST /auth/login

Request:

{
  "email": "string",
  "password": "string"
}

Response: { "token": "jwt_token" }
Request:
{
  "email": "string"
}

Response: { "message": "Password reset code sent." }
Password Reset

POST /auth/reset-password/confirm
Request:
{
  "code": "string",
  "newPassword": "string"
}
Response: { "message": "Password reset successfully." }

2.Contact Management
Endpoints
Add Contact

POST /contacts
Headers: Authorization: Bearer <token>
Request:
{
  "name": "string",
  "email": "string",
  "phone": "string",
  "address": "string",
  "timezone": "string"
}

Response: { "id": "uuid", "createdAt": "datetime" }
Retrieve Contacts

GET /contacts
Headers: Authorization: Bearer <token>
Query Params: ?name=string&email=string&timezone=string&sort=string
Response: [ { "id": "uuid", "name": "string", ... } ]
Update Contact

PUT /contacts/{id}
Headers: Authorization: Bearer <token>
Request:
{
  "name": "string",
  "email": "string",
  "phone": "string",
  "address": "string",
  "timezone": "string"
}
Response: { "message": "Contact updated." }
Soft Delete Contact

DELETE /contacts/{id}
Headers: Authorization: Bearer <token>
Response: { "message": "Contact deleted." }
Batch Add/Update Contacts

POST /contacts/batch
Headers: Authorization: Bearer <token>
Request:
[
  {
    "name": "string",
    "email": "string",
    "phone": "string",
    "address": "string",
    "timezone": "string"
  },
  ...
]
Response: { "message": "Batch processing completed." }
3.Date Validation
Implement validation using a library like Joi or Yup to ensure the following:
User inputs (registration, contact creation, etc.) are validated.
Email uniqueness is enforced for both users and contacts.
4.Data Time Handling
Store all timestamps in UTC.
When retrieving contacts, convert timestamps to the user’s specified timezone using a library like Moment.js or date-fns.
Implement date range filtering:
GET /contacts?startDate=YYYY-MM-DD&endDate=YYYY-MM-DD
Response: [ { "id": "uuid", ... } ]
5.File Handling
Endpoints
Upload CSV/Excel File

POST /contacts/upload
Headers: Authorization: Bearer <token>
Form Data: file (binary)
Response: { "message": "File processed successfully." }
Download Contacts as CSV/Excel

GET /contacts/download
Headers: Authorization: Bearer <token>
Response: Binary file download (CSV/Excel).
6.Data Base
Schema Design
Users Table

id: UUID
username: STRING
email: STRING UNIQUE
passwordHash: STRING
createdAt: TIMESTAMP
updatedAt: TIMESTAMP
Contacts Table

id: UUID
userId: UUID (FK to Users)
name: STRING
email: STRING UNIQUE
phone: STRING
address: STRING
timezone: STRING
isDeleted: BOOLEAN (default false)
createdAt: TIMESTAMP
updatedAt: TIMESTAMP
Implement transactions for batch processing and file uploads to ensure data integrity.
7.Security
Rate Limiting: Implement rate limiting on sensitive endpoints (e.g., /auth/login, /auth/register) to prevent abuse.
Password Security: Use a hashing library like bcrypt to securely store passwords.
Input Sanitization: Ensure that all user inputs are sanitized to avoid SQL injection and other attacks.
Example Workflow
User Registration: User sends a POST request to /auth/register. A verification email is sent.
Email Verification: User clicks on the link in the email, which hits the /auth/verify/{token} endpoint.
User Login: User logs in via /auth/login, receiving a JWT.
Add Contact: User adds a contact via /contacts.
Batch Processing: User uploads a CSV file of contacts via /contacts/upload.
Retrieve Contacts: User retrieves contacts with filtering and sorting options via /contacts.
Download Contacts: User downloads all contacts as a CSV file via /contacts/download.
