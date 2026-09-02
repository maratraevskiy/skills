# Tududi API use

Treat the target instance's Swagger UI at `<base_url>/api` as the API contract:
Tududi versions can differ. Confirm the task retrieval and update routes and
their request schema there before the first write to an installation.

The current public integration examples use bearer authentication and expose
task listing and creation at:

```text
GET  <base_url>/api/v1/tasks
POST <base_url>/api/v1/task
Authorization: Bearer <token>
Content-Type: application/json
```

Use the returned task UID as the binding identifier. The Swagger operation for
the installed version is authoritative for retrieving or updating a bound task
and for assigning its project. Send the smallest payload that changes the
snapshot; retain unknown fields and re-read the task after writing.

Do not create, revoke, or delete API keys as part of a normal sync. Respect
rate-limit responses and retry only when the response provides a safe retry
time.
