# 🍽️ AtlasReserve - Restaurant Reservation Platform

[![Java](https://img.shields.io/badge/Java-17-orange)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.3-brightgreen)](https://spring.io/projects/spring-boot)
[![Next.js](https://img.shields.io/badge/Next.js-15.2.0-black)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19.0.0-blue)](https://reactjs.org/)
[![AWS](https://img.shields.io/badge/AWS-Cloud-orange)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-blue)](https://www.docker.com/)

## 📋 Table of Contents
- [🌟 Project Overview](#-project-overview)
- [✨ Key Features](#-key-features)
- [🏗️ System Architecture](#️-system-architecture)
- [🚀 Technology Stack](#-technology-stack)
- [📐 Database Design](#-database-design)
- [🔐 Security & Authentication](#-security--authentication)
- [☁️ Cloud Infrastructure](#️-cloud-infrastructure)
- [🛠️ Installation & Setup](#️-installation--setup)
- [🚀 Deployment](#-deployment)
- [📱 API Documentation](#-api-documentation)
- [🧪 Testing](#-testing)
- [📈 Performance & Monitoring](#-performance--monitoring)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

## 🌟 Project Overview

**AtlasReserve** is a comprehensive, full-stack restaurant reservation platform designed to revolutionize the dining experience. Built with modern technologies and cloud-native architecture, it provides seamless booking experiences for customers, efficient management tools for restaurant owners, and powerful administrative capabilities.

### 🎯 Purpose
To create an intuitive, scalable, and reliable platform that connects diners with restaurants while providing robust management tools for all stakeholders in the restaurant ecosystem.

## ✨ Key Features

### 👥 Multi-Role User Management
- **Customers**: Browse, book, and manage reservations
- **Restaurant Managers**: Manage restaurant details, view bookings, and analytics
- **Administrators**: Oversee platform operations and restaurant approvals

### 🔍 Advanced Restaurant Discovery
- **Location-based Search**: Find restaurants using geospatial queries
- **Interactive Maps**: Google Maps integration for location visualization
- **Smart Filters**: Filter by cuisine, ratings, availability, and distance
- **Real-time Availability**: Live table availability checking

### 📅 Intelligent Booking System
- **Real-time Conflict Detection**: Prevents double bookings
- **Email Confirmations**: Automated confirmation and reminder emails
- **Flexible Cancellation**: Easy booking modifications and cancellations
- **Time Slot Management**: Dynamic time slot generation based on restaurant capacity

### ⭐ Review & Rating System
- **Authenticated Reviews**: Verified customer reviews and ratings
- **Aggregate Scores**: Comprehensive rating analytics
- **Review Moderation**: Admin-controlled review management

### 🖼️ Media Management
- **Secure Image Uploads**: AWS S3 integration with pre-signed URLs
- **Image Optimization**: Automatic image processing and optimization
- **CDN Distribution**: Fast image delivery via CloudFront

## 🏗️ System Architecture

### High-Level Architecture Diagram

```mermaid
graph TB
    subgraph "Client Layer"
        WEB[Web Browser]
        MOBILE[Mobile App]
    end
    
    subgraph "CDN & Load Balancing"
        CF[CloudFront CDN]
        ALB[Application Load Balancer]
    end
    
    subgraph "Application Layer"
        subgraph "Frontend"
            NEXTJS[Next.js Application]
            REACT[React Components]
            MUI[Material-UI]
        end
        
        subgraph "Backend"
            SB[Spring Boot API]
            SEC[Spring Security]
            JPA[Spring Data JPA]
        end
    end
    
    subgraph "Authentication"
        COGNITO[AWS Cognito]
        JWT[JWT Tokens]
    end
    
    subgraph "Data Layer"
        MYSQL[(MySQL Database)]
        REDIS[(Redis Cache)]
    end
    
    subgraph "External Services"
        S3[AWS S3]
        SES[AWS SES]
        GMAPS[Google Maps API]
        LAMBDA[AWS Lambda]
    end
    
    subgraph "Infrastructure"
        EB[Elastic Beanstalk]
        ECR[Elastic Container Registry]
        EC2[EC2 Instances]
        ASG[Auto Scaling Group]
    end
    
    WEB --> CF
    MOBILE --> CF
    CF --> ALB
    ALB --> NEXTJS
    NEXTJS --> SB
    SB --> COGNITO
    SB --> MYSQL
    SB --> REDIS
    SB --> S3
    SB --> SES
    SB --> GMAPS
    COGNITO --> LAMBDA
    SB --> EB
    EB --> ECR
    EB --> EC2
    EB --> ASG
```

### Microservices Architecture

```mermaid
graph LR
    subgraph "API Gateway"
        GW[Spring Cloud Gateway]
    end
    
    subgraph "Core Services"
        AUTH[Authentication Service]
        REST[Restaurant Service]
        BOOK[Booking Service]
        USER[User Service]
        REVIEW[Review Service]
        ADMIN[Admin Service]
    end
    
    subgraph "Supporting Services"
        EMAIL[Email Service]
        PHOTO[Photo Service]
        GEO[Geocoding Service]
        NOTIF[Notification Service]
    end
    
    subgraph "Data Stores"
        DB1[(User DB)]
        DB2[(Restaurant DB)]
        DB3[(Booking DB)]
        DB4[(Review DB)]
        CACHE[(Redis Cache)]
    end
    
    GW --> AUTH
    GW --> REST
    GW --> BOOK
    GW --> USER
    GW --> REVIEW
    GW --> ADMIN
    
    AUTH --> DB1
    REST --> DB2
    BOOK --> DB3
    REVIEW --> DB4
    
    BOOK --> EMAIL
    REST --> PHOTO
    REST --> GEO
    BOOK --> NOTIF
    
    AUTH --> CACHE
    REST --> CACHE
    BOOK --> CACHE
```

### Data Flow Architecture

```mermaid
sequenceDiagram
    participant Customer
    participant Frontend
    participant API Gateway
    participant Auth Service
    participant Restaurant Service
    participant Booking Service
    participant Email Service
    participant Database
    
    Customer->>Frontend: Search Restaurants
    Frontend->>API Gateway: GET /restaurants/search
    API Gateway->>Auth Service: Validate JWT
    Auth Service-->>API Gateway: Token Valid
    API Gateway->>Restaurant Service: Search Request
    Restaurant Service->>Database: Spatial Query
    Database-->>Restaurant Service: Restaurant Data
    Restaurant Service-->>API Gateway: Search Results
    API Gateway-->>Frontend: Restaurant List
    Frontend-->>Customer: Display Results
    
    Customer->>Frontend: Make Booking
    Frontend->>API Gateway: POST /bookings
    API Gateway->>Booking Service: Create Booking
    Booking Service->>Database: Check Availability
    Database-->>Booking Service: Availability Status
    Booking Service->>Database: Create Booking
    Booking Service->>Email Service: Send Confirmation
    Email Service-->>Customer: Confirmation Email
    Booking Service-->>API Gateway: Booking Created
    API Gateway-->>Frontend: Success Response
```

## 🚀 Technology Stack

### Frontend Stack
```mermaid
graph LR
    subgraph "Frontend Technologies"
        NEXT[Next.js 15.2.0]
        REACT[React 19.0.0]
        MUI[Material-UI 7.0.1]
        SASS[SASS/SCSS]
        AXIOS[Axios HTTP Client]
    end
    
    subgraph "Development Tools"
        ESLINT[ESLint]
        PRETTIER[Prettier]
        HUSKY[Husky Git Hooks]
    end
    
    NEXT --> REACT
    REACT --> MUI
    MUI --> SASS
    REACT --> AXIOS
```

### Backend Stack
```mermaid
graph TB
    subgraph "Framework"
        SB[Spring Boot 3.4.3]
        JAVA[Java 17]
        GRADLE[Gradle Build Tool]
    end
    
    subgraph "Spring Ecosystem"
        WEB[Spring Web]
        SEC[Spring Security]
        DATA[Spring Data JPA]
        OAUTH[OAuth2 Resource Server]
        ACTUATOR[Spring Actuator]
    end
    
    subgraph "Database"
        MYSQL[MySQL 8.0]
        HCP[HikariCP Pool]
        JDBC[JDBC Driver]
    end
    
    subgraph "Testing"
        JUNIT[JUnit 5]
        MOCKITO[Mockito]
        TESTCON[TestContainers]
        JACOCO[JaCoCo Coverage]
    end
    
    JAVA --> SB
    SB --> WEB
    SB --> SEC
    SB --> DATA
    SB --> OAUTH
    SB --> ACTUATOR
    DATA --> MYSQL
    MYSQL --> HCP
    MYSQL --> JDBC
```

### Cloud & DevOps
```mermaid
graph TD
    subgraph "AWS Services"
        EB[Elastic Beanstalk]
        ECR[Container Registry]
        S3[S3 Storage]
        SES[Simple Email Service]
        COGNITO[Cognito Auth]
        LAMBDA[Lambda Functions]
        CF[CloudFront CDN]
        RDS[RDS MySQL]
    end
    
    subgraph "CI/CD"
        GHA[GitHub Actions]
        DOCKER[Docker]
        CODEQL[CodeQL Security]
    end
    
    subgraph "Monitoring"
        CW[CloudWatch]
        XR[X-Ray Tracing]
        ALB[Application Load Balancer]
    end
```

## 📐 Database Design

### Entity Relationship Diagram

```mermaid
erDiagram
    USER {
        string user_id PK
        string email
        string phone_number
        string first_name
        string last_name
        enum role
        timestamp created_at
        timestamp updated_at
    }
    
    RESTAURANT {
        int restaurant_id PK
        string name
        string description
        string address
        decimal latitude
        decimal longitude
        string phone
        string email
        string cuisine_type
        int capacity
        string status
        string manager_id FK
        timestamp created_at
        timestamp updated_at
    }
    
    RESTAURANT_HOURS {
        int hours_id PK
        int restaurant_id FK
        enum day_of_week
        time opening_time
        time closing_time
        boolean is_closed
    }
    
    TABLE_ENTITY {
        int table_id PK
        int restaurant_id FK
        int table_number
        int capacity
        string table_type
        boolean is_active
    }
    
    TIME_SLOT {
        int slot_id PK
        int restaurant_id FK
        time start_time
        time end_time
        int duration_minutes
        boolean is_active
    }
    
    BOOKING {
        int booking_id PK
        string customer_id FK
        int restaurant_id FK
        int table_id FK
        int time_slot_id FK
        date booking_date
        int party_size
        enum status
        string special_requests
        timestamp booking_time
        timestamp created_at
        timestamp updated_at
    }
    
    REVIEW {
        int review_id PK
        string customer_id FK
        int restaurant_id FK
        int booking_id FK
        int rating
        string comment
        timestamp created_at
        timestamp updated_at
    }
    
    PHOTO {
        int photo_id PK
        int restaurant_id FK
        string s3_key
        string filename
        string content_type
        long file_size
        boolean is_primary
        timestamp uploaded_at
    }
    
    USER ||--o{ BOOKING : places
    USER ||--o{ REVIEW : writes
    USER ||--|| RESTAURANT : manages
    RESTAURANT ||--o{ RESTAURANT_HOURS : has
    RESTAURANT ||--o{ TABLE_ENTITY : contains
    RESTAURANT ||--o{ TIME_SLOT : offers
    RESTAURANT ||--o{ BOOKING : receives
    RESTAURANT ||--o{ REVIEW : receives
    RESTAURANT ||--o{ PHOTO : displays
    TABLE_ENTITY ||--o{ BOOKING : reserved_for
    TIME_SLOT ||--o{ BOOKING : scheduled_at
    BOOKING ||--|| REVIEW : generates
```

## 🔐 Security & Authentication

### Authentication Flow

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant API Gateway
    participant Cognito
    participant Lambda
    participant Backend
    
    User->>Frontend: Login Request
    Frontend->>Cognito: Initiate Auth
    Cognito->>Lambda: Custom Auth Challenge
    Lambda->>User: Send OTP (Email/SMS)
    User->>Frontend: Enter OTP
    Frontend->>Cognito: Verify OTP
    Cognito->>Lambda: Verify Auth Challenge
    Lambda-->>Cognito: Challenge Success
    Cognito-->>Frontend: JWT Tokens
    Frontend->>API Gateway: Request with JWT
    API Gateway->>Backend: Validated Request
    Backend-->>Frontend: Protected Resource
```

### Authorization Matrix

| Role | Restaurant Management | Booking Management | User Management | Admin Functions |
|------|----------------------|-------------------|----------------|----------------|
| **Customer** | ❌ View Only | ✅ Own Bookings | ✅ Own Profile | ❌ |
| **Restaurant Manager** | ✅ Own Restaurant | ✅ Restaurant Bookings | ✅ Own Profile | ❌ |
| **Admin** | ✅ All Restaurants | ✅ All Bookings | ✅ All Users | ✅ Platform |

## ☁️ Cloud Infrastructure

### AWS Architecture Diagram

```mermaid
graph TB
    subgraph "Global"
        ROUTE53[Route 53 DNS]
        CF[CloudFront CDN]
    end
    
    subgraph "Region: us-west-2"
        subgraph "Availability Zone A"
            EC2A[EC2 Instance]
            RDSA[RDS Primary]
        end
        
        subgraph "Availability Zone B"
            EC2B[EC2 Instance]
            RDSB[RDS Standby]
        end
        
        ALB[Application Load Balancer]
        EB[Elastic Beanstalk]
        ECR[Container Registry]
        
        subgraph "Storage"
            S3[S3 Bucket]
            EFS[EFS File System]
        end
        
        subgraph "Security & Auth"
            COGNITO[Cognito User Pool]
            SECRETS[Secrets Manager]
            IAM[IAM Roles]
        end
        
        subgraph "Monitoring"
            CW[CloudWatch]
            XR[X-Ray]
        end
    end
    
    ROUTE53 --> CF
    CF --> ALB
    ALB --> EC2A
    ALB --> EC2B
    EB --> EC2A
    EB --> EC2B
    EB --> ECR
    EC2A --> RDSA
    EC2B --> RDSA
    RDSA --> RDSB
    EC2A --> S3
    EC2B --> S3
```

## 🛠️ Installation & Setup

### Prerequisites

- **Node.js** (v18.x or v20.x)
- **Java Development Kit** (JDK 17)
- **Docker** and **Docker Compose**
- **AWS CLI** (configured with credentials)
- **MySQL** 8.0 or compatible database

### Environment Setup

#### Backend Configuration

Create `backend/bookTable/src/main/resources/application-local.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/booktable?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=password

# AWS Configuration (use local/test values for development)
aws.accessKeyId=your-access-key
aws.secretKey=your-secret-key
aws.region=us-west-2

# AWS Cognito Configuration
spring.security.oauth2.resourceserver.jwt.issuer-uri=https://cognito-idp.us-west-2.amazonaws.com/your-user-pool-id
cognito.userPoolId=your-user-pool-id
cognito.clientId=your-client-id
cognito.clientSecret=your-client-secret

# S3 Configuration
aws.s3.bucket=your-s3-bucket-name

# Google Maps API
google.api.key=your-google-maps-api-key

# Email Configuration
aws.ses.from-email=noreply@yourapp.com
```

#### Frontend Configuration

Create `open-table-frontend/.env.local`:

```bash
NEXT_PUBLIC_BASE_URL=http://localhost:8080
NEXT_PUBLIC_PLACES_API_KEY=your-google-places-api-key
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your-google-maps-api-key
```

### Quick Start with Docker

```bash
# Clone the repository
git clone https://github.com/Mrnidhi/AtlasReserve.git
cd AtlasReserve

# Start all services using Docker Compose
docker-compose up -d

# The application will be available at:
# Frontend: http://localhost:3000
# Backend API: http://localhost:8080
# MySQL: localhost:3306
```

### Manual Setup

#### Backend Setup

```bash
# Navigate to backend directory
cd backend/bookTable

# Make gradlew executable (Unix/Linux/MacOS)
chmod +x ./gradlew

# Build the application
./gradlew clean build

# Run tests
./gradlew test

# Start the application
./gradlew bootRun
```

#### Frontend Setup

```bash
# Navigate to frontend directory
cd open-table-frontend

# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build
npm start
```

### Database Setup

```sql
-- Create database
CREATE DATABASE booktable CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Create user (optional)
CREATE USER 'booktable_user'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON booktable.* TO 'booktable_user'@'localhost';
FLUSH PRIVILEGES;
```

### Verification

After setup, verify the installation:

```bash
# Check backend health
curl http://localhost:8080/actuator/health

# Check frontend
curl http://localhost:3000

# Check database connection
mysql -u root -p -e "SHOW DATABASES;"
```

## 🚀 Deployment

### AWS Deployment Architecture

```mermaid
graph TB
    subgraph "Development"
        DEV_CODE[Local Development]
        GIT[Git Repository]
    end
    
    subgraph "CI/CD Pipeline"
        GHA[GitHub Actions]
        BUILD[Build & Test]
        DOCKER[Docker Build]
        ECR_PUSH[Push to ECR]
        DEPLOY[Deploy to EB]
    end
    
    subgraph "AWS Production"
        EB[Elastic Beanstalk]
        ALB[Load Balancer]
        EC2[EC2 Instances]
        RDS[RDS MySQL]
        S3[S3 Storage]
    end
    
    DEV_CODE --> GIT
    GIT --> GHA
    GHA --> BUILD
    BUILD --> DOCKER
    DOCKER --> ECR_PUSH
    ECR_PUSH --> DEPLOY
    DEPLOY --> EB
    EB --> ALB
    ALB --> EC2
    EC2 --> RDS
    EC2 --> S3
```

## 📱 API Documentation

### Core API Endpoints

#### Authentication Endpoints
```http
POST /api/auth/login
POST /api/auth/register
POST /api/auth/verify-otp
POST /api/auth/logout
GET  /api/auth/status
```

#### Restaurant Management
```http
GET    /api/restaurants/search?lat={lat}&lng={lng}&radius={radius}
GET    /api/restaurants/{id}
GET    /api/restaurants/{id}/availability?date={date}
POST   /api/restaurants
PUT    /api/restaurants/{id}
DELETE /api/restaurants/{id}
```

#### Booking Operations
```http
GET    /api/bookings
POST   /api/bookings
GET    /api/bookings/{id}
PUT    /api/bookings/{id}
DELETE /api/bookings/{id}
GET    /api/bookings/restaurant/{restaurantId}
```

#### Review System
```http
GET    /api/reviews/restaurant/{restaurantId}
POST   /api/reviews
PUT    /api/reviews/{id}
DELETE /api/reviews/{id}
```

### API Response Format

```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": {
    "id": 123,
    "name": "Sample Restaurant",
    "location": {
      "latitude": 37.7749,
      "longitude": -122.4194
    },
    "rating": 4.5,
    "reviews_count": 150
  },
  "metadata": {
    "page": 1,
    "limit": 20,
    "total": 500,
    "total_pages": 25
  },
  "timestamp": "2024-01-01T12:00:00Z"
}
```

## 🧪 Testing

### Testing Strategy

```mermaid
pyramid
    title Testing Pyramid
    
    section E2E Tests
      Browser Tests: 5%
      API Integration: 10%
    
    section Integration Tests
      Database Tests: 15%
      Service Tests: 20%
    
    section Unit Tests
      Component Tests: 25%
      Service Logic: 25%
```

### Test Coverage Goals

| Component | Coverage Target |
|-----------|----------------|
| Service Layer | 90%+ |
| Controller Layer | 85%+ |
| Repository Layer | 80%+ |
| React Components | 80%+ |
| Utility Functions | 95%+ |

## 📈 Performance & Monitoring

### Performance Optimization

```mermaid
graph TB
    subgraph "Frontend Optimization"
        CODE_SPLIT[Code Splitting]
        LAZY_LOAD[Lazy Loading]
        IMAGE_OPT[Image Optimization]
        CACHING[Browser Caching]
    end
    
    subgraph "Backend Optimization"
        DB_INDEX[Database Indexing]
        CONN_POOL[Connection Pooling]
        CACHE_LAYER[Redis Caching]
        ASYNC_PROC[Async Processing]
    end
    
    subgraph "Infrastructure"
        CDN[CloudFront CDN]
        LOAD_BAL[Load Balancing]
        AUTO_SCALE[Auto Scaling]
        DB_READ_REP[Read Replicas]
    end
```

## 🤝 Contributing

### Development Workflow

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Git Commit Convention

```
type(scope): subject

[optional body]

[optional footer]
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

Examples:
```
feat(booking): add real-time availability checking
fix(auth): resolve JWT token expiration issue
docs(api): update restaurant endpoint documentation
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🚀 Quick Start Commands

```bash
# Development setup
git clone https://github.com/Mrnidhi/AtlasReserve.git
cd AtlasReserve
docker-compose up -d

# Backend only
cd backend/bookTable && ./gradlew bootRun

# Frontend only
cd open-table-frontend && npm install && npm run dev

# Run tests
./gradlew test                    # Backend tests
npm test                         # Frontend tests

# Production build
docker build -t atlasreserve .   # Build production image
```

## 📞 Support & Contact

- **Documentation**: [Project Wiki](https://github.com/Mrnidhi/AtlasReserve/wiki)
- **Issues**: [GitHub Issues](https://github.com/Mrnidhi/AtlasReserve/issues)
- **Discussions**: [GitHub Discussions](https://github.com/Mrnidhi/AtlasReserve/discussions)

---

<div align="center">

**Built with ❤️ using modern web technologies**

[⭐ Star this repository](https://github.com/Mrnidhi/AtlasReserve) | [🐛 Report Bug](https://github.com/Mrnidhi/AtlasReserve/issues) | [💡 Request Feature](https://github.com/Mrnidhi/AtlasReserve/issues)

</div>
