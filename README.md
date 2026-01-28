# library-api
# 📚 Library Management API

Build a production-ready REST API that manages library book inventory using **in-memory storage** without any external database dependencies. This API allows users to perform CRUD operations (Create, Read, Update, Delete) on book records efficiently.

---

## ✨ Features

- ✅ **Add Books** - Create new book entries with title, author, and year
- ✅ **View Books** - Retrieve all books or specific book by ID
- ✅ **Update Books** - Modify existing book information
- ✅ **Delete Books** - Remove books from inventory
- ✅ **Search Books** - Filter books by publication year
- ✅ **Thread-Safe** - Concurrent operations handled safely
- ✅ **Input Validation** - Automatic validation of all inputs
- ✅ **Error Handling** - Comprehensive error responses

---

## 🛠️ Technology Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| **Java** | 17+ | Programming Language |
| **Spring Boot** | 4.0.2 | Application Framework |
| **Spring Web** | 4.0.2 | REST API Development |
| **Spring Validation** | 4.0.2 | Input Validation |
| **ConcurrentHashMap** | - | Thread-Safe In-Memory Storage |

---

## 📋 Prerequisites

Before running this application, ensure you have:

- ☕ **Java 17 or higher** installed
- 🌐 **Port 8080** available

### Check Versions

```bash
# Check Java version
java -version

# Check Maven version
mvn -version
```

---

## 🚀 Setup & Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/library-api.git
cd library-api
```

### Step 2: Build the Project

```bash
mvn clean install
```

**Expected Output:**
```
[INFO] BUILD SUCCESS
[INFO] Total time: 10.5 s
```

### Step 3: Run the Application

```bash
mvn spring-boot:run
```

**Expected Output:**
```
Started LibraryApiApplication in 2.5 seconds
Tomcat started on port 8080
```

### Step 4: Verify Application

Open browser or use curl:
```bash
curl http://localhost:8080/books
```

**Response:** `[]` (empty array - no books yet)

---

## 📚 API Endpoints

### Base URL
```
http://localhost:8080
```

### Endpoints Overview

| Method | Endpoint | Description | Status Code |
|--------|----------|-------------|-------------|
| POST | `/books` | Add a new book | 201 Created |
| GET | `/books` | Get all books | 200 OK |
| GET | `/books/{id}` | Get book by ID | 200 OK / 404 Not Found |
| PUT | `/books/{id}` | Update a book | 200 OK / 404 Not Found |
| DELETE | `/books/{id}` | Delete a book | 204 No Content / 404 Not Found |
| GET | `/books/search?year=YYYY` | Search by year | 200 OK |

---

## 🧪 API Usage Examples

### 1️⃣ Add a New Book (POST)

**Request:**
```bash
curl -X POST http://localhost:8080/books \
  -H "Content-Type: application/json" \
  -d '{
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald",
    "year": 1925
  }'
```

**Response:** `201 Created`
```json
{
  "id": 1,
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald",
  "year": 1925
}
```

---

### 2️⃣ Get All Books (GET)

**Request:**
```bash
curl http://localhost:8080/books
```

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald",
    "year": 1925
  },
  {
    "id": 2,
    "title": "1984",
    "author": "George Orwell",
    "year": 1949
  }
]
```

---

### 3️⃣ Get Book by ID (GET)

**Request:**
```bash
curl http://localhost:8080/books/1
```

**Response:** `200 OK`
```json
{
  "id": 1,
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald",
  "year": 1925
}
```

**If book not found:** `404 Not Found`

---

### 4️⃣ Update a Book (PUT)

**Request:**
```bash
curl -X PUT http://localhost:8080/books/1 \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Animal Farm",
    "author": "George Orwell",
    "year": 1945
  }'
```

**Response:** `200 OK`
```json
{
  "id": 1,
  "title": "Animal Farm",
  "author": "George Orwell",
  "year": 1945
}
```

---

### 5️⃣ Delete a Book (DELETE)

**Request:**
```bash
curl -X DELETE http://localhost:8080/books/1
```

**Response:** `204 No Content` (empty body)

---

### 6️⃣ Search Books by Year (GET)

**Request:**
```bash
curl "http://localhost:8080/books/search?year=1949"
```

**Response:** `200 OK`
```json
[
  {
    "id": 2,
    "title": "1984",
    "author": "George Orwell",
    "year": 1949
  }
]
```

---

## 📸 Output Screenshots

### 1. POST Request - Adding a Book
![Add Book](<img width="1920" height="1080" alt="Screenshot 2026-01-28 123455" src="https://github.com/user-attachments/assets/c748ac99-b0a2-4404-8368-622ed633837e" />
)
*Successfully added a new book with ID 1*

### 2. GET Request - Retrieving All Books
![Get All Books](<img width="1920" height="1080" alt="Screenshot 2026-01-28 123345" src="https://github.com/user-attachments/assets/aac7cbe7-0c42-4587-9301-b41c18fa0472" />
)
*Retrieved all books from the library*

### 3. GET Request - Retrieving Specific Book
![Get Book by ID](screenshots/get-book-by-id.png)
*Retrieved book details by ID*


### 4. DELETE Request - Deleting a Book
![Delete Book](screenshots/delete-book.png)
*Successfully deleted a book (204 No Content)*

### 5. GET Request - Search by Year
![Search Books](screenshots/search-by-year.png)
*Filtered books by publication year*

---

## 🧠 The Logic - How We Built This

### Why This Approach?

**1. In-Memory Storage with ConcurrentHashMap**
- **Why?** No database setup required, faster operations, perfect for demo/testing
- **Thread-Safety:** `ConcurrentHashMap` handles multiple concurrent requests safely
- **Limitation:** Data is lost on application restart (acceptable for this use case)

**2. Layered Architecture (MVC Pattern)**
```
Controller Layer (REST API)
    ↓
Service Layer (Business Logic)
    ↓
Repository Layer (Data Access)
    ↓
In-Memory Storage (ConcurrentHashMap)
```
- **Why?** Separation of concerns, easy to test, maintainable code
- **Benefit:** Can easily switch to database later without changing controller/service

**3. AtomicLong for ID Generation**
- **Why?** Thread-safe auto-increment without race conditions
- **Alternative considered:** UUID (rejected - less readable for demo)

**4. Bean Validation**
- **Why?** Declarative validation, less boilerplate code
- **Annotations used:** `@NotBlank`, `@Min`

### Challenges Faced & Solutions

**Challenge 1: Thread Safety**
- **Problem:** Multiple users adding books simultaneously could cause data corruption
- **Solution:** Used `ConcurrentHashMap` instead of regular `HashMap`
- **Result:** Safe concurrent operations without explicit locking

**Challenge 2: Spring Boot 4.x Security**
- **Problem:** Auto-generated password blocked API access
- **Solution:** Disabled security auto-configuration in `application.properties`
- **Learning:** Always check framework version compatibility

**Challenge 3: Data Validation**
- **Problem:** Users could submit invalid data (empty titles, negative years)
- **Solution:** Used Jakarta Bean Validation with `@Valid` annotation
- **Result:** Automatic validation with clear error messages

### Design Decisions

| Decision | Rationale |
|----------|-----------|
| No Database | Faster setup, no dependencies, meets requirements |
| REST over SOAP | Industry standard, simpler, better for web/mobile |
| JSON over XML | Lightweight, readable, JavaScript-friendly |
| Spring Boot | Rapid development, convention over configuration |
| Maven over Gradle | More widely used in enterprise, easier for beginners |

---

## 🎓 What I Learned

### Technical Skills
- ✅ Building RESTful APIs with Spring Boot
- ✅ Thread-safe programming with Java concurrency utilities
- ✅ Proper HTTP status code usage
- ✅ Input validation and error handling
- ✅ API testing with Postman/curl

### Best Practices
- ✅ Separation of concerns (MVC architecture)
- ✅ Dependency injection for loose coupling
- ✅ Comprehensive error handling
- ✅ Code documentation and commenting
- ✅ Git version control

### Problem-Solving Approach
1. Understand requirements thoroughly
2. Design architecture before coding
3. Implement incrementally (one endpoint at a time)
4. Test frequently during development
5. Document as you build

---

## 🔮 Future Improvements

If I had 2 more days, I would add:

### Day 1: Enhanced Features
- [ ] **Database Integration** - PostgreSQL/H2 for data persistence
- [ ] **Pagination** - Handle large book collections efficiently
- [ ] **Sorting** - Sort by title, author, year
- [ ] **Advanced Search** - Search by title/author (full-text)
- [ ] **ISBN Support** - Add ISBN with validation
- [ ] **Categories/Genres** - Classify books
- [ ] **Book Availability** - Track borrowed/available status

### Day 2: Production Ready
- [ ] **Authentication** - JWT-based user authentication
- [ ] **Authorization** - Role-based access (Admin/User)
- [ ] **Unit Tests** - JUnit + Mockito (80%+ coverage)
- [ ] **Integration Tests** - Test complete API flows
- [ ] **API Documentation** - Swagger/OpenAPI integration
- [ ] **Docker Support** - Containerize the application
- [ ] **CI/CD Pipeline** - GitHub Actions for automated testing
- [ ] **Logging** - Structured logging with ELK stack
- [ ] **Monitoring** - Spring Boot Actuator + Prometheus
- [ ] **Rate Limiting** - Prevent API abuse

---

## 📁 Project Structure

```
library-api/
│
├── src/
│   ├── main/
│   │   ├── java/com/library/
│   │   │   ├── LibraryApiApplication.java      # Main application class
│   │   │   │
│   │   │   ├── controller/
│   │   │   │   └── BookController.java         # REST endpoints
│   │   │   │
│   │   │   ├── service/
│   │   │   │   └── BookService.java            # Business logic
│   │   │   │
│   │   │   ├── repository/
│   │   │   │   └── BookRepository.java         # Data access layer
│   │   │   │
│   │   │   ├── model/
│   │   │   │   └── Book.java                   # Book entity
│   │   │   │
│   │   │   └── exception/
│   │   │       ├── GlobalExceptionHandler.java # Error handling
│   │   │       └── ErrorResponse.java          # Error response model
│   │   │
│   │   └── resources/
│   │       └── application.properties          # Configuration
│   │
│   └── test/
│       └── java/com/library/                   # Test files (future)
│
├── screenshots/                                # API testing screenshots
├── pom.xml                                     # Maven dependencies
├── .gitignore                                  # Git ignore rules
├── README.md                                   # This file
├── QUICKSTART.md                               # Quick start guide
├── TROUBLESHOOTING.md                          # Common issues & solutions
└── Library-API.postman_collection.json         # Postman test collection
```

---

## 🧪 Testing

### Manual Testing with cURL

Run the included test script:
```bash
./test-api.sh
```

### Testing with Postman

1. Import the Postman collection: `Library-API.postman_collection.json`
2. Run all requests in sequence
3. Verify responses and status codes

### Test Coverage

| Test Case | Expected Result | Status |
|-----------|----------------|--------|
| Add valid book | 201 Created | ✅ |
| Add book with missing title | 400 Bad Request | ✅ |
| Add book with negative year | 400 Bad Request | ✅ |
| Get all books | 200 OK with array | ✅ |
| Get existing book by ID | 200 OK with book | ✅ |
| Get non-existent book | 404 Not Found | ✅ |
| Update existing book | 200 OK with updated book | ✅ |
| Update non-existent book | 404 Not Found | ✅ |
| Delete existing book | 204 No Content | ✅ |
| Delete non-existent book | 404 Not Found | ✅ |
| Search by existing year | 200 OK with results | ✅ |
| Search by non-existent year | 200 OK with empty array | ✅ |

---

## ⚙️ Configuration

### Application Properties

```properties
# Server Configuration
server.port=8080

# Application Name
spring.application.name=library-api

# Security (Disabled for testing)
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration

# Logging
logging.level.root=INFO
logging.level.com.library=DEBUG
```

### Customization

**Change Port:**
```properties
server.port=8081
```

**Enable Security:**
Remove the `spring.autoconfigure.exclude` line

---

## 🐛 Troubleshooting

### Common Issues

**Issue:** Port 8080 already in use
```bash
# Solution: Change port in application.properties
server.port=8081
```

**Issue:** Java version mismatch
```bash
# Solution: Ensure Java 17+ is installed
java -version
```

**Issue:** Maven build fails
```bash
# Solution: Clean and rebuild
mvn clean install -U
```

For more troubleshooting tips, see [TROUBLESHOOTING.md](TROUBLESHOOTING.md)

---


*Last Updated: January 28, 2026*
