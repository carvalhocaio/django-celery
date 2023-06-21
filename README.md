# Django Celery Async Tasks

A project with the objective of showing how asynchronous tasks work using Django and Celery.

## Commands

**redis container**  

```terminal
docker run -d --name redis-server -p 6379:6379 redis 
```

*connection url*: `redis://localhost:6379`

**execute celery**

```terminal
<!-- execute celery -->
celery -A core worker -l info

<!-- execute flower -->
celery --broker=redis://localhost:6379/0 flower -A tasks
```

---
