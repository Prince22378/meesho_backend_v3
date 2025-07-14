# Use official Python slim image as base
FROM python:3.10-slim

# Install system dependencies needed for building some Python packages
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    libffi-dev \
    libssl-dev \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Set working directory inside container
WORKDIR /app

# Copy only requirements file first (for caching)
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy all your project code into container
COPY . .

# Collect static files (only if you use collectstatic)
RUN python3 manage.py collectstatic --noinput

# Run database migrations at container startup (optional)
# CMD ["python3", "manage.py", "migrate"]

# Start the Gunicorn server to serve your Django app
CMD ["gunicorn", "backend.wsgi", "--log-file", "-"]
