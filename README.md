# ShopSmart - Your Ultimate Sales Management System

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Security Features](#security-features)
- [File Structure](#file-structure)
- [Usage Examples](#usage-examples)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

ShopSmart is a comprehensive sales management system built with Spring Boot that helps businesses manage their inventory, track sales, handle suppliers, and generate detailed reports. The system provides a secure, multi-user environment where each user can manage their own business data independently.

## ✨ Features

### 🔐 Authentication & Security
- **User Registration & Login**: Secure JWT-based authentication
- **Account Activation**: Email-based account activation system
- **Password Reset**: Forgot password functionality with email verification
- **Role-based Access**: User and Admin roles
- **Multi-user Isolation**: Each user sees only their own data

### 📦 Inventory Management
- **Product Management**: Add, edit, delete products with images
- **Stock Tracking**: Real-time stock quantity monitoring
- **Stock Alerts**: Low stock level notifications
- **Category Management**: Organize products by categories
- **Supplier Integration**: Link products to suppliers

### 💰 Sales Management
- **Sales Recording**: Track individual sales transactions
- **Sales Analytics**: Comprehensive sales reports and summaries
- **Profit Calculation**: Automatic profit margin calculations
- **Period-based Reports**: Daily, weekly, monthly sales analysis
- **Top Products**: Identify best-selling products

### 👥 Supplier Management
- **Supplier Directory**: Manage supplier information
- **Contact Details**: Store supplier contact information
- **Active/Inactive Status**: Track supplier availability
- **Category-based Filtering**: Organize suppliers by category

### 💳 Debt Management
- **Customer Debts**: Track outstanding customer payments
- **Due Date Tracking**: Monitor payment deadlines
- **Payment Status**: Mark debts as paid/unpaid
- **Debt History**: Maintain payment records

### 📊 Reporting & Analytics
- **Sales Reports**: Generate detailed sales reports
- **CSV Export**: Export data in CSV format
- **Category-wise Analysis**: Sales breakdown by product categories
- **Profit Analysis**: Detailed profit and margin reports
- **Period Comparison**: Compare sales across different time periods

## 🛠 Technology Stack

### Backend
- **Java 21**: Core programming language
- **Spring Boot 3.2.3**: Main framework
- **Spring Security**: Authentication and authorization
- **Spring Data JPA**: Database operations
- **MySQL**: Primary database
- **JWT**: Token-based authentication
- **Maven**: Dependency management

### Additional Libraries
- **Lombok**: Reduces boilerplate code
- **Jakarta Validation**: Input validation
- **JJWT**: JWT token handling
- **Spring Mail**: Email functionality

## 📋 Prerequisites

Before running ShopSmart, ensure you have the following installed:

- **Java 21** or higher
- **MySQL 8.0** or higher
- **Maven 3.6** or higher
- **Git** (for cloning the repository)

## 🚀 Installation & Setup

### 1. Clone the Repository
```bash
git clone <repository-url>
cd ShopSmart
```

### 2. Database Setup
```sql
CREATE DATABASE shopsmart;
CREATE USER 'shopsmart_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON shopsmart.* TO 'shopsmart_user'@'localhost';
FLUSH PRIVILEGES;
```

### 3. Configuration
Create or update `src/main/resources/application.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/shopsmart?createDatabaseIfNotExist=true&useSSL=false&serverTimezone=UTC
spring.datasource.username=shopsmart_user
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.properties.hibernate.format_sql=true

# JWT Configuration
jwt.secret=your_jwt_secret_key_here_make_it_long_and_secure
jwt.expiration=86400000

# Email Configuration (for account activation and password reset)
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true

# File Upload Configuration
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
product.images.upload-dir=./uploads

# Server Configuration
server.port=8080
```

### 4. Build and Run
```bash
# Clean and build the project
mvn clean install

# Run the application
mvn spring-boot:run
```

The application will start on `http://localhost:8080`

## 🔧 Configuration

### Environment Variables
You can override configuration using environment variables:

```bash
export SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/shopsmart
export SPRING_DATASOURCE_USERNAME=your_username
export SPRING_DATASOURCE_PASSWORD=your_password
export JWT_SECRET=your_jwt_secret
export SPRING_MAIL_USERNAME=your_email@gmail.com
export SPRING_MAIL_PASSWORD=your_app_password
```

### Email Setup
For Gmail, you need to:
1. Enable 2-Factor Authentication
2. Generate an App Password
3. Use the App Password in your configuration

## 📚 API Documentation

### Authentication Endpoints

#### Register User
```http
POST /api/auth/signup
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "password123",
  "confirmPassword": "password123",
  "shopName": "John's Shop",
  "address": {
    "street": "123 Main St",
    "city": "Springfield",
    "country": "USA",
    "postalCode": "12345"
  },
  "role": "USER"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

#### Activate Account
```http
POST /api/auth/activate-account?token=activation_token_here
```

#### Forgot Password
```http
POST /api/auth/forgot-password
Content-Type: application/json

{
  "email": "john@example.com"
}
```

#### Reset Password
```http
POST /api/auth/reset-password
Content-Type: application/json

{
  "token": "reset_token_here",
  "newPassword": "newpassword123"
}
```

### Product Management

#### Get All Products
```http
GET /inventory/all?page=0&size=10&sortBy=name&direction=asc
Authorization: Bearer <jwt_token>
```

#### Add Product
```http
POST /inventory/add
Content-Type: multipart/form-data
Authorization: Bearer <jwt_token>

name: "Product Name"
category: "Electronics"
quantity: 100
unit: "pieces"
costPrice: 50.00
sellingPrice: 75.00
supplier: "Supplier Name"
stockAlertLevel: 10
image: [file]
```

#### Update Product
```http
PUT /inventory/{id}
Content-Type: multipart/form-data
Authorization: Bearer <jwt_token>

name: "Updated Product Name"
category: "Electronics"
quantity: 150
unit: "pieces"
costPrice: 45.00
sellingPrice: 80.00
supplier: "Updated Supplier"
stockAlertLevel: 15
image: [file]
```

#### Delete Product
```http
DELETE /inventory/{id}
Authorization: Bearer <jwt_token>
```

### Sales Management

#### Create Sale
```http
POST /sales
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "productId": 1,
  "quantitySold": 5,
  "totalAmount": 375.00
}
```

#### Get Sales Summary
```http
GET /sales/summary
Authorization: Bearer <jwt_token>
```

#### Get Sales by Period
```http
GET /sales/summary/period?startDate=2024-01-01&endDate=2024-01-31
Authorization: Bearer <jwt_token>
```

### Supplier Management

#### Get All Suppliers
```http
GET /suppliers?page=0&size=10&sortBy=name&direction=asc
Authorization: Bearer <jwt_token>
```

#### Create Supplier
```http
POST /suppliers
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "name": "Supplier Name",
  "category": "Electronics",
  "contactPerson": "John Doe",
  "phoneNumber": "+1234567890",
  "email": "supplier@example.com",
  "address": "123 Supplier St",
  "city": "Supplier City",
  "state": "Supplier State",
  "postalCode": "12345",
  "country": "USA",
  "isActive": true
}
```

### Debt Management

#### Get All Debts
```http
GET /debts
Authorization: Bearer <jwt_token>
```

#### Create Debt
```http
POST /debts
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "customerName": "Customer Name",
  "amount": 500.00,
  "createdDate": "2024-01-01",
  "dueDate": "2024-02-01",
  "isPaid": false
}
```

### Reports

#### Generate Report
```http
POST /api/reports/generate
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "periodType": "monthly",
  "startDate": "2024-01-01",
  "endDate": "2024-01-31"
}
```

#### Export CSV Report
```http
POST /api/reports/export/csv
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "periodType": "monthly",
  "startDate": "2024-01-01",
  "endDate": "2024-01-31"
}
```

## 🗄 Database Schema

### Core Entities

#### User
- `id` (Primary Key)
- `firstName`, `lastName`, `email`
- `password` (encrypted)
- `shopName`, `role`
- `activated`, `activationToken`
- `resetToken`, `resetTokenExpiry`
- `address` (embedded)

#### Product
- `id` (Primary Key)
- `name`, `category`, `unit`
- `stockQuantity`, `stockAlertLevel`
- `costPrice`, `sellingPrice`
- `supplier`, `imageUrl`
- `user` (Foreign Key)

#### Sale
- `id` (Primary Key)
- `quantitySold`, `totalAmount`
- `saleDate`
- `user` (Foreign Key)
- `product` (Foreign Key)

#### Supplier
- `id` (Primary Key)
- `name`, `category`
- `contactPerson`, `phoneNumber`, `email`
- `address`, `city`, `state`, `postalCode`, `country`
- `isActive`
- `user` (Foreign Key)

#### Debt
- `id` (Primary Key)
- `customerName`, `amount`
- `createdDate`, `dueDate`
- `isPaid`
- `user` (Foreign Key)

## 🔒 Security Features

### JWT Authentication
- Secure token-based authentication
- Token expiration (24 hours by default)
- Automatic token validation on protected endpoints

### Password Security
- BCrypt password hashing
- Password validation (minimum 6 characters)
- Secure password reset flow

### Data Isolation
- Multi-tenant architecture
- User-specific data access
- No cross-user data leakage

### Input Validation
- Comprehensive input validation
- SQL injection prevention
- XSS protection

## 📁 File Structure

```
ShopSmart/
├── src/
│   ├── main/
│   │   ├── java/com/amnii/ShopSmart/
│   │   │   ├── Controller/          # REST controllers
│   │   │   ├── DTO/                 # Data Transfer Objects
│   │   │   ├── Exception/           # Custom exceptions
│   │   │   ├── Models/              # Entity classes
│   │   │   ├── Repository/          # Data access layer
│   │   │   ├── Security/            # Security configuration
│   │   │   ├── Services/            # Business logic
│   │   │   ├── JwtService.java      # JWT utilities
│   │   │   └── ShopSmartApplication.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/                        # Test files
├── uploads/                         # File uploads
├── pom.xml                          # Maven configuration
└── README.md                        # This file
```

## 💡 Usage Examples

### Frontend Integration

#### JavaScript/React Example
```javascript
// Login
const login = async (email, password) => {
  const response = await fetch('/api/auth/login', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ email, password }),
  });
  const data = await response.json();
  localStorage.setItem('token', data.token);
  return data;
};

// Get Products
const getProducts = async () => {
  const token = localStorage.getItem('token');
  const response = await fetch('/inventory/all', {
    headers: {
      'Authorization': `Bearer ${token}`,
    },
  });
  return await response.json();
};
```

#### cURL Examples
```bash
# Login
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password123"}'

# Get Products (with token)
curl -X GET http://localhost:8080/inventory/all \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## 🔧 Troubleshooting

### Common Issues

#### 1. Database Connection Issues
```bash
# Check MySQL service
sudo systemctl status mysql

# Check database credentials
mysql -u shopsmart_user -p
```

#### 2. JWT Token Issues
- Ensure the JWT secret is properly configured
- Check token expiration
- Verify token format in Authorization header

#### 3. Email Not Sending
- Verify SMTP configuration
- Check Gmail App Password
- Ensure 2FA is enabled for Gmail

#### 4. File Upload Issues
- Check upload directory permissions
- Verify file size limits
- Ensure multipart configuration

#### 5. 500 Internal Server Error
- Check application logs for stack traces
- Verify all required fields are provided
- Ensure user authentication

### Logs
Application logs are available in the console when running the application. For production, configure proper logging:

```properties
# Logging configuration
logging.level.com.amnii.ShopSmart=DEBUG
logging.file.name=logs/shopsmart.log
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow Java coding conventions
- Add proper documentation
- Include unit tests for new features
- Ensure all tests pass before submitting

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For support and questions:
- Create an issue in the repository
- Contact the development team
- Check the troubleshooting section above

---

**ShopSmart** - Empowering businesses with smart sales management solutions. 
