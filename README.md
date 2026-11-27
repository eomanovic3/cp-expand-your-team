# Expand your team with Copilot cloud agent

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey eomanovic3!

Mona here. I'm done preparing your exercise. Hope you enjoy! 💚

Remember, it's self-paced so feel free to take a break! ☕️

[![](https://img.shields.io/badge/Go%20to%20Exercise-%E2%86%92-1f883d?style=for-the-badge&logo=github&labelColor=197935)](https://github.com/eomanovic3/cp-expand-your-team/issues/1)

## Mergington High School Activities

This project includes a website for exploring extracurricular activities at
Mergington High School. Students can see activity descriptions, schedules,
available places, and participants. Teachers can sign students up for activities
or remove their registrations. The activity list can be filtered by day, time,
and difficulty.

### Run locally

You need Python (the development container uses Python 3.13) and MongoDB running
on `localhost:27017`. In a GitHub Codespace, the development container installs
the Python dependencies and starts MongoDB automatically. For a local setup,
install and start MongoDB separately before starting the application.

From the repository root, install the dependencies and start the backend:

```bash
python -m pip install -r src/requirements.txt
python -m uvicorn src.app:app --reload --host 127.0.0.1 --port 8000
```

Open [http://localhost:8000](http://localhost:8000) to use the website. The
backend also serves the HTML, CSS, and JavaScript frontend, so there is no
separate frontend installation or server. Interactive API documentation is
available at [http://localhost:8000/docs](http://localhost:8000/docs).

The application stores data in the `mergington_high` MongoDB database. It adds
sample activities and teacher accounts when their respective collections are
empty; changes remain in the database across server restarts.

**Known limitation:** The seeded teacher passwords use a different hashing
method from the login endpoint, so the supplied accounts cannot currently log
in. The signup and unregister endpoints expect a teacher username that exists
in the database, but do not verify a password or session themselves. Treat this
as a local development application, not a production authentication system.

### API overview

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | `/activities` | List activities, optionally filtered as described below. |
| GET | `/activities/days` | List days that have scheduled activities. |
| POST | `/activities/{activity_name}/signup` | Add a student; supply `email` and `teacher_username` as query parameters. |
| POST | `/activities/{activity_name}/unregister` | Remove a student; supply `email` and `teacher_username` as query parameters. |
| POST | `/auth/login` | Check a teacher's `username` and `password`, supplied as query parameters. |
| GET | `/auth/check-session` | Look up a teacher by the `username` query parameter. |

`GET /activities` accepts these optional query parameters, which can be
combined:

- `day`: a scheduled day such as `Monday` (matching the stored capitalization).
- `start_time`: include activities starting at or after this time, in 24-hour
  `HH:MM` format.
- `end_time`: include activities ending at or before this time, in 24-hour
  `HH:MM` format.
- `difficulty`: `beginner`, `intermediate`, `advanced`, or `all`. Omitting
  this parameter returns activities at every level; `all` specifically
  returns activities without a difficulty field.

For example:

```text
/activities?day=Monday&start_time=15:00&end_time=17:00&difficulty=beginner
```

### Project structure

- `src/backend` contains the API routes and MongoDB setup and sample data.
- `src/static` contains the website's HTML, CSS, and JavaScript.
- `src/app.py` starts the API and serves the frontend.

### Tests and further development

There is currently no automated test suite in the repository. To check the
application manually, start it as described above, open the website, and try
the activity filters or the endpoints in the interactive API documentation.

See the [Development Guide](docs/how-to-develop.md) for VS Code debugging and
Codespaces instructions. Its in-memory storage note and signup example are
outdated; use the MongoDB and API details above instead.
