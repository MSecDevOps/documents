# API Reference for MSECURITY LMS

This document outlines all API endpoints for the MSECURITY LMS platform, detailing their purpose, HTTP methods, required payloads, and sample responses.


## 1. Authentication Endpoints
### 1.1 Token Obtain
**Endpoint**: `/user/token/`<br>
**Method**: `POST`<br>
**Description**: Generate access and refresh tokens for a user.<br>
**Request**:<br>
`{
  "email": "user@example.com",
  "password": "yourpassword"
}`<br>
**Response:**<br>
`{
  "access": "access_token",
  "refresh": "refresh_token"
}`

### 1.2 Token Refresh
**Endpoint:** `/user/token/refresh/`<br>
**Method:** `POST`<br>
**Description:** Refresh an access token using the refresh token.<br>
**Request:**<br>
`{
  "refresh": "refresh_token"
}`<br>
**Response:**<br>
`{
  "access": "new_access_token"
}`


### 1.3 Register User
**Endpoint:** `/user/register/`<br>
**Method:** `POST`<br>
**Description:** Register a new user account.<br>
**Request:**<br>
`{
  "first_name": "John",
  "last_name": "Doe",
  "email": "user@example.com",
  "phone_number": "1234567890",
  "device_type": "mobile",
  "device_identifier": "device_uuid"
}`<br>
**Response:**<br>
`{
  "message": "Registration successful. Check your email for login credentials."
}`


### 1.4 Login User
**Endpoint**: `/user/login/`<br>
**Method**: `POST`<br>
**Description**: Authenticate a user and retrieve tokens.<br>
**Request**:<br>
`{
  "email": "user@example.com",
  "password": "yourpassword",
  "device_identifier": "device_uuid"
}`<br>
**Response:**<br>
`{
  "message": "Login successful."
}`

### 1.5 Logout User
**Endpoint:** `/user/logout/`<br>
**Method**: `POST`<br>
**Description**: Log out the user and invalidate their token.<br>
**Request**:<br>
`{
  "refresh_token": "refresh_token"
}`<br>
Response:<br>
`{
  "message": "Successfully logged out."
}`

### 1.6 Password Reset
**Endpoint**: `/user/password-reset-email/`,<br>
**Method**: `POST`<br>
**Description**: Send a temporary password to the user's email.<br>
**Request**:<br>
`{
  "email": "user@example.com"
}`<br>
**Response:** <br>
`{
  "message": "Temporary password sent to your email."
}
`

## 2. Profile Management
### 2.1 Get Profile
**Endpoint**: `/student/profile/<int:user_id>/`<br>
**Method**: `GET`<br>
**Description**: Retrieve the logged-in user's profile.<br>
**Response**:<br>
`{
  "email": "user@example.com",
  "full_name": "John Doe",
  "image": "profile_image_url",
  "country": "USA",
  "about": "Student at LMS"
}`

### 2.2 Update Profile
**Endpoint**: `/student/profile/<int:user_id>/`<br>
**Method**: `PUT`<br>
**Description**: Update profile information.<br>
**Request**:<br>
`{
  "full_name": "John Doe Updated",
  "country": "Canada"
}`<br>
**Response**:<br>
`{
  "message": "Profile updated successfully."
}`

## 3. Course Management
### 3.1 List All Courses
**Endpoint**: `/course/course-list/`<br>
**Method**: `GET`<br>
**Description**: Retrieve all published courses.<br>
**Response**:<br>

`[
  {
    "id": 1,
    "title": "Cybersecurity Basics",
    "description": "Learn the basics of cybersecurity.",
    "price": 100.0,
    "image": "course_image_url"
  }
]`


### 3.2 Course Details
**Endpoint**: `/course/course-detail/<slug:slug>/`<br>
**Method**: `GET`<br>
**Description**: Retrieve detailed information about a specific course.<br>
**Response**:<br>

`{
  "title": "Cybersecurity Basics",
  "description": "Comprehensive course on cybersecurity.",
  "price": 100.0,
  "modules": [
    {
      "title": "Introduction",
      "videos": [
        {
          "title": "What is Cybersecurity?",
          "duration": "10:00"
        }
      ]
    }
  ]
}`


### 3.3 Enroll in a Course
**Endpoint**: `/course/<slug:slug>/enroll/`<br>
**Method**: `POST`<br>
**Description**: Enroll the logged-in user in a course.<br>
**Response**:<br>

`{
  "message": "Enrolled successfully.",
  "course": "Cybersecurity Basics"
}`


## 4. Payment Endpoints
### 4.1 Create Payment
**Endpoint**: /payme/create-payment/<br>
**Method**: POST<br>
**Description**: Initiate payment for a course.<br>
**Request**:<br>


`{
  "order_oid": "order_uuid"
}`<br>
**Response**:<br>
`{
  "payment_id": "payment_uuid",
  "payment_url": "http://payme.uz/payment_link"
}`

### 4.2 Check Payment Status
**Endpoint**: `/payme/check-payment/<str:payment_id>/`<br>
**Method**: `GET`<br>
**Description**: Retrieve the payment status.<br>
**Response**:<br>
`{
  "status": "completed",
  "details": "Payment successful"
}`



## 5. Gamification
### 5.1 Mark Video as Completed
**Endpoint**: /gamification/videos/complete/<br>
**Method**: POST<br>
**Description**: Mark a video as completed and reward points.<br>
**Request**:<br>

`{
  "user_id": 1,
  "video_id": 101
}`<br>
**Response**:<br>
`{
  "message": "Video marked as completed.",
  "user": {
    "id": 1,
    "points": 100
  }
}`


## 6. Quiz and Test
### 6.1 Retrieve Test
**Endpoint**: `/test/<int:video_id>`/<br>
**Method**: `GET`<br>
**Description**: Retrieve test details for a specific video.<br>
**Response**:<br>
`{
  "id": 1,
  "title": "Cybersecurity Quiz",
  "questions": [
    {
      "id": 11,
      "text": "What is a firewall?",
      "answers": [
        {"id": 101, "text": "Hardware", "is_correct": true},
        {"id": 102, "text": "Virus", "is_correct": false}
      ]
    }
  ]
}`

### 6.2 Submit Test
**Endpoint**: `/test/submit/`<br>
**Method**: `POST`<br>
**Description**: Submit a completed test.<br>
**Request**:<br>

`{
  "test": 1,
  "questions": [
    {"question_id": 11, "selected_answer_id": 101}
  ]
}`<br>
**Response**:<br>
`{
  "message": "Test successfully submitted",
  "score": 90
}
`

