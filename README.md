# 😄 Chuck Norris Jokes API

![Java](https://img.shields.io/badge/Java_8-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_2.7-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![Lombok](https://img.shields.io/badge/Lombok-BC4521?style=for-the-badge&logo=lombok&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)

A Spring Boot REST API that fetches **25 unique Chuck Norris jokes** from the [Chuck Norris API](https://api.chucknorris.io) in a single request. The core challenge: the external API returns one random joke at a time, so this service handles repeated calls, deduplication, and infinite-loop prevention automatically.

---

## 🧠 How It Works

The external API (`api.chucknorris.io/jokes/random`) returns **one random joke per call** — and it can repeat jokes across calls. To collect 25 unique jokes, the service implements a smart accumulation strategy:

```
1. Call the external API repeatedly
2. Store results in a HashSet (guarantees no duplicates)
3. Keep calling until 25 unique jokes are collected
4. Safety guard: if 5 consecutive calls return no new joke,
   stop the loop to avoid infinite retries
5. Return the full list
```

This handles the real-world scenario where an API with a limited pool of results starts repeating itself before the target count is reached.

---

## 📐 Architecture

Clean layered architecture following Spring Boot conventions:

```
com.achavez.joke
 ├── controller
 │    └── JokeController          REST endpoint
 ├── service
 │    ├── JokeService             Interface
 │    └── impl/JokeServiceImpl    Business logic
 ├── repository
 │    ├── JokeRepository          Interface
 │    └── impl/JokeRepositoryImpl External API calls + deduplication
 ├── dto
 │    └── JokeDTO                 Response model
 └── config
      └── RestTemplateConfig      HTTP client setup
```

---

## 🔗 Endpoint

| Method | URL | Description |
|---|---|---|
| `GET` | `/api/getJokes` | Returns a list of 25 unique Chuck Norris jokes |

### Sample response

```json
[
  {
    "id": "abc123",
    "categories": [],
    "created_at": "2020-01-05 13:42:19.00000",
    "value": "Chuck Norris can divide by zero."
  }
]
```

---

## ⚙️ Configuration

Both the external API URL and the number of jokes to fetch are externalized in `application.properties`:

```properties
url.api.chuck=https://api.chucknorris.io/jokes/random
count.api.chuch=25
```

You can change the count to any number without touching the code.

---

## 🚀 Getting Started

### Prerequisites
- Java 8+
- Maven 3.6+

### Run locally

```bash
mvn spring-boot:run
```

The API will be available at `http://localhost:8080/api/getJokes`.

### Build

```bash
mvn clean package
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 8 |
| Framework | Spring Boot 2.7 |
| HTTP Client | RestTemplate |
| Deduplication | HashSet |
| Code generation | Lombok |
| Build Tool | Maven |
| External API | Chuck Norris API (api.chucknorris.io) |

---

## 👨‍💻 Author

**Antony Chávez** — Backend Developer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/antonychavez)
[![GitHub](https://img.shields.io/badge/GitHub-Xavez05-181717?style=flat&logo=github&logoColor=white)](https://github.com/Xavez05)
