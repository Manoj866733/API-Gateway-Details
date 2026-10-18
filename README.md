# Section 1: What is an API?

---

## What is an API? (Simple Definition)

An **API (Application Programming Interface)** is a contract that allows two software systems to talk to each other in a structured way.

👉 One system **requests** something  
👉 Another system **responds** with data or action

You don't care how it works internally, you only care:

- What **request** to send
- What **response** you'll get

---

### 📱 Swiggy / Zomato Analogy (Very Strong)

**Scenario:** You open Swiggy app and search "Biryani"

1. Mobile App (Client) sends a request
2. API receives the request
3. Backend Services fetch restaurant data
4. API sends response back to app

**Example Request:**
```
GET /restaurants?item=biryani&location=chennai
```

**Example Response:**
```
List of restaurants + price + rating
```

> 📌 **Teaching Tip**  
> Ask students: *"Is Swiggy app directly talking to the database?"*  
> ❌ No → **API is in between**

---

## Client → Server Communication

### Who is the Client?

- 🌐 Web browser (Chrome)
- 📱 Mobile app (Android / iOS)
- ⚙️ Another backend service
- 🖥️ CLI tools (curl, Postman)

### Who is the Server?

- Backend application
- Cloud service (AWS Lambda, EC2, ECS)
- Database access layer (indirectly)

> 📌 **Flow to draw on board:**
> ```
> Client → API → Backend → API → Client
> ```

---

## Request–Response Model

### What is a Request?

A request contains:

| Component | Description |
|-----------|-------------|
| **Method** | GET, POST, PUT, DELETE |
| **URL / Endpoint** | The resource address |
| **Headers** | Metadata (auth, content-type, etc.) |
| **Body** | Optional data payload |

**Example:**
```json
POST /order
{
  "item": "Chicken Biryani",
  "quantity": 2
}
```

---

### What is a Response?

A response contains:

| Component | Description |
|-----------|-------------|
| **Status Code** | 200 (OK), 400 (Bad Request), 500 (Server Error) |
| **Headers** | Metadata about the response |
| **Body** | Returned data |

**Example:**
```json
200 OK
{
  "orderId": 12345,
  "status": "Confirmed"
}
```

> 📌 **Important Teaching Line:**  
> *"API communication is always request → response. No magic, no guessing."*

---

# Section 2: HTTP Methods & Responses

---

## What are HTTP Methods?

HTTP Methods tell the server **what action** the client wants to perform on a resource.

> 📌 **Key line to say while teaching:**  
> *"HTTP method is the verb — it tells the server what to DO with the resource."*

Think of a resource like a **User Profile** in an app. You can:

| Action | HTTP Method | Meaning |
|--------|-------------|---------|
| Read data | `GET` | Fetch/retrieve |
| Create data | `POST` | Send new data |
| Update fully | `PUT` | Replace entire record |
| Update partially | `PATCH` | Modify specific fields |
| Remove data | `DELETE` | Delete a record |

---

## The 5 Core HTTP Methods

### 1. 🟢 GET — Fetch Data

Used to **retrieve** data from the server. No body is sent.

**Real-world:** View your Swiggy order history.

```http
GET /users/101
```

**Response:**
```json
200 OK
{
  "id": 101,
  "name": "Arjun",
  "email": "arjun@example.com",
  "city": "Chennai"
}
```

> ✅ GET is **safe** and **idempotent** — calling it multiple times won't change data.

---

### 2. 🔵 POST — Create Data

Used to **send new data** to the server. Body is required.

**Real-world:** Register a new user account.

```http
POST /users
Content-Type: application/json

{
  "name": "Arjun",
  "email": "arjun@example.com",
  "city": "Chennai"
}
```

**Response:**
```json
201 Created
{
  "id": 101,
  "message": "User created successfully"
}
```

> 📌 **Teaching Tip:** POST is **not idempotent** — calling it twice creates two records.

---

### 3. 🟡 PUT — Replace Data (Full Update)

Used to **completely replace** an existing resource.

**Real-world:** Update your entire delivery address profile.

```http
PUT /users/101
Content-Type: application/json

{
  "name": "Arjun Kumar",
  "email": "arjun.kumar@example.com",
  "city": "Bangalore"
}
```

**Response:**
```json
200 OK
{
  "message": "User updated successfully"
}
```

> ⚠️ **PUT replaces the whole record.** Fields not included will be wiped out.

---

### 4. 🟠 PATCH — Partial Update

Used to **update only specific fields** of an existing resource.

**Real-world:** Change only your city in the profile.

```http
PATCH /users/101
Content-Type: application/json

{
  "city": "Hyderabad"
}
```

**Response:**
```json
200 OK
{
  "message": "City updated successfully"
}
```

> 📌 **PUT vs PATCH:**  
> PUT = Replace the whole form. PATCH = Correct just one field.

---

### 5. 🔴 DELETE — Remove Data

Used to **delete** a resource from the server.

**Real-world:** Delete your saved address from Swiggy.

```http
DELETE /users/101
```

**Response:**
```json
200 OK
{
  "message": "User deleted successfully"
}
```

> ✅ DELETE is **idempotent** — deleting the same resource twice gives the same result (already gone).

---

## Quick Comparison Table

| Method | Has Body? | Idempotent? | Use Case |
|--------|-----------|-------------|----------|
| `GET` | ❌ No | ✅ Yes | Fetch a resource |
| `POST` | ✅ Yes | ❌ No | Create a new resource |
| `PUT` | ✅ Yes | ✅ Yes | Full replacement |
| `PATCH` | ✅ Yes | ✅ Yes | Partial update |
| `DELETE` | ❌ Optional | ✅ Yes | Remove a resource |

---

## HTTP Status Codes

Status codes tell the client **what happened** with the request. They are grouped into 5 families.

> 📌 **Key line to say while teaching:**  
> *"Status codes are how the server talks back — always check the code before reading the body."*

---

### 🟢 2xx — Success

| Code | Name | Meaning |
|------|------|---------|
| `200` | OK | Request succeeded |
| `201` | Created | New resource created (POST) |
| `204` | No Content | Success, but nothing to return (DELETE) |

**Example:**
```json
201 Created
{
  "id": 202,
  "status": "Account created"
}
```

---

### 🔵 3xx — Redirection

| Code | Name | Meaning |
|------|------|---------|
| `301` | Moved Permanently | Resource URL has changed forever |
| `302` | Found | Temporary redirect |
| `304` | Not Modified | Cached version is still valid |

> 📌 **Teaching Tip:** Browsers handle 3xx automatically — the user rarely notices.

---

### 🟡 4xx — Client Errors

These mean **the client made a mistake** in the request.

| Code | Name | Meaning |
|------|------|---------|
| `400` | Bad Request | Invalid request syntax or data |
| `401` | Unauthorized | Not logged in / no token |
| `403` | Forbidden | Logged in but no permission |
| `404` | Not Found | Resource doesn't exist |
| `422` | Unprocessable Entity | Validation failed |
| `429` | Too Many Requests | Rate limit exceeded |

**Example — 404:**
```json
404 Not Found
{
  "error": "User with id 999 not found"
}
```

**Example — 401:**
```json
401 Unauthorized
{
  "error": "Token missing or expired. Please login again."
}
```

> 📌 **401 vs 403:**  
> 401 = *"Who are you?"* (not authenticated)  
> 403 = *"I know who you are, but you can't do this."* (not authorized)

---

### 🔴 5xx — Server Errors

These mean **the server failed**, not the client.

| Code | Name | Meaning |
|------|------|---------|
| `500` | Internal Server Error | Generic server crash |
| `502` | Bad Gateway | Upstream server issue |
| `503` | Service Unavailable | Server overloaded or down |
| `504` | Gateway Timeout | Upstream server timed out |

**Example — 500:**
```json
500 Internal Server Error
{
  "error": "Something went wrong. Please try again later."
}
```

> 📌 **Teaching Tip:** Ask students:  
> 👉 *"If Swiggy shows 'Something went wrong', what status code is likely being returned?"*  
> ✅ Answer: **500 or 503**

---

## Full Request–Response Example (End to End)

**Scenario:** A user places a food order on Swiggy.

**Request:**
```http
POST /orders
Authorization: Bearer eyJhbGci...
Content-Type: application/json

{
  "userId": 101,
  "restaurantId": 55,
  "items": [
    { "name": "Chicken Biryani", "quantity": 2 },
    { "name": "Raita", "quantity": 1 }
  ],
  "deliveryAddress": "Anna Nagar, Chennai"
}
```

**Response:**
```json
201 Created
{
  "orderId": "ORD-2024-9981",
  "status": "Confirmed",
  "estimatedDelivery": "35 minutes",
  "totalAmount": 520
}
```

---

## Backend Code Examples

### 🐍 Python — FastAPI (Task Manager API)

Shows how all four HTTP methods (GET, POST, PUT, DELETE) are implemented in a real Python backend using FastAPI.

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

# 1. Our "Database" (a simple Python list)
tasks_db = [
    {"id": 1, "title": "Buy groceries", "completed": False},
    {"id": 2, "title": "Learn FastAPI", "completed": True}
]

# 2. Our Data Model (for validation)
class Task(BaseModel):
    id: int
    title: str
    completed: bool = False

# --- API ENDPOINTS ---

# GET: Fetch all tasks
@app.get("/tasks", response_model=List[dict])
def get_tasks():
    return tasks_db

# POST: Add a new task
@app.post("/tasks")
def create_task(task: Task):
    tasks_db.append(task.dict())
    return {"message": "Task created successfully", "task": task}

# PUT: Update an existing task by ID
@app.put("/tasks/{task_id}")
def update_task(task_id: int, updated_task: Task):
    for index, task in enumerate(tasks_db):
        if task["id"] == task_id:
            tasks_db[index] = updated_task.dict()
            return {"message": "Task updated"}
    raise HTTPException(status_code=404, detail="Task not found")

# DELETE: Remove a task
@app.delete("/tasks/{task_id}")
def delete_task(task_id: int):
    for index, task in enumerate(tasks_db):
        if task["id"] == task_id:
            del tasks_db[index]
            return {"message": "Task deleted"}
    raise HTTPException(status_code=404, detail="Task not found")
```

**What to observe:**
- Each `@app.get / .post / .put / .delete` decorator maps directly to an HTTP method
- `task_id` in the URL path (`/tasks/{task_id}`) is a **path parameter**
- `HTTPException(status_code=404)` is how the backend returns a `404 Not Found` response
- `Task(BaseModel)` is the **request body schema** — FastAPI validates incoming JSON against it automatically

---

### ☕ Java — Task Model (Data Class)

Shows how a backend data model is structured in Java — equivalent to the `Task(BaseModel)` in Python above.

```java
public class Task {
    private int id;
    private String title;
    private boolean completed;

    // Constructors
    public Task(int id, String title, boolean completed) {
        this.id = id;
        this.title = title;
        this.completed = completed;
    }

    // Getters and Setters (Java needs these to "see" the data)
    public int getId()              { return id; }
    public String getTitle()        { return title; }
    public boolean isCompleted()    { return completed; }
    public void setTitle(String title)          { this.title = title; }
    public void setCompleted(boolean completed) { this.completed = completed; }
}
```

**What to observe:**
- Fields are `private` — data is hidden and only accessible through getters/setters (encapsulation)
- `getId()`, `getTitle()`, `isCompleted()` → **Getters** — used when sending data back as a response
- `setTitle()`, `setCompleted()` → **Setters** — used when receiving and updating data from a request
- In Python (Pydantic), all of this is handled automatically — Java requires it to be written explicitly

> 📌 **Key line to say while teaching:**  
> *"In both Python and Java, the data model defines the shape of the request and response — the language syntax differs, the concept is identical."*

---

# Section 3: What is Amazon API Gateway?

---

## Simple Definition

**Amazon API Gateway** is a fully managed AWS service that lets you create, publish, secure, monitor, and scale APIs — without managing any infrastructure.

> 📌 **One-liner to remember:**  
> *"API Gateway is the front door for all backend services in AWS."*

---

## Fully Managed — What Does That Mean?

### You do NOT manage:

| Concern | Handled By |
|---------|-----------|
| Servers & OS patching | AWS |
| Scaling & load balancing | AWS |
| High availability across AZs | AWS |
| Failover | AWS |

### AWS automatically handles:
- Millions of concurrent requests
- Sudden traffic spikes
- Multi-AZ availability
- Auto scaling with zero configuration

> 📌 **Teaching line:**  
> *"You focus on business logic. AWS handles infrastructure headaches."*

### Real-World Scaling Example

| Event | Traffic Impact |
|-------|---------------|
| IPL Final | 100 → 1,000,000 requests in minutes |
| Big Billion Day (Flipkart) | Massive sudden surge |
| New feature launch | Unpredictable spike |

👉 API Gateway scales automatically — no manual intervention, no downtime.

---

## API Gateway as a Front Door

Clients **never directly access** backend services:

❌ Client → Lambda *(not allowed)*  
❌ Client → EC2 *(not allowed)*  
❌ Client → Database *(never)*

✅ The correct flow is always:

```
Client → API Gateway → Backend Service
```

### Why This Matters

| Capability | Benefit |
|------------|---------|
| Authentication | Only valid requests pass through |
| Throttling | Prevents backend overload |
| Validation | Bad requests rejected early |
| Monitoring | Centralized logging & metrics |
| Security | Backend never directly exposed |

### 🏢 Office Security Desk Analogy

| Office | API Gateway |
|--------|------------|
| Visitors can't enter work floors directly | Clients can't call backend directly |
| Security desk verifies ID | API Gateway validates auth tokens |
| Access allowed or denied | Requests approved or rejected |
| Visitor log maintained | All requests are logged |

> 📌 **Strong line:**  
> *"API Gateway protects backend services from the internet."*

---

## Why Not Call the Backend Directly?

| ❌ Without API Gateway | ✅ With API Gateway |
|----------------------|-------------------|
| No rate limiting | Throttling built-in |
| No authentication enforcement | Auth at the gateway layer |
| Hard to monitor across services | Centralized CloudWatch metrics |
| Tight coupling between client & backend | Loose coupling — backend can change freely |
| Every service exposed to internet | Single unified endpoint |

---

## Integrations with Backend Services

API Gateway can route requests to multiple backend types. Each integration has a specific use case.

---

### 🔹 API Gateway + AWS Lambda *(Most Common)*

```
Client → API Gateway → Lambda → Response
```

**Use Cases:**
- Serverless APIs
- Microservices
- Mobile & web backends
- Event-driven systems

**Example:**
```
POST /order  →  Lambda processes order  →  Returns orderId
```

> 📌 *"Lambda is the brain, API Gateway is the mouth."*

**Why this combination works well:**
- No servers to manage on either side
- Lambda scales in sync with API Gateway
- Pay only for actual invocations

---

### 🔹 API Gateway + EC2

```
Client → API Gateway → EC2 (HTTP endpoint)
```

**Use Cases:**
- Legacy applications migrating to AWS
- Monolithic backends that can't be broken into functions
- Custom frameworks not suited for containers or Lambda

**Example:**
- A Java Spring Boot app running on EC2
- API Gateway sits in front and handles auth, throttling, and routing before passing the request to EC2

---

### 🔹 API Gateway + ALB (Application Load Balancer)

```
Client → API Gateway → ALB → EC2 fleet
```

**Why ALB is added:**
- Multiple EC2 instances running behind the load balancer
- ALB distributes traffic across instances
- API Gateway handles the API layer; ALB handles traffic distribution

**Use Cases:**
- Microservices spread across multiple EC2 instances
- Blue/Green deployments
- Path-based routing (`/users` → Service A, `/orders` → Service B)

> 📌 *"API Gateway + ALB = API layer + traffic distribution."*

---

### 🔹 API Gateway + ECS (Containers)

```
Client → API Gateway → ALB → ECS Service → Containers
```

**Use Cases:**
- Containerized microservices (Docker-based)
- Teams deploying services independently
- Scalable backend services managed via ECS

**Real Example — Microservices split by domain:**

| Service | Route |
|---------|-------|
| Auth Service | `POST /auth/login` |
| Payment Service | `POST /payment/charge` |
| Inventory Service | `GET /inventory/check` |

Each service runs in its own ECS container. API Gateway routes requests to the correct one.

---

### 🔹 API Gateway + Step Functions

```
Client → API Gateway → Step Functions → Lambda → Lambda → Approval → Response
```

**When to use this:**
- Long-running workflows that span multiple steps
- Orchestrating several Lambda functions in sequence
- Processes that require human approval or conditional branching

**Use Cases:**
- E-commerce order processing (validate → charge → notify → ship)
- Approval workflows (submit → manager review → HR approval → done)
- Payment + notification pipelines

> 📌 *"API Gateway can trigger entire workflows, not just individual functions."*

---

## Architecture Overview

```
                    Client
                      │
              Amazon API Gateway
             /    |       |     \
         Lambda   ALB    EC2   Step Functions
                   |
                  ECS
               (Containers)
```

All client traffic enters through a **single API Gateway endpoint**. From there, requests are routed to the appropriate backend based on the path, method, or configuration.

---

## Why API Gateway is Central to AWS Architecture

| Feature | What It Gives You |
|---------|------------------|
| Single entry point | One URL for all backend services |
| Multi-backend support | Lambda, EC2, ECS, Step Functions |
| Security | IAM, Cognito, API keys, throttling |
| Observability | CloudWatch metrics, access logs, tracing |
| Auto scaling | Handles any traffic volume without configuration |

> 📌 **Final line:**  
> *"API Gateway is the traffic controller of AWS backend services."*

---

# Section 4: Creating Your First API (AWS Console Walkthrough)

---

## Goal

By the end of this hands-on, you will have a live API running on AWS and understand exactly how a request travels from client to backend and back.

```
Client → API Gateway → Lambda → Response
```

> *"I can create a real API in AWS."*

---

## Pre-Requisite — Lambda Backend

Before touching API Gateway, you need a working Lambda function as the backend.

**Function name:** `hello-api-lambda`

**Expected response:**
```json
{
  "message": "Hello from API Gateway"
}
```

> 📌 *"API Gateway does not process logic — it only routes traffic. Lambda is where the logic lives."*

---

## Step 1 — Open API Gateway

1. Login to **AWS Console**
2. Search for **API Gateway** in the top search bar
3. Click **API Gateway**

You will land on the API types screen showing:

| Type | When to Use |
|------|-------------|
| REST API | Advanced features, transformations, usage plans |
| HTTP API | Simple, fast, cost-effective — best for learning |
| WebSocket API | Real-time, bidirectional communication |

> 📌 *"This screen itself tells us AWS supports multiple API types — each built for a different purpose."*

---

## Step 2 — Create API

1. Click **Create API**
2. You will see the same three options: REST API, HTTP API, WebSocket API
3. Under **HTTP API** → Click **Build**

> 📌 *"We use HTTP API because it's faster to set up, cheaper to run, and covers everything needed at this stage."*

---

## Step 3 — Basic Setup & Integration

On the **Create an API** page:

### Connect to Backend

| Field | Value |
|-------|-------|
| Integration type | Lambda |
| Lambda function | `hello-api-lambda` |
| AWS Region | Your region (e.g. `us-east-1`) |

> 📌 *"This is the most important step — this is where API Gateway is wired to the backend."*

### API Details

| Field | Value |
|-------|-------|
| API name | `my-first-http-api` |
| Description | *(optional)* |
| All other settings | Leave as default |

Click **Next**

---

## Step 4 — Configure Routes

A **route** is the combination of an HTTP method and a URL path. It tells API Gateway which backend to call for a given request.

```
HTTP Method + Path = Route
GET /hello  →  Lambda (hello-api-lambda)
```

### Add the Route

| Field | Value |
|-------|-------|
| Method | `GET` |
| Resource path | `/hello` |
| Integration target | `hello-api-lambda` |

> 📌 *"Routes decide which request goes to which backend. One API can have many routes pointing to different backends."*

Click **Next**

---

## Step 5 — Configure Stages

**Stages** represent deployment environments (e.g. dev, staging, prod).

For HTTP API, AWS handles this simply:

| Setting | Value |
|---------|-------|
| Stage name | `$default` |
| Auto-deploy | ✅ Enabled |

> 📌 *"HTTP APIs auto-deploy every time you make a change. No manual deployment step needed — unlike REST APIs."*

Click **Next**

---

## Step 6 — Review & Create

Review your configuration before creating:

| Item | What to Check |
|------|--------------|
| API name | `my-first-http-api` |
| Integration | `hello-api-lambda` |
| Route | `GET /hello` |
| Stage | `$default` with auto-deploy on |

Click **Create**

✅ Your API is now live.

---

## Step 7 — Test Your API

After creation, AWS provides an **Invoke URL**:

```
https://abc123.execute-api.us-east-1.amazonaws.com/hello
```

### Test in Browser

1. Copy the Invoke URL
2. Paste it into your browser address bar
3. Press Enter

### Test in Postman

1. Open Postman
2. Set method to `GET`
3. Paste the Invoke URL
4. Click **Send**

### Test in Browser (Online — No Install Needed)

Use **ReqBin** to send API requests directly from your browser without installing any tool.

🔗 [https://reqbin.com](https://reqbin.com)

1. Open ReqBin in your browser
2. Set method to `GET`
3. Paste your Invoke URL
4. Click **Send**

### Expected Response

```json
{
  "message": "Hello from API Gateway"
}
```

> 📌 *"This response is coming from Lambda — not API Gateway. API Gateway only routed the request there."*

---

## Request Flow (End to End)

```
Browser / Postman
        ↓
Amazon API Gateway
  Route: GET /hello
        ↓
  Lambda Function
  hello-api-lambda
        ↓
  Response returned
        ↓
Back to Client
```

Every request follows this exact path. API Gateway never generates the response — it only receives, validates, routes, and returns.

---

## What You Just Built

| Component | Role |
|-----------|------|
| **API Gateway** | Received the HTTP request, matched the route, forwarded to Lambda |
| **Lambda** | Executed the logic, returned the JSON response |
| **Route `GET /hello`** | The rule that connected the request to the correct backend |
| **`$default` stage** | The live deployment environment, auto-updated on every change |

---

# Section 5: REST API – Resources, Methods & Hands-On

---

## What is a Resource?

A **resource** is a noun — it represents a thing or entity in your system that the API exposes.

| Resource URL | Represents |
|-------------|-----------|
| `/users` | All users in the system |
| `/users/101` | A specific user with ID 101 |
| `/orders` | All orders |
| `/orders/5001` | A specific order with ID 5001 |
| `/products` | All products |

> 📌 *"Resources are nouns, not verbs. You don't say `/getUser` or `/deleteOrder` — you say `/users` and let the HTTP method define the action."*

---

## What is a Method?

A **method** defines the action you want to perform on a resource. The same URL behaves differently depending on the method used.

| Method | Action |
|--------|--------|
| `GET` | Read / fetch data |
| `POST` | Create a new record |
| `PUT` | Update an existing record |
| `DELETE` | Remove a record |

> 📌 *"Same resource, different behavior based on method."*

---

## Resource + Method = API Operation

This is the core building block of every REST API. Combining a resource path with a method defines a complete operation.

```
GET    /users        →  Fetch all users
POST   /users        →  Create a new user
GET    /users/101    →  Fetch user with ID 101
PUT    /users/101    →  Update user with ID 101
DELETE /users/101    →  Delete user with ID 101
```

> 📌 *"REST APIs are designed around resources and HTTP methods — not actions or functions."*

---

## Method Hierarchy

Methods always belong to a resource. You cannot create a method in isolation — a resource must exist first.

```
/users
  ├── GET              →  Get all users
  ├── POST             →  Create a user
  └── /{userId}
        ├── GET        →  Get a specific user
        ├── PUT        →  Update a specific user
        └── DELETE     →  Delete a specific user
```

> 📌 *"You don't create a method first. You always create a resource first, then attach methods to it."*

This hierarchy means:
- `/users` and `/users/{userId}` are **two separate resources**
- Each resource has its own set of allowed methods
- `{userId}` is a **path parameter** — a dynamic value passed in the URL

---

## REST API Structure in AWS Console

When you create a REST API in API Gateway, the console displays resources as a tree. This is what a standard Users API looks like:

```
Resources
└── /
    └── /users
          ├── GET
          ├── POST
          └── /{userId}
                ├── GET
                ├── PUT
                └── DELETE
```

> 📌 *"This tree structure is why REST API is called resource-based. Every branch is a resource, every leaf is a method."*

---

## Resource vs Route — Key Difference

Students coming from HTTP API often confuse REST API resources with HTTP API routes. They serve the same purpose but are modelled differently.

| Concept | HTTP API | REST API |
|---------|----------|----------|
| Term used | Route | Resource + Method |
| Example | `GET /users` as one unit | `/users` resource with `GET` method attached |
| Structure | Flat list of routes | Tree of resources with methods |
| Path parameters | Defined inline: `/users/{id}` | Separate child resource: `/{userId}` |

---

## Full Example — Orders API

A complete resource and method design for an order management system:

```
/orders
  ├── GET              →  List all orders
  ├── POST             →  Place a new order
  └── /{orderId}
        ├── GET        →  Get order details
        ├── PUT        →  Update order status
        └── DELETE     →  Cancel an order
```

**Sample request and response:**

```http
GET /orders/5001
```

```json
200 OK
{
  "orderId": 5001,
  "item": "Chicken Biryani",
  "status": "Out for delivery",
  "total": 260
}
```

```http
DELETE /orders/5001
```

```json
200 OK
{
  "message": "Order 5001 has been cancelled"
}
```

---

---

## Hands-On — Pre-Requisite: Deploy the Lambda Backend

Before building the REST API in the console, deploy this Lambda function. It handles all five operations across `/users` and `/users/{userId}` so every method you wire up in API Gateway is immediately testable.

**Function name:** `users-api-lambda`

```python
import json

# In-memory mock database
users_db = {
    "101": {"id": "101", "name": "Arjun", "email": "arjun@example.com", "city": "Chennai"},
    "102": {"id": "102", "name": "Priya", "email": "priya@example.com", "city": "Bangalore"},
    "103": {"id": "103", "name": "Ravi",  "email": "ravi@example.com",  "city": "Hyderabad"}
}

def lambda_handler(event, context):
    method      = event.get("httpMethod", "")
    resource    = event.get("resource", "")   # e.g. /users  or  /users/{userId}
    path_params = event.get("pathParameters") or {}
    body        = json.loads(event.get("body") or "{}")

    user_id = path_params.get("userId")

    # ── GET /users ─────────────────────────────────────────
    if method == "GET" and resource == "/users":
        return response(200, list(users_db.values()))

    # ── POST /users ────────────────────────────────────────
    if method == "POST" and resource == "/users":
        new_id = str(len(users_db) + 1)
        body["id"] = new_id
        users_db[new_id] = body
        return response(201, {"message": "User created", "user": body})

    # ── GET /users/{userId} ────────────────────────────────
    if method == "GET" and resource == "/users/{userId}":
        user = users_db.get(user_id)
        if not user:
            return response(404, {"error": f"User {user_id} not found"})
        return response(200, user)

    # ── PUT /users/{userId} ────────────────────────────────
    if method == "PUT" and resource == "/users/{userId}":
        if user_id not in users_db:
            return response(404, {"error": f"User {user_id} not found"})
        body["id"] = user_id
        users_db[user_id] = body
        return response(200, {"message": "User updated", "user": body})

    # ── DELETE /users/{userId} ─────────────────────────────
    if method == "DELETE" and resource == "/users/{userId}":
        if user_id not in users_db:
            return response(404, {"error": f"User {user_id} not found"})
        deleted = users_db.pop(user_id)
        return response(200, {"message": f"User {user_id} deleted", "user": deleted})

    # ── Fallback (logs event to help debug) ───────────────
    print("Unmatched event:", json.dumps(event))
    return response(400, {"error": "Invalid route or method"})


def response(status_code, body):
    return {
        "statusCode": status_code,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps(body)
    }
```

### What Each Part Does

| Code Section | Purpose |
|-------------|---------|
| `users_db` | In-memory dictionary acting as a mock database — no DynamoDB needed for teaching |
| `event.get("httpMethod")` | Reads which HTTP method the client used (GET, POST, PUT, DELETE) |
| `event.get("resource")` | Returns the **route template** as defined in API Gateway — `/users` or `/users/{userId}`. This is the key fix: REST API's `resource` field is reliable for matching, unlike `path` which includes the stage prefix |
| `event.get("pathParameters")` | Extracts `{userId}` from the URL when a child resource is hit |
| `event.get("body")` | Reads the JSON request body sent by the client (POST / PUT) |
| `response()` helper | Builds a properly formatted API Gateway response every time |
| `print("Unmatched event")` | Logs the full event to CloudWatch if the fallback is hit — helps debug routing issues |

> 📌 *"Always use `event['resource']` for route matching in REST API — not `event['path']`. The `resource` field always returns the template like `/users/{userId}`, while `path` returns the actual URL like `/users/101` and can behave differently across stages."*

---

## AWS Console Walkthrough — Creating a REST API

Unlike HTTP API, REST API in API Gateway requires you to manually create resources, attach methods, configure integrations, and **explicitly deploy** before the API is accessible.

> 📌 *"If you forget deployment in REST API, your changes will not take effect. This is the most common mistake beginners make."*

### High-Level Steps

| Step | Action |
|------|--------|
| 1 | Create REST API |
| 2 | Create Resource (`/users`) |
| 3 | Create Method (`GET`) |
| 4 | Attach Integration (Lambda) |
| 5 | Deploy API to a Stage |
| 6 | Copy Invoke URL and test |

---

### Step 1 — Create the REST API

1. Open **API Gateway** in the AWS Console
2. Click **Create API**
3. Under **REST API** → Click **Build**
4. Choose:
   - Protocol: `REST`
   - Create new API: `New API`
   - API name: `my-rest-api`
   - Endpoint type: `Regional`
5. Click **Create API**

---

### Step 2 — Create a Resource

1. In the left panel, click **Resources**
2. Click **Create Resource**
3. Fill in:

| Field | Value |
|-------|-------|
| Resource name | `users` |
| Resource path | `/users` |
| Enable API Gateway CORS | *(optional for now)* |

4. Click **Create Resource**

You will now see `/users` appear in the resource tree under `/`.

---

### Step 3 — Create a Method

1. Select the `/users` resource
2. Click **Create Method**
3. Choose method: `GET`
4. Configure integration:

| Field | Value |
|-------|-------|
| Integration type | Lambda Function |
| Lambda Proxy Integration | ✅ **Enable this** |
| Lambda region | Your region (e.g. `us-east-1`) |
| Lambda function | `users-api-lambda` |

5. Click **Save**

Repeat this process to add `POST` on `/users`.

> ⚠️ **Lambda Proxy Integration — must be enabled on every method**
>
> Without this, API Gateway strips the request before sending it to Lambda. Your function will receive an empty `{}` event and won't be able to read the HTTP method, resource path, path parameters, or body.
>
> With it enabled, API Gateway forwards the **complete request** to Lambda exactly as received — and Lambda returns the full response object directly including the status code and headers.

> 📌 *"Each method on the same resource can point to a different Lambda function or the same one — your choice based on architecture."*

---

### Step 4 — Create a Child Resource (Path Parameter)

Path parameters like `{userId}` are modelled as **child resources** in REST API — not inline in the method.

1. Select the `/users` resource
2. Click **Create Resource**
3. Fill in:

| Field | Value |
|-------|-------|
| Resource name | `userId` |
| Resource path | `{userId}` |

4. Click **Create Resource**

You will now see `/{userId}` nested under `/users`.

> 📌 *"Curly braces in the resource path mean it is a path parameter — a dynamic value the client passes in the URL, like `/users/101` or `/users/205`."*

5. On `/{userId}`, add methods `GET`, `PUT`, and `DELETE` following the same process as Step 3.

Your final resource tree should look like:

```
/
└── /users
      ├── GET
      ├── POST
      └── /{userId}
            ├── GET
            ├── PUT
            └── DELETE
```

---

### Step 5 — Deploy the API

This step is **mandatory** in REST API. Nothing you configure is live until you deploy.

1. Click **Deploy API** (top of the Resources panel)
2. Fill in:

| Field | Value |
|-------|-------|
| Deployment stage | `[New Stage]` |
| Stage name | `dev` |
| Description | *(optional)* |

3. Click **Deploy**

> 📌 *"Every time you add a resource, add a method, or change an integration — you must redeploy. HTTP API auto-deploys; REST API does not."*

---

### Step 6 — Copy the Invoke URL and Test

After deployment, navigate to **Stages → dev** and copy the Invoke URL:

```
https://abc123.execute-api.us-east-1.amazonaws.com/dev
```

---

## Testing with ReqBin

🔗 [https://reqbin.com](https://reqbin.com)

Open ReqBin in your browser. For each test below, paste the full URL, set the method, add the body if required, and click **Send**.

---

### 1. GET /users — Fetch all users

| Field | Value |
|-------|-------|
| Method | `GET` |
| URL | `https://abc123.execute-api.us-east-1.amazonaws.com/dev/users` |
| Body | None |

**Expected Response:**
```json
200 OK
[
  { "id": "101", "name": "Arjun", "email": "arjun@example.com", "city": "Chennai"   },
  { "id": "102", "name": "Priya", "email": "priya@example.com", "city": "Bangalore" },
  { "id": "103", "name": "Ravi",  "email": "ravi@example.com",  "city": "Hyderabad" }
]
```

---

### 2. GET /users/{userId} — Fetch a specific user

| Field | Value |
|-------|-------|
| Method | `GET` |
| URL | `https://abc123.execute-api.us-east-1.amazonaws.com/dev/users/101` |
| Body | None |

**Expected Response:**
```json
200 OK
{
  "id": "101",
  "name": "Arjun",
  "email": "arjun@example.com",
  "city": "Chennai"
}
```

---

### 3. POST /users — Create a new user

| Field | Value |
|-------|-------|
| Method | `POST` |
| URL | `https://abc123.execute-api.us-east-1.amazonaws.com/dev/users` |
| Content-Type | `application/json` |

**Request Body:**
```json
{
  "name": "Meera",
  "email": "meera@example.com",
  "city": "Mumbai"
}
```

**Expected Response:**
```json
201 Created
{
  "message": "User created",
  "user": {
    "id": "4",
    "name": "Meera",
    "email": "meera@example.com",
    "city": "Mumbai"
  }
}
```

> 📌 In ReqBin — click the **Content** tab, select **JSON**, and paste the body there.

---

### 4. PUT /users/{userId} — Update a user

| Field | Value |
|-------|-------|
| Method | `PUT` |
| URL | `https://abc123.execute-api.us-east-1.amazonaws.com/dev/users/101` |
| Content-Type | `application/json` |

**Request Body:**
```json
{
  "name": "Arjun Kumar",
  "email": "arjun.kumar@example.com",
  "city": "Bangalore"
}
```

**Expected Response:**
```json
200 OK
{
  "message": "User updated",
  "user": {
    "id": "101",
    "name": "Arjun Kumar",
    "email": "arjun.kumar@example.com",
    "city": "Bangalore"
  }
}
```

> 📌 After a PUT, run `GET /users/101` again to confirm the change was applied.

---

### 5. DELETE /users/{userId} — Delete a user

| Field | Value |
|-------|-------|
| Method | `DELETE` |
| URL | `https://abc123.execute-api.us-east-1.amazonaws.com/dev/users/103` |
| Body | None |

**Expected Response:**
```json
200 OK
{
  "message": "User 103 deleted",
  "user": {
    "id": "103",
    "name": "Ravi",
    "email": "ravi@example.com",
    "city": "Hyderabad"
  }
}
```

> 📌 After DELETE, run `GET /users` to confirm user 103 is no longer in the list.

---

### 6. GET /users/{userId} — Non-existent user (Error case)

| Field | Value |
|-------|-------|
| Method | `GET` |
| URL | `https://abc123.execute-api.us-east-1.amazonaws.com/dev/users/999` |
| Body | None |

**Expected Response:**
```json
404 Not Found
{
  "error": "User 999 not found"
}
```

---

## Testing Checklist

| Operation | Method | Status Code | Verified |
|-----------|--------|-------------|----------|
| Fetch all users | `GET /users` | `200` | ☐ |
| Fetch one user | `GET /users/101` | `200` | ☐ |
| Create a user | `POST /users` | `201` | ☐ |
| Update a user | `PUT /users/101` | `200` | ☐ |
| Delete a user | `DELETE /users/103` | `200` | ☐ |
| User not found | `GET /users/999` | `404` | ☐ |

---

## Method Execution Flow

Every request through API Gateway passes through a fixed sequence of stages before reaching the backend and after receiving a response. This is unique to REST API — HTTP API does not have this pipeline.

```
Client
  ↓
Method Request        ←  Validates incoming request (headers, params, body)
  ↓
Integration Request   ←  Transforms & forwards request to backend
  ↓
Backend (Lambda)      ←  Executes business logic, returns result
  ↓
Integration Response  ←  Receives backend response, applies transformations
  ↓
Method Response       ←  Maps status codes, formats final response
  ↓
Client
```

| Stage | What Happens |
|-------|-------------|
| **Method Request** | API Gateway validates auth, required headers, query params, and request body schema |
| **Integration Request** | Request is mapped and forwarded to the backend — Lambda, EC2, HTTP endpoint, etc. |
| **Backend** | Lambda (or other backend) executes and returns a result |
| **Integration Response** | Backend response is received; transformations or error mappings are applied |
| **Method Response** | Final HTTP status code and response body are sent back to the client |

> 📌 *"API Gateway processes requests in multiple stages — this pipeline is what makes REST API powerful for transformations, validation, and fine-grained control over every request and response."*

---

## How the In-Memory Database Works in Lambda

### Cold Start vs Warm Start

When Lambda runs, it goes through two possible phases:

```
Cold Start                         Warm Start
─────────────────────────          ─────────────────────────
Container spins up                 Container already alive
users_db initialized     →  →  →   users_db from previous run
lambda_handler runs                lambda_handler runs
```

**Cold Start — DB resets:**
- Lambda spins up a brand new container
- All code outside `lambda_handler` runs again from scratch
- `users_db` is re-initialized with the original 3 users
- Any data added or deleted in a previous run is gone

**Warm Start — DB survives:**
- Lambda reuses the existing container
- Code outside `lambda_handler` is not re-executed
- `users_db` retains whatever state it had from the last invocation
- POST and DELETE changes persist across requests until the container is recycled

---

### Why This Happens

```python
# Runs ONCE per container lifecycle (Cold Start only)
users_db = {
    "101": {...},
    "102": {...},
    "103": {...}
}

# Runs on EVERY request
def lambda_handler(event, context):
    ...
```

Anything at the **module level** (outside the handler) is initialized once when the container starts and lives in memory until AWS recycles the container.

---

### Behaviour by Scenario

| Scenario | What Happens to `users_db` |
|----------|---------------------------|
| First request after deploy | Cold start — DB resets to 3 users |
| Second request immediately after | Warm start — DB has your changes |
| No requests for ~15 minutes | Container recycled — next request cold starts, DB resets |
| Lambda redeployed | New container always — DB always resets |
| High traffic (concurrent requests) | Multiple containers — each has its **own separate** `users_db` |

---

### The Concurrent Request Problem

This is the most important gotcha to understand:

```
Request 1 → Container A → users_db: Arjun, Priya, Ravi
Request 2 → Container B → users_db: Arjun, Priya, Ravi  (separate copy)

POST /users → Container A → Meera added
GET  /users → Container B → Meera not there!
```

Each container has its own **isolated copy** of `users_db`. There is no shared state between containers.

---

> 📌 *"This is exactly why real applications use a database like DynamoDB or RDS — state must live outside Lambda, not inside it. The in-memory DB works fine for teaching a single session, but in production it would lose data constantly."*

---

## REST vs HTTP vs WebSocket APIs

> 📌 *"Many people confuse these three. HTTP is the foundation, REST is a design style built on top of it, and WebSocket is a completely different communication model."*

---

### 1. HTTP — The Base Protocol

HTTP (HyperText Transfer Protocol) is the protocol that defines how data travels across the internet. REST and WebSocket both build on top of HTTP concepts.

| Method | Purpose | Real-World Analogy |
|--------|---------|-------------------|
| `GET` | Retrieve data from a server | Reading a book from a library |
| `POST` | Send new data to create something | Submitting a job application |
| `PUT` | Fully replace an existing resource | Replacing an old appliance with a new one |
| `PATCH` | Partially update a resource | Fixing a leak in a pipe, not replacing the whole pipe |
| `DELETE` | Remove a resource | Shredding a document |

---

### 2. REST API — Architecture Style Built on HTTP

REST (Representational State Transfer) is not a protocol — it is an **architectural style** that uses HTTP in a structured, predictable way.

**REST Principles:**
- URLs represent **resources** (nouns, not verbs)
- HTTP methods define the **action**
- Every request is **stateless** — the server remembers nothing between calls
- Responses are typically JSON

```
GET    /users        →  Fetch all users
POST   /users        →  Create a user
GET    /users/10     →  Fetch user 10
DELETE /users/10     →  Delete user 10
```

**Used by:** Swiggy, Zomato, Amazon, and virtually every backend API in production today.

---

### 3. WebSocket API — Persistent Real-Time Connection

WebSocket is a different communication model entirely. Unlike REST which opens and closes a connection per request, WebSocket keeps the connection **alive** so the server can push data to the client at any time without the client asking.

**Examples:**
- Live chat (WhatsApp Web, Slack)
- Stock price updates
- Live order tracking
- Multiplayer games
- Live sports scores

> 📌 *"REST is like sending letters — you send one, you wait for a reply. WebSocket is like a phone call — both sides can talk at any time."*

---

### REST vs WebSocket — Side by Side

| Feature | REST API | WebSocket API |
|---------|----------|---------------|
| Communication | Request → Response | Bi-directional |
| Connection | Opens and closes per request | Persistent — stays open |
| Real-time capable | ❌ No | ✅ Yes |
| Server can push data | ❌ No — client must ask | ✅ Yes — anytime |
| Complexity | Simple | More complex |
| Best for | CRUD operations | Live updates, chat, games |
| Example | Place a food order | Track the delivery in real time |

---

### How All Three Relate

```
HTTP Protocol
    │
    ├── REST API       →  Resource-based, stateless, request-response
    │                     Used for: CRUD, standard backend APIs
    │
    └── WebSocket API  →  Persistent connection, bi-directional
                          Used for: real-time, live data, chat
```

REST and WebSocket are both built on HTTP but serve fundamentally different purposes. Most applications use **REST for data operations** and **WebSocket only where real-time push is needed**.

---

## Why APIs Are Needed

### Without APIs

| Problem | Impact |
|---------|--------|
| Frontend tightly coupled to backend | Any backend change breaks the frontend |
| No standard interface | Mobile, web, and third-party can't share the same backend |
| No security layer | Backend exposed directly to the internet |
| No scalability | Hard to scale individual components independently |

### With APIs

| Benefit | Impact |
|---------|--------|
| Frontend and backend are independent | Teams can build and deploy separately |
| Mobile + Web + third-party reuse the same API | One backend serves all clients |
| Secure access | Auth, throttling, and validation enforced at the API layer |
| Easy scaling | API Gateway scales automatically — backend scales independently |

> 📌 *"APIs are the backbone of modern cloud applications. API Gateway exists to manage them at scale."*

---

# Section 6: API Integrations (Backend Connectivity)

---

## Goal

By the end of this section, students should understand:
- How API Gateway forwards requests to the backend
- What data is passed between API Gateway and the backend
- Who controls the shape of the request and response

> 📌 *"Integration = how API Gateway connects to and communicates with a backend service."*

---

## Integration Types

API Gateway supports four integration types. Each one determines where the request goes after API Gateway receives it.

---

### 1. Lambda Integration *(Most Used)*

API Gateway invokes a Lambda function directly.

```
Client → API Gateway → Lambda → Response
```

**Use Cases:**
- Serverless APIs
- Microservices
- Event-driven backends

**Console Configuration:**

| Field | Value |
|-------|-------|
| Integration type | Lambda Function |
| Lambda Proxy Integration | ✅ Enable (recommended) |
| Region | Your AWS region |
| Lambda function | Your function name |

> 📌 *"API Gateway becomes an event source for Lambda — just like S3 or DynamoDB triggers, but triggered by an HTTP request."*

---

### 2. HTTP Integration

API Gateway forwards the request to any HTTP endpoint — whether that's an EC2 instance, an ALB, or a completely external third-party API.

```
Client → API Gateway → HTTP Backend
```

**Use Cases:**
- Legacy applications running on EC2
- On-premises APIs exposed via a public endpoint
- Third-party services

**Example:**
```
GET /weather  →  https://api.weather.com/data
```

API Gateway receives the client request and proxies it forward to the external URL. The response comes back through API Gateway to the client.

---

### 3. Mock Integration *(Interview Favourite)*

API Gateway returns a response directly — no backend is called at all.

**Use Cases:**
- API testing and prototyping before the backend is ready
- Frontend development against a fixed contract
- CORS preflight (`OPTIONS`) responses

> 📌 *"Mock integration is API Gateway talking to itself — useful when you want to define the API contract before building the backend."*

---

### 4. AWS Service Integration

API Gateway calls an AWS service directly — no Lambda in between.

```
Client → API Gateway → AWS Service
```

**Supported Services:**

| Service | Example Use Case |
|---------|-----------------|
| DynamoDB | `PUT /orders` → write directly to a DynamoDB table |
| S3 | `GET /files/{key}` → fetch an object from S3 |
| SQS | `POST /messages` → push a message to a queue |
| SNS | `POST /notify` → trigger a notification |
| Step Functions | `POST /workflow` → start a long-running workflow |

> 📌 *"No Lambda needed. API Gateway acts as an AWS client — fewer moving parts, lower latency, lower cost."*

---

### Integration Types — Summary

| Integration | Backend | Best For |
|-------------|---------|---------|
| Lambda | Lambda function | Serverless APIs, microservices |
| HTTP | EC2, ALB, external URL | Legacy apps, third-party APIs |
| Mock | None — API Gateway itself | Testing, prototyping, CORS |
| AWS Service | S3, DynamoDB, SQS, SNS | Direct AWS service access without Lambda |

---

## Lambda Proxy vs Non-Proxy Integration

This is one of the most important distinctions in API Gateway — and a frequent interview topic.

---

### Lambda Proxy Integration

API Gateway forwards the **entire HTTP request** to Lambda as-is without any modification. Lambda is fully responsible for reading the request and building the complete HTTP response.

```
Client Request → API Gateway → Lambda (full event) → Lambda builds response → Client
```

**Event structure Lambda receives:**
```json
{
  "resource": "/users/{userId}",
  "path": "/users/10",
  "httpMethod": "GET",
  "headers": {
    "Content-Type": "application/json"
  },
  "queryStringParameters": {
    "active": "true"
  },
  "pathParameters": {
    "userId": "10"
  },
  "body": null
}
```

> 📌 *"Everything about the HTTP request is inside the event — method, path, headers, query params, body, path parameters. Lambda has full context."*

**Response Lambda must return:**
```python
return {
    "statusCode": 200,
    "headers": {
        "Content-Type": "application/json"
    },
    "body": json.dumps({"message": "Success"})
}
```

> ⚠️ **Key Rule:** `body` must always be a **string** — use `json.dumps()` to serialize it. Returning a dict directly will cause API Gateway to reject the response.

---

### Non-Proxy Integration

API Gateway **controls the request and response format** using mapping templates. Lambda receives only what API Gateway explicitly maps — not the full event.

```
Client Request → API Gateway (transforms request) → Lambda (shaped event) → API Gateway (transforms response) → Client
```

**Event Lambda receives (only what was mapped):**
```json
{
  "userId": "10"
}
```

Lambda returns data, and API Gateway applies a response mapping template before sending it back to the client.

---

### Proxy vs Non-Proxy — Comparison

| Feature | Proxy Integration | Non-Proxy Integration |
|---------|------------------|-----------------------|
| Request control | Lambda | API Gateway |
| Response control | Lambda | API Gateway |
| Mapping templates | ❌ Not used | ✅ Required |
| Simplicity | High — less configuration | Medium — templates needed |
| Flexibility | High | Very High |
| Recommended for | Most use cases | Advanced transformation needs |

> 📌 *"Proxy integration is the default choice for almost everything. Use non-proxy only when strict request/response transformation is required and the backend cannot be changed."*

---

### When to Use Each

**Use Proxy Integration when:**
- Building serverless APIs or microservices
- You want Lambda to own the full request/response logic
- Faster development and iteration is the priority
- Starting a new project from scratch

**Use Non-Proxy Integration when:**
- Connecting to a legacy backend that expects a specific input format
- A strict API contract must be enforced at the gateway level
- The backend cannot be modified to handle the full proxy event
- Complex request or response transformation is needed before the backend sees the data

---

## Mapping Templates (Advanced — Hands-On)

> ⚠️ **Mapping Templates only work with Non-Proxy Integration.** If Lambda Proxy Integration is enabled, API Gateway skips all templates and forwards the raw request directly.

---

### What is a Mapping Template?

A mapping template is a script written in **VTL (Velocity Template Language)** that tells API Gateway how to transform data at two points:

```
Client Request  →  [Request Mapping Template]  →  Backend (Lambda)
Backend Result  →  [Response Mapping Template] →  Client
```

This is useful when the client sends data in one shape, the backend expects it in another, and you cannot or do not want to change either side.

> 📌 *"Mapping templates let API Gateway act as a translator between the client and the backend."*

---

### Hands-On — Request Mapping Template

**Scenario:**

The client calls:
```
GET /users/10
```

But the Lambda function expects:
```json
{
  "user_id": "10"
}
```

Without a mapping template, Lambda would receive the full proxy event or nothing useful at all. With a request mapping template, API Gateway reshapes the incoming request into exactly what Lambda expects.

---

#### Step 1 — Create the Lambda Function

Deploy this Lambda function first. It expects `user_id` in the event body — not `pathParameters`.

**Function name:** `mapping-demo-lambda`

```python
import json

users_db = {
    "10": {"id": "10", "name": "Mitran",  "role": "Trainer"},
    "11": {"id": "11", "name": "Arjun",   "role": "Student"},
    "12": {"id": "12", "name": "Priya",   "role": "Student"}
}

def lambda_handler(event, context):
    user_id = str(event.get("user_id", ""))

    if not user_id:
        return {"statusCode": 400, "body": json.dumps({"error": "user_id missing"})}

    user = users_db.get(user_id)
    if not user:
        return {"statusCode": 404, "body": json.dumps({"error": f"User {user_id} not found"})}

    return {"statusCode": 200, "body": json.dumps(user)}
```

> 📌 *"Notice this Lambda reads `event.get('user_id')` — not `event['pathParameters']`. This is what makes it a Non-Proxy integration. The mapping template is responsible for shaping the event."*

---

#### Step 2 — Configure the REST API in Console

1. Go to **API Gateway → REST API**
2. Navigate to **Resources → /users/{userId} → GET method**
3. Click **Integration Request**
4. ❌ **Disable Lambda Proxy Integration** — uncheck the box
5. Set integration type to **Lambda Function**
6. Select region and enter `mapping-demo-lambda`
7. Click **Save**

---

#### Step 3 — Add the Request Mapping Template

1. Still in **Integration Request**, scroll down to **Mapping Templates**
2. Click **Add mapping template**
3. Set Content-Type: `application/json`
4. Paste the following VTL template:

```vtl
{
  "user_id": "$input.params('userId')"
}
```

5. Click **Save**

> 📌 *"`$input.params('userId')` reads the `{userId}` path parameter from the URL. So when the client calls `GET /users/10`, API Gateway extracts `10` and maps it to `user_id` before Lambda sees the request."*

---

#### Step 4 — Test the Request Mapping

Deploy the API and test:

```
GET https://abc123.execute-api.us-east-1.amazonaws.com/dev/users/10
```

**Lambda receives this event (shaped by the template):**
```json
{
  "user_id": "10"
}
```

**Expected Response:**
```json
200 OK
{
  "id": "10",
  "name": "Mitran",
  "role": "Trainer"
}
```

---

### Hands-On — Response Mapping Template

**Scenario:**

Lambda returns:
```json
{
  "id": "10",
  "name": "Mitran",
  "role": "Trainer"
}
```

But the client should only receive:
```json
{
  "userName": "Mitran"
}
```

The backend response has more fields than the client needs. The response mapping template filters and renames fields before the response reaches the client.

---

#### Step 1 — Add Method Response

1. Go to **GET method → Method Response**
2. Click **Add Response**
3. Enter status code: `200`
4. Click **Save**

---

#### Step 2 — Add Integration Response

1. Go to **GET method → Integration Response**
2. Click on the `200` status row to expand it
3. Scroll to **Mapping Templates**
4. Set Content-Type: `application/json`
5. Paste the following VTL template:

```vtl
{
  "userName": "$input.path('$.name')"
}
```

6. Click **Save**
7. **Deploy the API**

---

#### Step 3 — Test the Response Mapping

```
GET https://abc123.execute-api.us-east-1.amazonaws.com/dev/users/10
```

**Lambda returns:**
```json
{
  "id": "10",
  "name": "Mitran",
  "role": "Trainer"
}
```

**Client receives (after response template is applied):**
```json
{
  "userName": "Mitran"
}
```

> 📌 *"The backend response does not have to match what the client receives. The response mapping template is the filter in between."*

---

### VTL Syntax Reference

VTL (Velocity Template Language) is the scripting language used inside mapping templates. You don't need to learn all of VTL — these are the expressions used most commonly in API Gateway.

| Expression | What It Does | Example |
|------------|-------------|---------|
| `$input.params('key')` | Reads a path parameter or query string value | `$input.params('userId')` → `10` |
| `$input.body` | Returns the entire raw request body as a string | Useful for POST pass-through |
| `$input.path('$.key')` | Extracts a field from a JSON body using JSONPath | `$input.path('$.name')` → `"Mitran"` |
| `$input.path('$.key.nested')` | Reads nested JSON fields | `$input.path('$.address.city')` |
| `$context.requestId` | Unique ID for the request generated by API Gateway | Useful for logging and tracing |
| `$context.httpMethod` | The HTTP method of the current request | `GET`, `POST`, etc. |

> 📌 *"VTL is what makes Non-Proxy integration powerful — you can reshape any request or response without touching the backend code."*

---

### Request vs Response Mapping — Summary

| Template Type | Location in Console | What It Transforms | VTL reads from |
|--------------|--------------------|--------------------|----------------|
| Request Mapping | Integration Request → Mapping Templates | Client request → Lambda event | `$input.params()`, `$input.body` |
| Response Mapping | Integration Response → Mapping Templates | Lambda response → Client response | `$input.path('$.key')` |

---

### Common Mistakes to Avoid

| Mistake | What Happens | Fix |
|---------|-------------|-----|
| Lambda Proxy Integration left ON | Mapping templates are completely ignored | Disable proxy integration first |
| Forgetting to add Method Response | Integration Response has no status code to map to | Add `200` in Method Response before Integration Response |
| Not deploying after changes | Old template still in effect | Always redeploy after any template change |
| Using `$input.path()` on a non-JSON body | Template throws a parsing error | Ensure Lambda returns valid JSON |

---

# Section 7: Security & Authorization

---

## Goal

By the end of this section, students should clearly understand:

- **Who are you?** — Authentication
- **Are you allowed?** — Authorization
- **Where are you calling from?** — Resource Policies
- How API Gateway enforces all three layers

---

## Authentication vs Authorization (Foundation)

These two terms are often confused. They are different concepts that happen in sequence.

### 🔑 Authentication — Who Are You?

Authentication is **identity verification** — confirming the caller is who they claim to be.

| Example | How Identity is Proven |
|---------|----------------------|
| IAM User | AWS access key + secret |
| Cognito User | Username + password → JWT token |
| JWT Token | Signed token issued by an auth server |
| Signed Request | SigV4 signature on the HTTP request |

> 📌 *"Authentication answers WHO you are."*

---

### 🚦 Authorization — Are You Allowed?

Authorization is **permission verification** — determining what actions the authenticated caller is permitted to perform.

| Question | Example |
|----------|---------|
| Can you call `/admin`? | Admin role required |
| Can you use `POST`? | Write permission required |
| Can you access this API? | Valid API key or policy required |

> 📌 *"Authorization answers WHAT you can do."*

---

### Order of Execution

```
Client
  ↓
Authentication   ←  Who are you?
  ↓
Authorization    ←  What are you allowed to do?
  ↓
API Gateway
  ↓
Backend
```

> 📌 *"Authentication always happens before authorization — you cannot check permissions for an unknown identity."*

---

### Real-World Analogy — Office Building

| Office Security | API Gateway Equivalent |
|----------------|----------------------|
| Show your ID card at the entrance | Authentication — prove your identity |
| Security checks which floors you can access | Authorization — verify your permissions |
| Visitor from outside blocked at gate | Resource Policy — restrict by IP or network |

---

## Authorization Options in API Gateway

API Gateway supports four authorization mechanisms. Each is designed for a different use case.

---

### 🔹 IAM Authorization

Uses AWS IAM identities to control API access. Every request must be **signed using SigV4** (AWS Signature Version 4).

```
IAM User / Role
  ↓  (Signed Request — SigV4)
API Gateway
  ↓
IAM Policy Evaluation
  ↓
Allow / Deny
```

**Use Cases:**
- Internal service-to-service communication
- Backend microservices calling each other
- AWS-native integrations (Lambda calling another API)

| ✅ Pros | ❌ Cons |
|--------|--------|
| Strong, native AWS security | Not browser-friendly |
| No token management needed | Requires AWS credentials |
| Works seamlessly across AWS services | Complex for external/public users |

> 📌 *"IAM auth is not browser-friendly — it is designed for machine-to-machine communication inside AWS."*

---

### 🔹 Lambda Authorizer (Custom Authorizer)

A Lambda function that acts as a **custom security gate**. API Gateway calls the authorizer Lambda before forwarding the request to the backend.

```
Client → Token / Header
           ↓
    Lambda Authorizer
           ↓
     Allow / Deny
           ↓
        Backend
```

**How it works:**

1. Client sends a request with a token in the header:
```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

2. API Gateway invokes the Lambda Authorizer with that token
3. Lambda validates the token (checks signature, expiry, claims)
4. Lambda returns an **IAM policy** — Allow or Deny
5. API Gateway enforces the policy — forwards or rejects the request

**Use Cases:**
- Custom JWT validation against your own auth server
- Third-party authentication providers (Auth0, Okta, Firebase)
- Legacy token formats that Cognito doesn't support
- Fine-grained access control based on token claims

| ✅ Pros | ❌ Cons |
|--------|--------|
| Maximum flexibility | Extra latency per request |
| Works with any token format | Additional Lambda cost |
| Business logic can inform decisions | More complexity to build and maintain |

> 📌 *"Lambda authorizers give maximum flexibility — if you need to hit a database or apply business logic to decide access, this is your only option."*

---

### 🔹 Cognito Authorizer

Uses **Amazon Cognito User Pools** to authenticate users. Cognito issues a JWT token on login, and API Gateway validates it automatically — no Lambda needed.

```
User Login
  ↓
Cognito issues JWT
  ↓
Client calls API with JWT in Authorization header
  ↓
API Gateway validates token against Cognito User Pool
  ↓
Allow / Deny
```

**Use Cases:**
- Web applications with user login
- Mobile apps (iOS / Android)
- Any public-facing API where end users sign up and log in

| ✅ Pros | ❌ Cons |
|--------|--------|
| Fully managed by AWS | Cognito has a learning curve |
| Scales automatically | Less flexible than Lambda Authorizer |
| Built-in user management (sign-up, MFA, password reset) | Tied to Cognito ecosystem |

> 📌 *"Cognito is the best choice for user-facing applications — it handles user management, token issuance, and validation all in one place."*

---

### 🔹 API Keys

> 🚨 **Important Clarification: API Keys are NOT authentication.**

API Keys are used for **usage tracking and rate limiting** — not for verifying identity or securing an API.

**What API Keys are for:**

| Use Case | Example |
|----------|---------|
| Usage tracking | Monitor how many calls a partner makes |
| Rate limiting | Allow 1000 requests/day per key |
| Tiered access | Free vs paid API plans |

**What API Keys are NOT for:**
- Proving who the caller is
- Securing sensitive data
- Replacing tokens or IAM policies

> 📌 *"API Keys tell you HOW MUCH someone is using your API — not WHO they are or WHETHER they are allowed."*

---

### Authorization Options — Summary

| Option | Identity Type | Best For | Browser Friendly |
|--------|--------------|---------|-----------------|
| IAM | AWS users / roles | Internal, service-to-service | ❌ No |
| Lambda Authorizer | Custom token / header | Flexible, complex auth logic | ✅ Yes |
| Cognito | End users (JWT) | Web and mobile apps | ✅ Yes |
| API Keys | Usage control only | Rate limiting, partner APIs | ✅ Yes |

---

### The Golden Rule

> *"If you need to check a database or apply business logic to decide if a user can access a route — use a **Lambda Authorizer**. If you just need to verify the user is logged in to your app — use **Cognito**."*

---

## Resource Policies

Resource policies operate at the **network level** — they control where requests can come from, before any authentication or authorization is evaluated.

> 📌 *"Resource policy is the firewall of API Gateway."*

---

### Restrict by IP Address

Allow only requests from a specific IP range — block everything else, even if the caller has a valid token.

**Use Case:** Internal tool accessible only from the office network.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "execute-api:Invoke",
    "Resource": "execute-api:/*",
    "Condition": {
      "IpAddress": {
        "aws:SourceIp": "203.0.113.0/24"
      }
    }
  }]
}
```

> 📌 *"Even with a valid token, an IP restriction can block access. Network-level checks happen before any auth check."*

---

### Restrict by VPC Endpoint (Private API)

Make the API accessible **only from inside a VPC** — completely unreachable from the public internet.

```
EC2 / Lambda (inside VPC)
  ↓
VPC Interface Endpoint
  ↓
API Gateway (Private)
```

**Use Case:** An internal microservice that should never be exposed publicly — payment processing, internal reporting, data pipelines.

> 📌 *"Private APIs are not accessible from the internet at all — even if someone knows the URL, they cannot reach it from outside the VPC."*

---

### Cross-Account Access

One AWS account exposes an API, another AWS account consumes it. The resource policy grants access to a specific account ID.

**Use Case:** A platform team in Account A exposes a shared API. Application teams in Account B consume it.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::ACCOUNT-B-ID:root"
    },
    "Action": "execute-api:Invoke",
    "Resource": "execute-api:/*"
  }]
}
```

> 📌 *"Cross-account API access is controlled entirely through resource policies — no extra infrastructure needed."*

---

## Security Layers — Big Picture

API Gateway enforces security in three distinct layers, each answering a different question:

```
Resource Policy      ←  WHERE is the request coming from?
  ↓
Authentication       ←  WHO is making the request?
  ↓
Authorization        ←  WHAT are they allowed to do?
  ↓
Backend
```

| Layer | Mechanism | Question Answered |
|-------|-----------|------------------|
| Resource Policy | IP, VPC, Account restrictions | Where is the request from? |
| Authentication | IAM, Cognito, Lambda Authorizer | Who is the caller? |
| Authorization | IAM Policy, Authorizer response | What can the caller do? |
| Backend | Your business logic | What should happen? |

All three layers must pass for a request to reach the backend. Failing at any layer stops the request immediately with a `403 Forbidden` or `401 Unauthorized` response.

---

# Section 8: Stages, Deployment & Versioning

---

## Goal

By the end of this section, students should understand:
- How APIs move from `dev` → `test` → `prod` safely
- What a deployment is and when it is required
- How to release changes without breaking existing users
- What canary deployments are and why they matter

---

## What is a Stage?

A **stage** is a named environment for your API. It represents a distinct version of your API that is independently deployed and configured.

| Stage | Purpose |
|-------|---------|
| `dev` | Active development — breaking changes allowed |
| `test` | QA and integration testing |
| `prod` | Live, user-facing traffic — changes must be safe |

> 📌 *"Stages are like environments in software development — the same codebase behaves differently depending on where it is deployed."*

---

### Why Stages Exist

| Without Stages | With Stages |
|---------------|------------|
| One API, one URL | Separate URL per environment |
| Every change is immediately live | Changes tested in `dev` before reaching `prod` |
| A bad deploy breaks all users | Only `dev` traffic is affected during testing |
| No isolation between testing and production | Full config isolation per stage |

---

### Stage URLs

Each stage gets its own **Invoke URL**. Same API ID, different stage path:

```
https://abc123.execute-api.us-east-1.amazonaws.com/dev
https://abc123.execute-api.us-east-1.amazonaws.com/test
https://abc123.execute-api.us-east-1.amazonaws.com/prod
```

> 📌 *"Same API, different stage, different URL — your frontend dev environment points to `/dev`, production points to `/prod`."*

---

## Stage Variables

Stage variables are **key-value pairs** attached to a stage. They allow the same API definition to behave differently across environments — without changing any configuration.

Think of them as environment variables for API Gateway.

**Example stage variables:**

| Stage | Variable | Value |
|-------|----------|-------|
| `dev` | `lambdaAlias` | `dev` |
| `prod` | `lambdaAlias` | `prod` |
| `dev` | `dbEndpoint` | `dev-db.example.com` |
| `prod` | `dbEndpoint` | `prod-db.example.com` |

---

### Using Stage Variables in Lambda Integration

Instead of hardcoding a Lambda function name, reference a stage variable:

```
arn:aws:lambda:us-east-1:123456789:function:my-fn:${stageVariables.lambdaAlias}
```

- When called via `/dev` → invokes `my-fn:dev`
- When called via `/prod` → invokes `my-fn:prod`

The API configuration stays identical. Only the stage variable value changes per environment.

> 📌 *"One API definition, multiple runtime behaviors — stage variables are what make this possible."*

---

### REST API vs HTTP API — Stage Differences

| Feature | REST API | HTTP API |
|---------|----------|----------|
| Custom stage names | ✅ `dev`, `test`, `prod` | ❌ Only `$default` |
| Stage variables | ✅ Supported | ❌ Not supported |
| Manual deployment | ✅ Required | ❌ Auto-deploys |
| Canary deployments | ✅ Supported | ❌ Not supported |

> 📌 *"Stages are most powerful in REST API — HTTP API is simpler but gives up this control."*

---

## Deployments

### What is a Deployment?

A **deployment** is a snapshot of your entire API configuration at a specific point in time — resources, methods, integrations, and mapping templates all captured together.

When you deploy to a stage, that stage starts using that snapshot. Until you deploy, **no changes are visible to users**.

> 📌 *"No deployment = no change visible to users. This is the most common reason a REST API 'isn't working' after a change."*

---

### Manual Deployment — REST API (Console Steps)

1. Go to **API Gateway → Your REST API**
2. Click **Deploy API**
3. Select stage:
   - Existing stage: `dev` or `prod`
   - Or create a new stage
4. Add a deployment note *(optional but recommended)*
5. Click **Deploy**

> 📌 *"If a student says 'my API isn't working after I made a change' — the first question is always: did you deploy?"*

---

### When Redeployment is Required

Every one of these changes requires a manual redeploy in REST API:

| Change Made | Redeploy Required? |
|-------------|-------------------|
| New resource added | ✅ Yes |
| New method added | ✅ Yes |
| Integration changed | ✅ Yes |
| Mapping template updated | ✅ Yes |
| Stage variable changed | ❌ No — takes effect immediately |
| Lambda function code changed | ❌ No — API Gateway calls the latest version |

---

### Rollbacks

API Gateway keeps a full **deployment history** per stage. If a new deployment causes errors, you can revert to any previous deployment in seconds.

**How to rollback (Console):**

1. Go to **API Gateway → Stages → dev (or prod)**
2. Click the **Deployment History** tab
3. Select a previous deployment
4. Click **Change Deployment**

The stage immediately starts using the older snapshot — no redeployment needed.

> 📌 *"API Gateway rollback is instant — you are just pointing the stage at an older snapshot that already exists."*

---

## Canary Deployments *(Advanced)*

### What is a Canary Deployment?

A canary deployment sends a **small percentage of live traffic** to a new version while the majority continues using the stable version.

```
Incoming Traffic
      ↓
  ┌───┴────────────────┐
  │                    │
 10%                  90%
New Version (v2)    Current Version (v1)
  │                    │
  └───────┬────────────┘
          ↓
       Monitor
```

> 📌 *"Test new changes on real users, safely — if the new version has errors, only 10% of users are affected and you can roll back immediately."*

---

### Why Canary Deployments Matter

| Without Canary | With Canary |
|---------------|------------|
| All users hit the new version immediately | Only a small % of users hit the new version |
| A bug affects 100% of traffic | A bug affects only the canary % |
| Rollback requires redeployment | Rollback is instant — promote or revert the canary |
| No gradual confidence building | Traffic % increased as confidence grows |

---

### Canary Deployment in API Gateway (Console Steps)

Canary deployments are configured at the **stage level** in REST API.

1. Go to **API Gateway → Stages → prod**
2. Click the **Canary** tab
3. Click **Create Canary**
4. Set canary traffic percentage (e.g. `10%`)
5. Deploy the new version to the canary
6. Monitor **CloudWatch metrics** — error rate, latency, 5xx count
7. Decide:
   - ✅ **Promote** — move 100% traffic to new version
   - ❌ **Delete Canary** — instantly roll back to stable version

> 📌 *"Canary is controlled per stage, not per API — you can run a canary on `prod` while `dev` runs the full new version."*

---

### Canary vs Blue-Green — Quick Comparison

| Strategy | How It Works | Traffic Split | Rollback Speed | Risk |
|----------|-------------|---------------|----------------|------|
| **Blue-Green** | Two full environments, switch DNS | 0% → 100% instantly | Fast | Medium |
| **Canary** | One environment, gradual traffic shift | Starts small, increases gradually | Instant | Low |

---

### Safe Release Flow

```
Deploy v2 to Canary (10% traffic)
          ↓
  Monitor error rate & latency
          ↓
  Errors low? Increase to 25%
          ↓
  Still healthy? Increase to 50%
          ↓
  Promote to 100% → Canary complete
          ↓
  (If errors spike at any point → Delete Canary → instant rollback to v1)
```

---

## Stages & Deployment — Summary

| Concept | What It Does | REST API | HTTP API |
|---------|-------------|----------|----------|
| Stage | Named environment with its own URL and config | ✅ Full control | ✅ `$default` only |
| Stage Variables | Per-environment key-value config | ✅ Supported | ❌ |
| Manual Deploy | Snapshot API config to make changes live | ✅ Required | ❌ Auto |
| Deployment History | Saved snapshots for rollback | ✅ | ❌ |
| Canary Deployment | Gradual traffic shift to new version | ✅ | ❌ |

---

# Section 9: Throttling, Quotas & Usage Plans

---

## Goal

By the end of this section, students should understand:
- How API Gateway protects backends from being overwhelmed
- How to control usage per client individually
- How to implement basic API metering and monetization

---

## What is Throttling?

Throttling controls the **number of requests** a client or API can make within a given time window. When the limit is exceeded, API Gateway returns `429 Too Many Requests` instead of forwarding to the backend.

> 📌 *"Throttle = slow down or stop excessive requests before they overload the backend."*

---

### Two Key Concepts

| Concept | Definition | Example |
|---------|-----------|---------|
| **Rate Limit** | Maximum sustained requests per second | 100 RPS |
| **Burst Limit** | Maximum requests allowed in a short spike | 200 requests at once |

> 📌 *"Rate = long-term speed limit. Burst = how many cars can rush the toll booth at once."*

**Toll booth analogy:**
- The toll booth processes X cars per minute *(rate limit)*
- A sudden rush of Y cars can queue up at once *(burst limit)*
- Cars beyond the burst limit are turned away *(429 response)*

---

### Per-Method Throttling

Different routes have different processing costs. You can set independent throttle limits per method:

```
GET  /users   →  50 RPS   (cheap read, can handle more)
POST /orders  →  10 RPS   (expensive write, throttle tighter)
```

**Console Location:**

- **REST API:** API Gateway → Stages → Select Stage → Default Route Throttling or per-method override
- **HTTP API:** Global throttling only — per-method throttling not supported

> 📌 *"Some operations are far more expensive to process than others. Throttle write operations tighter than read operations."*

---

## Usage Plans & API Keys

### What is a Usage Plan?

A **usage plan** is a named set of throttling and quota rules that is assigned to one or more API keys. It lets you control exactly how much each consumer can use your API.

| Plan | Rate Limit | Quota |
|------|-----------|-------|
| Free Tier | 10 RPS | 1,000 requests / day |
| Pro Tier | 100 RPS | 100,000 requests / day |
| Enterprise Tier | 1,000 RPS | Unlimited |

> 📌 *"Usage plans are how AWS helps you monetize or protect APIs — like mobile data plans, free 1 GB, paid 10 GB."*

---

### API Keys — Consumer Identifiers

API Keys identify **who is calling** the API for metering and throttling purposes. Each consumer gets their own key, and each key is tied to a usage plan.

> 🚨 **Reminder:** API Keys are **not authentication**. They identify and meter consumers — they do not verify identity or secure your API.

| API Key | Consumer | Rate Limit | Daily Quota |
|---------|----------|-----------|-------------|
| `free123` | Free tier user | 10 RPS | 1,000 / day |
| `pro456` | Paid subscriber | 100 RPS | 100,000 / day |
| `partner789` | Partner integration | 500 RPS | Unlimited |

> 📌 *"Even if one client misbehaves and hammers the API, their quota is exhausted — other consumers are completely unaffected."*

---

### Quotas

Quotas are **hard limits on total request count** over a rolling time window.

| Quota Type | Resets |
|-----------|--------|
| Daily | Midnight UTC |
| Weekly | Monday midnight UTC |
| Monthly | 1st of month midnight UTC |

When a quota is exhausted, API Gateway returns `429 Too Many Requests` for the remainder of the period — regardless of the rate limit.

---

### Hands-On — Create a Usage Plan (REST API Console)

1. Go to **API Gateway → Usage Plans → Create**
2. Fill in:

| Field | Value |
|-------|-------|
| Name | `FreePlan` |
| Throttling — Rate | `5` requests per second |
| Throttling — Burst | `10` requests |
| Quota | `1000` requests per day |

3. Click **Next**
4. Associate API Stage:
   - Select your REST API
   - Select stage: `dev`
   - Click **Add to Usage Plan**
5. Click **Next**
6. Create a new API Key:
   - Name: `free-user-key`
   - Click **Add API Key to Usage Plan**
7. Click **Done**

> 📌 *"Each API key represents a consumer. The usage plan enforces what that consumer is allowed to do."*

---

### Request Flow with Throttling

```
Client
  ↓  (sends API Key in header: x-api-key: free123)
API Gateway
  ↓
Check Usage Plan
  ├── Rate limit exceeded?  → 429 Too Many Requests
  ├── Quota exhausted?      → 429 Too Many Requests
  └── Within limits?        → Forward to Backend
          ↓
       Backend
```

---

### Throttling Summary

| Feature | REST API | HTTP API |
|---------|----------|----------|
| Per-method throttling | ✅ | ❌ |
| Usage plans | ✅ | ❌ |
| API Keys | ✅ | ❌ |
| Global throttling | ✅ | ✅ |
| Response on limit exceeded | `429 Too Many Requests` | `429 Too Many Requests` |

---

# Section 10: CORS Handling in API Gateway

---

## Goal

By the end of this section, students should understand:
- Why CORS exists and what it protects
- How browsers enforce it through preflight requests
- How to configure CORS in both REST API and HTTP API
- How to test and debug CORS errors

---

## What is CORS?

**CORS (Cross-Origin Resource Sharing)** is a browser security feature that blocks web pages from making requests to a different **origin** than the one they were loaded from.

**Origin = scheme + domain + port**

```
Frontend:  https://myapp.com          (Origin A)
Backend:   https://api.example.com    (Origin B)
```

When the frontend JavaScript tries to call the backend, the browser sees two different origins and **blocks the request** — unless the backend explicitly allows it via CORS headers.

> 📌 *"CORS is a browser security feature, not an AWS restriction. The server allows it, the browser enforces it."*

---

### Real-World Analogy

| Office Building | CORS |
|----------------|------|
| Visitor arrives from outside | Browser makes a cross-origin request |
| Security guard stops them | Browser intercepts and blocks the request |
| Visitor needs a guest pass | Server must send CORS headers |
| Security checks the pass | Browser reads `Access-Control-Allow-Origin` |
| Pass valid → visitor enters | Headers match → request proceeds |

---

## How CORS Works — The Preflight Request

For non-simple requests (POST, PUT, DELETE, or any request with custom headers), the browser sends an automatic **OPTIONS preflight request** before the actual request.

```
Browser
  ↓  OPTIONS /users  (preflight — "am I allowed?")
API Gateway
  ↓  200 OK + CORS headers  ("yes, you're allowed")
Browser
  ↓  POST /users  (actual request proceeds)
API Gateway
  ↓  Response + CORS headers
Browser  ✅
```

The preflight response must include:

```http
Access-Control-Allow-Origin:  https://myapp.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
```

If any of these are missing or mismatched, the browser blocks the actual request entirely.

---

## Configuring CORS — REST API (Console)

### Step 1 — Enable CORS on the Resource

1. Go to **API Gateway → REST API → Resources**
2. Select the resource (e.g. `/users`)
3. Click **Actions → Enable CORS**
4. Configure:

| Field | Value |
|-------|-------|
| Access-Control-Allow-Headers | `Content-Type, Authorization` |
| Access-Control-Allow-Methods | `GET, POST, PUT, DELETE, OPTIONS` |
| Access-Control-Allow-Origin | `*` (or your specific frontend URL) |

5. Click **Enable CORS and replace existing CORS headers**

### Step 2 — Deploy

Changes are not live until deployed:

1. Click **Deploy API**
2. Select stage `dev`
3. Click **Deploy**

---

### What AWS Does Behind the Scenes

When you enable CORS on a resource, API Gateway automatically:
- Creates an `OPTIONS` method on the resource
- Configures a **Mock Integration** on OPTIONS (no backend call needed)
- Adds the `Access-Control-*` headers to the OPTIONS response

> 📌 *"The OPTIONS preflight request is handled entirely by API Gateway — your Lambda function never sees it."*

---

## Configuring CORS — HTTP API (Console)

HTTP API has a simpler, centralized CORS configuration — no per-resource setup needed.

1. Go to **API Gateway → HTTP API → Your API**
2. Click **CORS** in the left menu
3. Configure:

| Field | Value |
|-------|-------|
| Allow Origins | `https://myapp.com` or `*` |
| Allow Methods | `GET, POST, PUT, DELETE, OPTIONS` |
| Allow Headers | `Content-Type, Authorization` |
| Max Age | `300` (seconds to cache preflight response) |

4. Click **Save**

HTTP API automatically handles all preflight responses — no manual OPTIONS method required.

---

## Testing CORS

### Browser Console Test

Open the browser DevTools → Console and run:

```javascript
fetch("https://abc123.execute-api.us-east-1.amazonaws.com/dev/users")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

**CORS not configured → blocked:**
```
Access to fetch at 'https://...' from origin 'https://myapp.com'
has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header
```

**CORS configured correctly → succeeds:**
```json
[{ "id": "101", "name": "Arjun", ... }]
```

> 📌 *"CORS errors only appear in the browser — Postman and ReqBin bypass CORS entirely because they are not browsers. If it works in Postman but fails in the browser, it is always a CORS issue."*

---

## Common Mistakes & Fixes

| Mistake | What Happens | Fix |
|---------|-------------|-----|
| OPTIONS method not created | Browser preflight gets 403 or 404 | Re-enable CORS via console — it creates OPTIONS automatically |
| Using `*` with credentials | Browser rejects auth headers and cookies | Set `Allow-Origin` to the specific domain, not `*` |
| Forgot to redeploy after enabling | CORS headers not in responses | Always deploy after enabling CORS in REST API |
| Headers mismatch | Browser blocks even with CORS enabled | Ensure `Access-Control-Allow-Headers` includes every header your frontend sends |
| Testing with Postman | No CORS error seen | Always test from a browser — Postman skips CORS checks |

---

## CORS at a Glance

```
Same Origin Request     Cross-Origin Request
──────────────────      ─────────────────────────
No CORS check           Browser sends OPTIONS preflight
Request proceeds        Server must return CORS headers
                        Browser checks headers
                        ✅ Match → request proceeds
                        ❌ No match → request blocked
```

> 📌 *"CORS errors in production almost always mean either the OPTIONS method is missing, the `Allow-Origin` header doesn't match the frontend URL, or the API was not redeployed after CORS was enabled."*




