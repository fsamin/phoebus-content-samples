---
title: "Building a Complete REST API"
type: lesson
estimated_duration: "30m"
---

# Building a Complete REST API

Let's put everything together and build a complete CRUD API for a "tasks" resource.

## Project Structure

```
taskapi/
├── go.mod
├── main.go
├── handler.go
├── model.go
├── store.go
└── middleware.go
```

## Model (`model.go`)

```go
package main

import "time"

type Task struct {
    ID          string     `json:"id"`
    Title       string     `json:"title"`
    Description string     `json:"description,omitempty"`
    Status      string     `json:"status"` // "todo", "in_progress", "done"
    CreatedAt   time.Time  `json:"created_at"`
    UpdatedAt   time.Time  `json:"updated_at"`
}

type CreateTaskRequest struct {
    Title       string `json:"title"`
    Description string `json:"description"`
}

type UpdateTaskRequest struct {
    Title       *string `json:"title"`       // pointer = optional
    Description *string `json:"description"`
    Status      *string `json:"status"`
}
```

## In-Memory Store (`store.go`)

```go
package main

import (
    "fmt"
    "sync"
    "time"

    "github.com/google/uuid"
)

type TaskStore struct {
    mu    sync.RWMutex
    tasks map[string]Task
}

func NewTaskStore() *TaskStore {
    return &TaskStore{tasks: make(map[string]Task)}
}

func (s *TaskStore) Create(req CreateTaskRequest) Task {
    s.mu.Lock()
    defer s.mu.Unlock()

    task := Task{
        ID:          uuid.NewString(),
        Title:       req.Title,
        Description: req.Description,
        Status:      "todo",
        CreatedAt:   time.Now(),
        UpdatedAt:   time.Now(),
    }
    s.tasks[task.ID] = task
    return task
}

func (s *TaskStore) List() []Task {
    s.mu.RLock()
    defer s.mu.RUnlock()

    tasks := make([]Task, 0, len(s.tasks))
    for _, t := range s.tasks {
        tasks = append(tasks, t)
    }
    return tasks
}

func (s *TaskStore) Get(id string) (Task, error) {
    s.mu.RLock()
    defer s.mu.RUnlock()

    task, ok := s.tasks[id]
    if !ok {
        return Task{}, fmt.Errorf("task %s not found", id)
    }
    return task, nil
}

func (s *TaskStore) Update(id string, req UpdateTaskRequest) (Task, error) {
    s.mu.Lock()
    defer s.mu.Unlock()

    task, ok := s.tasks[id]
    if !ok {
        return Task{}, fmt.Errorf("task %s not found", id)
    }

    if req.Title != nil {
        task.Title = *req.Title
    }
    if req.Description != nil {
        task.Description = *req.Description
    }
    if req.Status != nil {
        task.Status = *req.Status
    }
    task.UpdatedAt = time.Now()
    s.tasks[id] = task
    return task, nil
}

func (s *TaskStore) Delete(id string) error {
    s.mu.Lock()
    defer s.mu.Unlock()

    if _, ok := s.tasks[id]; !ok {
        return fmt.Errorf("task %s not found", id)
    }
    delete(s.tasks, id)
    return nil
}
```

## Handlers (`handler.go`)

```go
package main

import (
    "encoding/json"
    "net/http"
)

type API struct {
    store *TaskStore
}

func (a *API) ListTasks(w http.ResponseWriter, r *http.Request) {
    respondJSON(w, http.StatusOK, a.store.List())
}

func (a *API) CreateTask(w http.ResponseWriter, r *http.Request) {
    var req CreateTaskRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        respondError(w, http.StatusBadRequest, "invalid JSON")
        return
    }
    if req.Title == "" {
        respondError(w, http.StatusBadRequest, "title is required")
        return
    }
    task := a.store.Create(req)
    respondJSON(w, http.StatusCreated, task)
}

func (a *API) GetTask(w http.ResponseWriter, r *http.Request) {
    task, err := a.store.Get(r.PathValue("id"))
    if err != nil {
        respondError(w, http.StatusNotFound, err.Error())
        return
    }
    respondJSON(w, http.StatusOK, task)
}

func (a *API) UpdateTask(w http.ResponseWriter, r *http.Request) {
    var req UpdateTaskRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        respondError(w, http.StatusBadRequest, "invalid JSON")
        return
    }
    task, err := a.store.Update(r.PathValue("id"), req)
    if err != nil {
        respondError(w, http.StatusNotFound, err.Error())
        return
    }
    respondJSON(w, http.StatusOK, task)
}

func (a *API) DeleteTask(w http.ResponseWriter, r *http.Request) {
    if err := a.store.Delete(r.PathValue("id")); err != nil {
        respondError(w, http.StatusNotFound, err.Error())
        return
    }
    w.WriteHeader(http.StatusNoContent)
}
```

## Wiring (`main.go`)

```go
package main

import (
    "log"
    "net/http"
)

func main() {
    store := NewTaskStore()
    api := &API{store: store}

    mux := http.NewServeMux()
    mux.HandleFunc("GET /api/tasks", api.ListTasks)
    mux.HandleFunc("POST /api/tasks", api.CreateTask)
    mux.HandleFunc("GET /api/tasks/{id}", api.GetTask)
    mux.HandleFunc("PUT /api/tasks/{id}", api.UpdateTask)
    mux.HandleFunc("DELETE /api/tasks/{id}", api.DeleteTask)

    handler := recoveryMiddleware(loggingMiddleware(mux))

    log.Println("Task API listening on :8080")
    log.Fatal(http.ListenAndServe(":8080", handler))
}
```

## Testing with cURL

```bash
# Create a task
curl -s -X POST http://localhost:8080/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Learn Go","description":"Complete the Go track"}' | jq

# List tasks
curl -s http://localhost:8080/api/tasks | jq

# Update status
curl -s -X PUT http://localhost:8080/api/tasks/{id} \
  -H "Content-Type: application/json" \
  -d '{"status":"in_progress"}' | jq

# Delete
curl -s -X DELETE http://localhost:8080/api/tasks/{id}
```
