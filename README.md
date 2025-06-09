# Architecture Documentation

This folder contains the architecture-related resources for the Mir ChatApp project.

## Overview
The architecture of the Mir ChatApp is designed to ensure scalability, reliability, and maintainability. The system components and their interactions are illustrated in the diagram below:

![Architecture Diagram](/mir_chatapp.png)

## Contents
- `mir_chatapp.drawio`: The source file for the architecture diagram, editable using draw.io or compatible tools.
- `mir_chatapp.png`: The architecture diagram in PNG format.
- `mir_chatapp.svg`: The architecture diagram in SVG format.

# ChatApp WebSocket API

This project implements a WebSocket-based chat application using AWS Lambda and API Gateway. It includes multiple Lambda functions to handle WebSocket events such as connecting, disconnecting, sending messages, and default actions.

## Project Structure

```
Authorizer/
    lambda_function.py          # Handles JWT-based authorization for WebSocket connections
    requirements.txt            # Python dependencies for the Authorizer
Connect/
    lambda_function.py          # Handles WebSocket connection events
Default/
    lambda_function.py          # Handles default WebSocket events
Disconnect/
    lambda_function.py          # Handles WebSocket disconnection events
SendMessage/
    lambda_function.py          # Handles sending messages between users
```

## Lambda Functions

### Authorizer
- **Purpose**: Validates JWT tokens for WebSocket connections.
- **Dependencies**: Install required packages using `pip install -r requirements.txt`.

### Connect
- **Purpose**: Handles new WebSocket connection events and stores connection IDs in DynamoDB.

### Default
- **Purpose**: Handles unrecognized WebSocket events.

### Disconnect
- **Purpose**: Handles WebSocket disconnection events and removes connection IDs from DynamoDB.

### SendMessage
- **Purpose**: Handles sending messages between users. It retrieves connection IDs from DynamoDB and uses the API Gateway Management API to send messages.

## Setup Instructions

1. **Install Dependencies**:
   - For the `Authorizer` function, navigate to the `Authorizer/` directory and run:
     ```bash
     pip install -r requirements.txt -t .
     ```

2. **Deploy Lambda Functions**:
   - Use the AWS Management Console or AWS CLI to deploy each Lambda function.

3. **Configure API Gateway**:
   - Set up a WebSocket API in API Gateway.
   - Map routes (`$connect`, `$disconnect`, `$default`, `sendmessage`) to their respective Lambda functions.

4. **Environment Variables**:
   - Ensure the following environment variables are set for the Lambda functions:
     - `USER_CONVERSATION_MAPPER_TABLE`
     - `USER_CONVERSATION_TABLE`
     - `CONNECTIONS_TABLE`
     - `APIGATEWAY_ENDPOINT`

## Testing

- Use WebSocket testing tools like [wscat](https://github.com/websockets/wscat) or Postman to test the WebSocket API.
- Example command to connect using `wscat`:
  ```bash
  wscat -c wss://<your-api-id>.execute-api.<region>.amazonaws.com/<stage>
  ```


# ChatApp REST API

This repository contains the Lambda functions for the ChatApp REST API. These functions interact with AWS DynamoDB to provide various functionalities for the application.

## Functions

### 1. Get Users
- **Path**: `src/get_users/lambda_function.py`
- **Description**: Fetches a list of users from the `users` DynamoDB table with pagination support.
- **Environment Variables**:
  - `USERS_TABLE_NAME`: The name of the DynamoDB table containing user data.
- **Headers**:
  - `Authorization`: Access token required for authentication.

### 2. Get Chat History
- **Path**: `src/get_chat_history/lambda_function.py`
- **Description**: (To be implemented) Fetches the chat history between users from the DynamoDB table.

## Setup

1. Ensure you have AWS credentials configured to allow access to DynamoDB.
2. Set the required environment variables for each Lambda function.
3. Deploy the Lambda functions using your preferred deployment method.

## Example Request

### Get Users
**Request**:
```http
GET /get-users
Authorization: Bearer <access_token>
```

**Response**:
```json
{
    "users": [
        {
            "userName": "ice-cream",
            "createdat": "2025-06-05T19:03:55.509736",
            "email": "faiem.ict.learning@gmail.com",
            "userId": "5881b3d0-00e1-705c-b4e2-18b764530b78"
        }
    ],
    "lastEvaluatedKey": null
}
```

# Mir ChatApp Frontend

Mir ChatApp is a modern chat application frontend built with React, TypeScript, Vite, and Bootstrap.

## Features

- Responsive and centered login UI
- Bootstrap-powered styling
- React Router for navigation

## Getting Started

### Prerequisites

- Node.js (v18 or newer recommended)
- npm

### Installation

1. Clone the repository or download the source code.
2. Install dependencies:

   ```sh
   npm install
   ```

3. Start the development server:

   ```sh
   npm run dev
   ```

4. Open your browser and go to `http://localhost:5173` (or the port shown in your terminal).

## Project Structure

- `src/` - Main source code
  - `App.tsx` - Main app component (login UI)
  - `ChatPage.tsx` - Chat page after login
  - `index.css` - Global and custom styles
  - `main.tsx` - Entry point
- `public/` - Static assets
- `index.html` - Main HTML file


