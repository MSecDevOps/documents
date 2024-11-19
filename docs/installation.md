# Installation Guide

This guide will take you through the setup process for the MSECURITY LMS project, from setting up the environment to running the project. Let’s go step-by-step to ensure everything is correctly installed and configured.

## Prerequisites

Before getting started, make sure you have the following installed:


1. **Python**: (version 3.8 or higher)
2. **PostgreSQL**: for database management
3. **Redis Server**: for task queue management with Celery
4. **Git**:  to clone the repository
5. **Virtualenv**: for managing a clean Python environment
6. **MkDocs**: if you plan to build the project documentation

## 1.Clone the Project
Start by cloning the project repository

```bash
git clone https://github.com/MSecDevOps/msecurity_lms.git
```

```bash
cd msecurity_lms
```

## 2. Set Up the Virtual Environment
Setting up a virtual environment ensures the project’s dependencies do not interfere with other projects on your system.

```bash
python -m venv venv
```

```bash
source venv/bin/activate # On Windows, use venv\Scripts\activate
```  


## 3. Install the Required Packages
Now, install all the project dependencies using the `requirements.txt` file:

```bash
pip install -r requirements.txt
```

This will install Django, Django Rest Framework, Celery, Redis, Psycopg2 (for PostgreSQL), and any other required libraries.


## 4. Configure PostgreSQL Database

Make sure PostgreSQL is running on your system. Then, create a new database and user for the project:\n
1. Open the PostgreSQL command line or use a GUI tool like pgAdmin.
2. Run the following SQL commands:


```bash
CREATE DATABASE lms_db;
CREATE USER lms_user WITH PASSWORD 'your_password';
ALTER ROLE lms_user SET client_encoding TO 'utf8';
ALTER ROLE lms_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE lms_user SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE lms_db TO lms_user;
```
## 5. Set Up Redis for Celery
Redis will act as a message broker for Celery. Install Redis on your machine, or use a Redis cloud service.

To install Redis on Ubuntu:

```bash
sudo apt update
sudo apt install redis-server
```

After installation, start the Redis server:
```bash
sudo service redis-server start
```

In your Django settings, configure Celery to use Redis as the broker:

```bash title="settings.py" linenums="5" 
CELERY_BROKER_URL = 'redis://localhost:6379/0'
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'
```

## 6. Run Initial Database Migrations
To set up the initial database tables, run Django’s migrations:

```bash
python manage.py migrate
```

## 7. Create a Superuser
Create a superuser account for accessing the Django admin interface:

```bash
python manage.py createsuperuser
```
Follow the prompts to set up the admin credentials.

## 8. Start the Celery Worker
Celery is used to handle background tasks in the LMS, such as sending notifications. Start the Celery worker with the following command:

```bash
celery -A your_project_name worker -l info
```

Make sure to replace your_project_name with the name of your Django project directory.


## 9. Run the Django Development Server
Start the Django development server to see your project in action:

```bash
python manage.py runserver
```

Navigate to http://127.0.0.1:8000/ in your browser to access the LMS.

## 10. Set Up Documentation with MkDocs
If you’re using MkDocs to build project documentation, ensure MkDocs is installed in your environment:

```bash
pip install mkdocs
```


To serve your documentation locally:

```bash
mkdocs serve
```

Your documentation will be available at http://127.0.0.1:8000/.


## Summary

1. Clone the repository and set up a virtual environment.
2. Install dependencies from requirements.txt.
3. Set up a PostgreSQL database and update your Django settings.
4. Install Redis and configure it for Celery.
5. Run initial migrations and create a superuser.
6. Start the Celery worker and Django server to launch the LMS.
7. Serve documentation locally with MkDocs.
8. With everything set up, you should now have a fully operational LMS development environment!








