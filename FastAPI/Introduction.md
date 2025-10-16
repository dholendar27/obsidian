---
date created: 2025-10-16 17:51
date updated: 2025-10-16 17:51
---

**FastAPI** is a **modern, fast (high-performance)** web framework for building **APIs** with Python, based on:

- **Python 3.7+** type hints
- **Starlette** for the web parts (routing, requests, responses)
- **Pydantic** for data validation and serialization

---

### Key Features

- **Fast**: Extremely fast thanks to **Starlette** and async support (comparable to Node.js and Go in performance).
- **Intelligent**: Uses Python type hints to automatically generate:
  - Input validation
  - Data parsing
  - Automatic **OpenAPI (Swagger)** docs
- **Dependency Injection**: Clean and powerful system for injecting dependencies.
- **Testing-ready**: Simple to test using standard Python tools like `pytest`.
- **Asynchronous support**: Built with **async/await**, allowing for non-blocking code.

| **Feature**           | **Django**                               | **FastAPI**                                   | **Flask**                                |
| --------------------- | ---------------------------------------- | --------------------------------------------- | ---------------------------------------- |
| **Use Case**          | Full-stack web apps (frontend + backend) | High-performance APIs, async microservices    | Lightweight web apps or APIs             |
| **Async Support**     | Partial (from Django 3.1+)               | Full async/await support                      | Not native (requires extensions)         |
| **ORM**               | Built-in (Django ORM)                    | No built-in ORM (commonly uses SQLAlchemy)    | No built-in ORM (optional)               |
| **Form Handling**     | Built-in (Forms + Admin)                 | Not built-in                                  | Manual or third-party                    |
| **Authentication**    | Built-in authentication system           | Requires manual setup or third-party packages | Requires manual setup                    |
| **Admin Interface**   | Auto-generated admin dashboard           | None                                          | None                                     |
| **Data Validation**   | Basic (via Forms or DRF serializers)     | Strong (via Pydantic models)                  | Manual or third-party (e.g. Marshmallow) |
| **API Documentation** | Requires DRF + Swagger tools             | Auto-generated (OpenAPI + Swagger UI)         | Not built-in                             |
| **Performance**       | Slower (monolithic, more overhead)       | Very fast (async-first, minimal overhead)     | Moderate (sync, simple design)           |
| **Community**         | Large, mature                            | Growing rapidly                               | Very large, well-established             |
