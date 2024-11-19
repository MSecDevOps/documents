# Models Documentation

This document provides an overview of the database models used in the LMS project. Each model is described in detail, including its purpose, fields, relationships, and important methods.
## Userauths App
### User Model
* Represents a user in the system, supporting authentication and profile management.
* **Fields**:<br>
  * `username`: A unique identifier for the user. 
  * `email`: User's email address, used as the primary identifier.
  * `first_name` and last_name: Basic user information.
  * `phone_number`: User's contact number.
  * `bonus`: A field to store bonus points earned through referrals or activities.
  * `otp`: A one-time password for authentication-related actions.
  * `points`: Total points earned by the user for completing courses, modules, or assignments.
* **Key Methods**:
  * `update_last_activate()`: Updates the user's last activation timestamp.

### Profile Model
* Stores additional user information, such as profile image and social media links.
* **Fields**:
  * `image`: User's profile picture.
  * `full_name`: Full name of the user.
  * `country`: User's location.
  * `instagram`, `linkedin`, `github`: Links to the user's social media profiles.
* **Key Methods:**
  * `generate_referral_url()`: Generates a referral link for the user.


### Referral Model
* Manages referral data for users.
* **Fields**:
  * referral_code: A unique code generated for each user.
  * bonus: The bonus points associated with the referral.
* **Key Methods**: 
  * `generate_referral_url()`: Creates a referral link.


### UserActivityLog Model
* Tracks user activities, such as login, profile updates, and course enrollment.
* **Fields**:
  * `action`: The type of action performed by the user (e.g., "LOGIN").
  * `timestamp`: The date and time of the activity.
  * `details`: Additional information about the activity. 

    

## Payment App
### Cart Model
* Represents a user's shopping cart.
* **Fields**:
  * `course`: The course added to the cart.
  * `price` and total: Monetary values related to the cart.
  * `cart_id`: A unique identifier for the cart.
* Relationships:
  * Related to the Course and User models.


### CartOrder Model
* Represents an order created from a cart.
* **Fields**:
  * `sub_total`, total_fee, total: Financial details of the order.
  * `payment_status`: Indicates whether the payment is "Paid", "Processing", or "Failed".
  * `coupons`: Coupons applied to the order.

### Coupon Model
* Represents discount coupons applicable to courses.
* **Fields**:
  * `code`: A unique coupon code.
  * `discount`: The percentage discount offered.
  * `max_usage_count`: Maximum times the coupon can be used.


## Course App
### Course Model
* Represents an individual course available for enrollment.
* **Fields**:
  * `title`: The course name.
  * `description`: Details about the course content.
  * `price`: The cost of enrolling in the course.
  * `slug`: A unique slug for the course.
* **Relationships**:
  * Linked to `CourseCategory`, `Module`, and `User`.

### Module Model
* Represents a collection of lessons or videos in a course.
* **Fields**:
  * `title`: The name of the module.
  * `variant_id`: A unique identifier for the module.
  * `assignments`: Related assignments for the module.
* **Key Methods**: 
* `mark_as_completed()`: Marks the module as completed if all tasks are done.


### Video Model
* Represents a single video lesson within a module.
* Fields:
  * `title`: The name of the video.
  * `video_iframe`: The embed code for the video.
  * `duration`: The length of the video.
* Key Methods:
  * `mark_as_completed()`: Marks the video as completed by a user.


### HomeWorkAssignment Model
* Represents assignments linked to a video.
* **Fields**:
  * `description`: Instructions for the assignment.
  * `due_date`: The deadline for submission.
* Key Methods:
  * `mark_as_completed(`): Awards points when the assignment is completed.


### Test Model
* Represents a quiz linked to a video.
* **Fields**:
   * `title`: The name of the test.
   * `passing_score`: The minimum score required to pass.
   * `time_limit`: The maximum time allowed for the test.
* Key Methods:
  * `get_random_question()`: Fetches a random question from the test.


## Gamification and Notifications

### BonusHistory Model
* Tracks the bonus points earned by users over time.
  * `Fields`:
  * `amount`: The number of points awarded.
  * `created_at`: The date and time of the bonus.

### NotificationForHomeWork Model
* Stores notifications for users about their homework.
* Fields:
  * `message`: The notification message.
  * `is_read`: Indicates whether the notification has been read.