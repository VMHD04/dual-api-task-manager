Built to demonstrate production-grade REST and GraphQL APIs in Python — FastAPI · Strawberry · SQLAlchemy.


# Task Manager API 📋

A production-ready Task Manager API that exposes the same data through two interfaces:

- **REST API** — classic CRUD via HTTP verbs (`/api/v1/tasks`)
- **GraphQL API** — flexible queries and mutations (`/graphql`)

Built with **FastAPI**, **Strawberry GraphQL**, **SQLAlchemy**, and **SQLite**.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | [FastAPI](https://fastapi.tiangolo.com/) |
| GraphQL | [Strawberry](https://strawberry.rocks/) |
| ORM | [SQLAlchemy 2.0](https://docs.sqlalchemy.org/) |
| Validation | [Pydantic v2](https://docs.pydantic.dev/) |
| Database | SQLite (swappable to PostgreSQL) |
| Testing | Pytest + TestClient |

---

## Getting Started

### 1. Clone and install

```bash
git clone https://github.com/your-username/task-api.git
cd task-api
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Run the server

```bash
uvicorn app.main:app --reload
```

### 3. Explore the docs

| Interface | URL |
|---|---|
| Swagger UI (REST) | http://localhost:8000/docs |
| ReDoc (REST) | http://localhost:8000/redoc |
| GraphiQL (GraphQL) | http://localhost:8000/graphql |

---

## REST API Reference

### Endpoints

```
POST   /api/v1/tasks/              Create a task
GET    /api/v1/tasks/              List tasks (with filters)
GET    /api/v1/tasks/{id}          Get a task by ID
PATCH  /api/v1/tasks/{id}          Update a task
DELETE /api/v1/tasks/{id}          Delete a task
POST   /api/v1/tasks/bulk/complete Mark multiple tasks as done
GET    /health                     Health check
```

### Examples

**Create a task**
```bash
curl -X POST http://localhost:8000/api/v1/tasks/ \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn GraphQL", "priority": "high"}'
```

**List tasks filtered by priority**
```bash
curl "http://localhost:8000/api/v1/tasks/?priority=high&completed=false"
```

**Update a task**
```bash
curl -X PATCH http://localhost:8000/api/v1/tasks/1 \
  -H "Content-Type: application/json" \
  -d '{"completed": true}'
```

**Bulk complete**
```bash
curl -X POST http://localhost:8000/api/v1/tasks/bulk/complete \
  -H "Content-Type: application/json" \
  -d '[1, 2, 3]'
```

---

## GraphQL Reference

Access the interactive GraphiQL playground at `http://localhost:8000/graphql`.

### Queries

```graphql
# List all tasks
query {
  tasks {
    id
    title
    priority
    completed
    createdAt
  }
}

# Filter tasks
query {
  tasks(completed: false, priority: HIGH) {
    id
    title
  }
}

# Single task
query {
  task(taskId: 1) {
    id
    title
    description
    completed
  }
}

# Stats
query {
  stats
}
```

### Mutations

```graphql
# Create
mutation {
  createTask(input: { title: "New task", priority: HIGH }) {
    id
    title
    priority
  }
}

# Update
mutation {
  updateTask(taskId: 1, input: { completed: true, title: "Done!" }) {
    id
    title
    completed
  }
}

# Delete
mutation {
  deleteTask(taskId: 1) {
    success
    message
  }
}

# Bulk complete
mutation {
  bulkComplete(taskIds: [1, 2, 3]) {
    updatedCount
    taskIds
  }
}
```

---

## Running Tests

```bash
pytest tests/ -v
```

Expected output:
```
tests/test_api.py::TestRestCreate::test_create_task         PASSED
tests/test_api.py::TestRestCreate::test_create_task_missing_title PASSED
tests/test_api.py::TestRestRead::test_list_tasks_empty      PASSED
...
tests/test_api.py::TestGraphQL::test_create_task_mutation   PASSED
tests/test_api.py::TestGraphQL::test_query_tasks            PASSED
...
25 passed in X.XXs
```

---

## Project Structure

```
task-api/
├── app/
│   ├── main.py          # FastAPI app, routers, lifespan
│   ├── database.py      # SQLAlchemy engine & session
│   ├── models.py        # ORM models (Task, Priority)
│   ├── schemas.py       # Pydantic request/response schemas
│   ├── rest/
│   │   └── routes.py    # REST CRUD endpoints
│   └── graphql/
│       └── schema.py    # Strawberry types, queries & mutations
├── tests/
│   └── test_api.py      # Full test suite (REST + GraphQL)
├── requirements.txt
├── .env.example
└── .gitignore
```

---

## Switching to PostgreSQL

Update `DATABASE_URL` in your `.env`:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/taskdb
```

No other changes needed — SQLAlchemy handles the rest.

---

## License

MIT
