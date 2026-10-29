# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Sign up for activities and unregister participants as a teacher
- View activities and participant lists without logging in
- Protect enrollment changes with teacher login

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Configure teacher credentials as environment variables and start the application. Enter the password when prompted, and do not commit credentials to the repository.

   ```
   cd src
   export TEACHER_USERNAME=teacher
   read -rs -p "Teacher password: " TEACHER_PASSWORD; echo
   export TEACHER_PASSWORD COOKIE_SECURE=false
   uvicorn app:app --reload
   ```

   Set `COOKIE_SECURE=true` when serving the application over HTTPS. Teacher sessions are stored in memory and expire after eight hours.

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| GET    | `/auth/status`                                                    | Check whether the current browser has a teacher session             |
| POST   | `/auth/login`                                                     | Sign in with the configured teacher username and password           |
| POST   | `/auth/logout`                                                    | Sign out and invalidate the current teacher session                 |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Teacher-only student enrollment                                     |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Teacher-only student removal                                       |

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

Activity and participant data, as well as teacher sessions, are stored in memory. They are reset when the server restarts.
