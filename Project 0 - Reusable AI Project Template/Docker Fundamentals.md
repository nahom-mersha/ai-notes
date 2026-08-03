# Docker Fundamentals

## Why Docker?

Sometimes software works on my computer but not on someone else's because they have different Python versions or missing dependencies.

Docker solves this by packaging the whole environment together with the project.

---

## Image

An image is a packaged version of the project.

It contains:

- Python
- dependencies
- project files
- runtime environment

---

## Container

A container is a running instance of an image.

---

## Dockerfile

The Dockerfile contains the instructions for building the image.

Mine looked like:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY . .

RUN python -m pip install --no-cache-dir ".[dev]"

CMD ["pytest"]
```

`RUN` prepares the image while building it.

`CMD` is the default command that runs when the container starts.

---

## Build and Run

Build the image:

```bash
docker build -t reusable-ai-template .
```

Run the container:

```bash
docker run --rm reusable-ai-template
```

The container runs the tests inside the packaged environment.

---

## Biggest takeaway

Docker packages the application together with its runtime environment so that it behaves the same on different computers.