# The app is deployed in the azure web services which work on serverless principle it will auto manage resources and loads.
# when to deploy the app in the self managed container service you can use this Dockerfile, e.g. Cloud Run GCP, AWS Lambda...
# Use the official Python 3.9 image as a base image
FROM python:3.9

# Set the working directory in the container
WORKDIR /app

#####################################################
# Install packages.txt all here manually
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    apt-utils \
    ca-certificates \
    gnupg \
    wget \
    curl \
    unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy the requirements.txt file into the container at /app dir
COPY requirements.txt /app/requirements.txt
COPY packages.txt /app/packages.txt
COPY diabetes-prediction-rfc-model.pkl /app/diabetes-prediction-rfc-model.pkl

# Install the dependencies specified in requirements.txt
RUN pip3 install --no-cache-dir -r /app/requirements.txt

# Update Everything
RUN apt-get update

# Copy the rest of the application code into the container
COPY . /app

# Expose port 5000 for the Flask app at port 80
EXPOSE 80

# Command to run the Flask app with Gunicorn
CMD ["gunicorn", "--workers", "2", "--bind", "0.0.0.0:80", "--timeout", "200", "app:app"]
