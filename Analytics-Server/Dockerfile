FROM python:3.13.1-slim-bookworm

# Create a non-root user
RUN addgroup --system appuser && adduser --system --group appuser

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    gcc \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Install uv package manager
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin/:$PATH"

# Copy only necessary files for dependency installation
COPY --chown=appuser:appuser pyproject.toml uv.lock ./

# Install dependencies
RUN uv sync

# Copy application code
COPY --chown=appuser:appuser main.py ./

# Use ARG for build-time variables that shouldn't persist in the final image
ARG ENVIRONMENT="production"
ARG PROJECT_NAME="Form Analytics Dashboard"
ARG PROJECT_VERSION="1.0.0"

# Use environment variables for runtime configuration
ENV ENVIRONMENT=${ENVIRONMENT} \
    PROJECT_NAME=${PROJECT_NAME} \
    PROJECT_VERSION=${PROJECT_VERSION} \
    PORT=8080 \
    CLICKHOUSE_HOST="localhost" \
    CLICKHOUSE_PORT="9000" \
    CLICKHOUSE_USER="formAnalytics"

# Use a build argument for sensitive data
ARG CLICKHOUSE_PASSWORD="test1234"

# Switch to non-root user
USER appuser

# Expose the port FastAPI will run on
EXPOSE 8000

# Run the FastAPI application with uvicorn
CMD ["sh", "-c", "uv run fastapi dev main.py --network host"]


# FROM python:3.13.1-slim-bookworm

# WORKDIR /app

# # Install curl and uv package manager
# RUN apt update && apt install -y curl && \
#     curl -LsSf https://astral.sh/uv/install.sh | sh
# ENV PATH="/root/.local/bin/:$PATH"

# # Copy project configuration files
# COPY pyproject.toml uv.lock .python-version ./

# # Additional dependencies needed for clickhouse-driver
# # Install dependencies needed for clickhouse-driver and project dependencies
# RUN apt install -y gcc python3-dev && \
#     uv sync
# # Copy application code
# COPY main.py ./

# # Set environment variables
# ENV ENVIRONMENT="production" \
#     PROJECT_NAME="Form Analytics Dashboard" \
#     PROJECT_VERSION="1.0.0" \
#     PORT=8080 \
#     CLICKHOUSE_HOST="localhost" \
#     CLICKHOUSE_PORT="9000" \
#     CLICKHOUSE_USER="formAnalytics" \
#     CLICKHOUSE_PASSWORD="test1234"

# # Expose the port FastAPI will run on
# EXPOSE 8000

# # Run the FastAPI application with uvicorn
# CMD ["sh", "-c", "uv run fastapi dev main.py", "--", "--network host"]