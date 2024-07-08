# Use the official Python 3.11 Alpine image as base
FROM python:3.11-alpine

# Set the working directory in the container
WORKDIR /app

# Use an alternative repository URL and install system dependencies and Poetry
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories \
    && apk update \
    && apk add --no-cache \
    curl \
    bash \
    gcc \
    libc-dev \
    libffi-dev \
    musl-dev \
    openssl-dev \
    make \
    && curl -sSL https://install.python-poetry.org | python3 - \
    && apk del curl

# Add Poetry to the PATH
ENV PATH="/root/.local/bin:$PATH"

# Copy the local code to the container
COPY . .

# Install project dependencies
RUN poetry install

# Ensure prestart.sh is executable and convert line endings to Unix format
RUN chmod +x prestart.sh && dos2unix prestart.sh


# Run prestart.sh directly
RUN poetry run bash ./prestart.sh

# Expose port 8000 to the outside world
EXPOSE 8000

# Command to run the FastAPI application with Uvicorn
CMD ["poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
