# Auto-Scaling Face Recognition System

## Overview
This project implements a distributed, elastic face recognition system using AWS EC2, SQS, and S3. It features a custom auto-scaling algorithm to dynamically scale EC2 instances based on demand, efficiently handling up to 1,000 concurrent requests.

## Features
- **Web Tier**: Handles HTTP requests and forwards image files for processing.
- **App Tier**: Processes image recognition using a deep learning model.
- **Data Tier**: Stores input images and recognition results on AWS S3.
- **Auto-Scaling**: Dynamically scales the App Tier based on demand.
- **Concurrency**: Handles multiple concurrent requests efficiently.

## Architecture
The system follows a multi-tiered architecture:
1. **Web Tier**: Single EC2 instance responsible for receiving user requests and sending responses.
2. **App Tier**: Multiple EC2 instances for processing requests, auto-scaling based on demand.
3. **Data Tier**: AWS S3 for storage and AWS SQS for message queuing.

## Setup Instructions

### Prerequisites
- AWS account with EC2, S3, SQS, and IAM permissions.
- Python 3.x installed on the system.
- AWS CLI configured with proper credentials.

### Installation
1. **Web Tier Setup**:
   - Deploy a single EC2 instance (Ubuntu or AWS Linux AMI) and name it `web-instance`.
   - Install required dependencies:
     ```sh
     sudo apt update && sudo apt install -y python3-pip
     pip3 install boto3 flask
     ```
   - Run the Web Tier application.

2. **App Tier Setup**:
   - Create a base EC2 instance and install dependencies:
     ```sh
     sudo apt update && sudo apt install -y python3-pip
     pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
     ```
   - Copy the deep learning model code and model weights to the instance.
   - Create an AMI from this instance and use it to launch `app-tier-instance-<instance#>`.

3. **Data Tier Setup**:
   - Create two S3 buckets:
     ```sh
     aws s3 mb s3://<ASU-ID>-in-bucket
     aws s3 mb s3://<ASU-ID>-out-bucket
     ```
   - Create two SQS queues:
     ```sh
     aws sqs create-queue --queue-name <ID>-req-queue
     aws sqs create-queue --queue-name <ID>-resp-queue
     ```

### Running the System
1. Start the Web Tier application.
2. Run the Auto-Scaling controller.
3. Monitor EC2 instances and ensure App Tier instances scale dynamically.
