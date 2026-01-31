# Requirements Document: CloudSketch

## Introduction

CloudSketch is a developer productivity tool designed to help students and developers in India learn cloud architecture through interactive diagram analysis. Users upload hand-drawn architecture diagrams, and the system uses AI to identify AWS services, provide educational explanations, and generate infrastructure-as-code (Terraform). This bridges the gap between conceptual learning and practical implementation.

## Glossary

- **System**: The CloudSketch application
- **User**: A student or developer learning cloud architecture
- **Diagram**: A hand-drawn architecture diagram uploaded by the user
- **Bedrock_Service**: Amazon Bedrock AI service using Claude 3.5 Sonnet model
- **Analysis_Result**: The output containing identified services, explanations, and generated code
- **S3_Storage**: AWS S3 bucket for storing uploaded diagrams
- **Lambda_Backend**: AWS Lambda functions handling backend processing
- **Streamlit_Frontend**: The web interface built with Streamlit
- **Terraform_Code**: Infrastructure-as-code output in Terraform format
- **AWS_Service**: Any AWS cloud service (S3, Lambda, DynamoDB, etc.)

## Requirements

### Requirement 1: Image Upload and Storage

**User Story:** As a user, I want to upload hand-drawn architecture diagrams, so that I can get them analyzed and learn about cloud architecture patterns.

#### Acceptance Criteria

1. WHEN a user selects an image file, THE System SHALL accept common image formats (PNG, JPG, JPEG)
2. WHEN an image is uploaded, THE System SHALL validate the file size is under 10MB
3. WHEN a valid image is uploaded, THE System SHALL store it in S3_Storage with a unique identifier
4. WHEN an invalid file is uploaded, THE System SHALL display a descriptive error message and prevent processing
5. WHEN an image is stored, THE System SHALL return a confirmation with the storage location

### Requirement 2: AI-Powered Diagram Analysis

**User Story:** As a user, I want the system to analyze my hand-drawn diagrams using AI, so that I can understand what AWS services I've sketched.

#### Acceptance Criteria

1. WHEN a diagram is uploaded, THE System SHALL send the image to Bedrock_Service for analysis
2. WHEN Bedrock_Service analyzes a diagram, THE System SHALL identify all recognizable AWS_Service components
3. WHEN AWS services are identified, THE System SHALL extract the relationships and connections between services
4. WHEN the analysis completes, THE System SHALL return a structured Analysis_Result containing service names and relationships
5. IF Bedrock_Service returns an error, THEN THE System SHALL log the error and return a user-friendly error message

### Requirement 3: Educational Explanations

**User Story:** As a user learning cloud architecture, I want to receive educational explanations about my diagram, so that I can understand why certain services are used and how they work together.

#### Acceptance Criteria

1. WHEN AWS services are identified, THE System SHALL generate educational explanations for each service
2. WHEN generating explanations, THE System SHALL describe the purpose and common use cases of each AWS_Service
3. WHEN multiple services are connected, THE System SHALL explain the architectural patterns and best practices
4. WHEN explanations are generated, THE System SHALL use beginner-friendly language suitable for students
5. WHEN displaying explanations, THE System SHALL organize them by service with clear headings

### Requirement 4: Terraform Code Generation

**User Story:** As a user, I want to receive Terraform code based on my diagram, so that I can learn infrastructure-as-code and deploy the architecture.

#### Acceptance Criteria

1. WHEN AWS services are identified, THE System SHALL generate valid Terraform_Code for each service
2. WHEN generating Terraform_Code, THE System SHALL include resource definitions with appropriate configurations
3. WHEN services are connected, THE System SHALL generate the necessary IAM policies and permissions
4. WHEN Terraform_Code is generated, THE System SHALL include comments explaining each resource block
5. WHEN the code is complete, THE System SHALL provide it in a downloadable format

### Requirement 5: Frontend User Interface

**User Story:** As a user, I want an intuitive web interface, so that I can easily upload diagrams and view results without technical complexity.

#### Acceptance Criteria

1. WHEN a user visits the application, THE Streamlit_Frontend SHALL display a file upload widget
2. WHEN a user uploads a file, THE Streamlit_Frontend SHALL show a preview of the uploaded diagram
3. WHEN analysis is in progress, THE Streamlit_Frontend SHALL display a loading indicator
4. WHEN results are ready, THE Streamlit_Frontend SHALL display identified services, explanations, and generated code in organized sections
5. WHEN displaying Terraform_Code, THE Streamlit_Frontend SHALL provide syntax highlighting and a copy-to-clipboard button

### Requirement 6: Backend Processing

**User Story:** As a system administrator, I want the backend to handle requests efficiently using serverless architecture, so that the system scales automatically and minimizes costs.

#### Acceptance Criteria

1. WHEN a diagram upload request is received, THE Lambda_Backend SHALL process the request within 30 seconds
2. WHEN processing a diagram, THE Lambda_Backend SHALL coordinate between S3_Storage and Bedrock_Service
3. WHEN multiple requests arrive, THE Lambda_Backend SHALL handle them concurrently without blocking
4. WHEN an error occurs during processing, THE Lambda_Backend SHALL return appropriate HTTP status codes and error messages
5. WHEN processing completes, THE Lambda_Backend SHALL return a JSON response with all analysis results

### Requirement 7: Error Handling and Validation

**User Story:** As a user, I want clear error messages when something goes wrong, so that I can understand what happened and how to fix it.

#### Acceptance Criteria

1. WHEN a file format is unsupported, THE System SHALL display "Please upload a PNG, JPG, or JPEG image file"
2. WHEN a file exceeds size limits, THE System SHALL display "File size must be under 10MB"
3. WHEN Bedrock_Service cannot identify any services, THE System SHALL display "No AWS services detected. Please ensure your diagram clearly shows AWS components"
4. WHEN a network error occurs, THE System SHALL display "Connection error. Please check your internet and try again"
5. WHEN any error occurs, THE System SHALL log detailed error information for debugging while showing user-friendly messages

### Requirement 8: Security and Access Control

**User Story:** As a system administrator, I want secure handling of user uploads and API access, so that the system protects user data and prevents unauthorized access.

#### Acceptance Criteria

1. WHEN storing files in S3_Storage, THE System SHALL use server-side encryption
2. WHEN accessing Bedrock_Service, THE System SHALL use IAM roles with least-privilege permissions
3. WHEN generating presigned URLs for S3, THE System SHALL set expiration times of no more than 1 hour
4. WHEN processing user uploads, THE System SHALL scan for malicious content before analysis
5. WHEN handling API requests, THE Lambda_Backend SHALL validate all input parameters before processing
