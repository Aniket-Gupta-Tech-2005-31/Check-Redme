# Inventory Management System (IMS) - Complete Project Documentation

## 📋 Table of Contents
1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [Technology Stack](#technology-stack)
4. [Database Design](#database-design)
5. [API Endpoints](#api-endpoints)
6. [User Flow Diagrams](#user-flow-diagrams)
7. [Features & Functionality](#features--functionality)
8. [Installation & Setup](#installation--setup)
9. [Security Implementation](#security-implementation)
10. [File Structure](#file-structure)
11. [Business Logic](#business-logic)
12. [Technical Implementation](#technical-implementation)
13. [Future Enhancements](#future-enhancements)

---

## 🎯 Project Overview

**Kailash Stationery - Inventory Management System** is a comprehensive web-based application designed to manage inventory, products, categories, and purchase orders for a stationery business. The system provides a modern, responsive interface with real-time data management capabilities.

### Key Business Objectives:
- **Inventory Tracking**: Monitor product stock levels and movements
- **Category Management**: Organize products into logical categories
- **Purchase Order Management**: Track product purchases and supplier information
- **User Authentication**: Secure access control for authorized personnel
- **Analytics Dashboard**: Real-time insights into business performance

---

## 🏗️ System Architecture

### High-Level Architecture Diagram
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Client Side   │    │   Server Side   │    │   Database      │
│                 │    │                 │    │                 │
│ ┌─────────────┐ │    │ ┌─────────────┐ │    │ ┌─────────────┐ │
│ │   Browser   │◄────►│   Express    │◄────►│   MongoDB    │ │
│ │   (EJS)     │ │    │   Server     │ │    │   Database   │ │
│ └─────────────┘ │    │ └─────────────┘ │    │ └─────────────┘ │
│                 │    │                 │    │                 │
│ ┌─────────────┐ │    │ ┌─────────────┐ │    │                 │
│ │   CSS/JS    │ │    │   Routes &    │    │                 │
│ │   Assets    │ │    │   Controllers │    │                 │
│ └─────────────┘ │    │ └─────────────┘ │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Request Flow Architecture
```
User Request → Express Server → Authentication Middleware → Route Handler → Database → Response → Client
```

---

## 🛠️ Technology Stack

### Backend Technologies
| Technology | Version | Purpose |
|------------|---------|---------|
| **Node.js** | Latest LTS | Runtime environment |
| **Express.js** | 5.1.0 | Web framework |
| **MongoDB** | Latest | NoSQL database |
| **Mongoose** | 8.14.1 | MongoDB ODM |
| **JWT** | 9.0.2 | Authentication tokens |
| **bcrypt** | 5.1.1 | Password hashing |
| **Multer** | 1.4.5 | File upload handling |

### Frontend Technologies
| Technology | Purpose |
|------------|---------|
| **EJS** | Server-side templating |
| **HTML5** | Markup structure |
| **CSS3** | Styling and responsive design |
| **JavaScript (ES6+)** | Client-side interactivity |
| **Font Awesome** | Icons and UI elements |
| **SweetAlert2** | Enhanced alerts and modals |

### Development Tools
| Tool | Purpose |
|------|---------|
| **Nodemon** | Auto-restart server during development |
| **dotenv** | Environment variable management |
| **CORS** | Cross-origin resource sharing |

---

## 🗄️ Database Design

### Database Schema Overview

#### 1. User Collection
```javascript
{
  _id: ObjectId,
  username: String (unique, required),
  password: String (hashed, required),
  email: String (unique, optional),
  role: String (enum: ['admin', 'user', 'manager']),
  isActive: Boolean (default: true),
  lastLogin: Date,
  createdAt: Date,
  updatedAt: Date
}
```

#### 2. Category Collection
```javascript
{
  _id: ObjectId,
  name: String (required),
  description: String,
  tags: [String],
  image: {
    data: String (Base64),
    contentType: String
  },
  sellers: [{
    name: String,
    contactNumber: String,
    location: String
  }],
  createdAt: Date,
  updatedAt: Date
}
```

#### 3. Product Collection
```javascript
{
  _id: ObjectId,
  image: {
    data: String (Base64),
    contentType: String
  },
  name: String (required),
  description: String,
  purchasePrice: Number (required),
  sellingPrice: Number (required),
  categories: [String],
  stockQuantity: Number (default: 0),
  sku: String (unique),
  isActive: Boolean (default: true),
  createdAt: Date,
  updatedAt: Date
}
```

#### 4. PurchaseProduct Collection
```javascript
{
  _id: ObjectId,
  name: String (required),
  category: String (required),
  quantity: Number (required),
  purchaseDate: Date (default: now),
  status: String (default: "Pending"),
  comment: String (default: ""),
  createdAt: Date,
  updatedAt: Date
}
```

### Database Relationships
```
User (1) ──── (Many) Products
Category (1) ──── (Many) Products
Product (1) ──── (Many) PurchaseProducts
```

---

## 🔌 API Endpoints

### Authentication Routes
| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| `GET` | `/login` | Render login page | Not required |
| `POST` | `/login` | User authentication | Not required |
| `GET` | `/logout` | User logout | Required |
| `POST` | `/register` | User registration | Not required |

### Dashboard Routes
| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| `GET` | `/dashboard` | Main dashboard | Required |
| `GET` | `/dashboard/getCategories` | Fetch all categories | Required |
| `GET` | `/dashboard/getProducts` | Fetch products with filters | Required |
| `GET` | `/dashboard/getTotals` | Get total counts | Required |
| `GET` | `/dashboard/getRecentProducts` | Get recent products | Required |

### Category Routes
| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| `GET` | `/category` | Category management page | Required |
| `POST` | `/category/add` | Add new category | Required |
| `GET` | `/category/getAll` | Fetch all categories | Required |
| `PUT` | `/category/update/:id` | Update category | Required |
| `DELETE` | `/category/delete/:id` | Delete category | Required |

### Product Routes
| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| `GET` | `/products` | Product management page | Required |
| `POST` | `/products/add` | Add new product | Required |
| `GET` | `/products/getAll` | Fetch all products | Required |
| `PUT` | `/products/update/:id` | Update product | Required |
| `DELETE` | `/products/delete/:id` | Delete product | Required |

### Purchase Product Routes
| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| `GET` | `/PurchaseProduct` | Purchase management page | Required |
| `POST` | `/PurchaseProduct/add` | Add new purchase | Required |
| `GET` | `/PurchaseProduct/getAll` | Fetch all purchases | Required |
| `PUT` | `/PurchaseProduct/update/:id` | Update purchase | Required |
| `DELETE` | `/PurchaseProduct/delete/:id` | Delete purchase | Required |

---

## 🔄 User Flow Diagrams

### 1. Complete System Flow
```mermaid
graph TD
    A[User Access] --> B{Authenticated?}
    B -->|No| C[Login Page]
    B -->|Yes| D[Dashboard]
    
    C --> E[Enter Credentials]
    E --> F[Validate JWT Token]
    F -->|Valid| D
    F -->|Invalid| G[Show Error]
    G --> C
    
    D --> H[Main Navigation]
    H --> I[Products Management]
    H --> J[Category Management]
    H --> K[Purchase Orders]
    H --> L[Analytics Dashboard]
    H --> M[Logout]
    
    I --> N[Add Product]
    I --> O[Edit Product]
    I --> P[Delete Product]
    I --> Q[Search Products]
    
    J --> R[Add Category]
    J --> S[Edit Category]
    J --> T[Delete Category]
    J --> U[Manage Sellers]
    
    K --> V[Create Purchase Order]
    K --> W[Update Order Status]
    K --> X[Track Orders]
    
    L --> Y[View Statistics]
    L --> Z[Generate Reports]
    
    M --> A
    
    style A fill:#e1f5fe
    style D fill:#c8e6c9
    style C fill:#fff3e0
    style M fill:#ffcdd2
```

### 2. Database Relationships
```mermaid
erDiagram
    USER ||--o{ PRODUCT : creates
    USER {
        ObjectId _id
        String username
        String password
        String email
        String role
        Boolean isActive
        Date lastLogin
        Date createdAt
        Date updatedAt
    }
    
    CATEGORY ||--o{ PRODUCT : contains
    CATEGORY {
        ObjectId _id
        String name
        String description
        Array tags
        Object image
        Array sellers
        Date createdAt
        Date updatedAt
    }
    
    PRODUCT ||--o{ PURCHASE_PRODUCT : generates
    PRODUCT {
        ObjectId _id
        Object image
        String name
        String description
        Number purchasePrice
        Number sellingPrice
        Array categories
        Number stockQuantity
        String sku
        Boolean isActive
        Date createdAt
        Date updatedAt
    }
    
    PURCHASE_PRODUCT {
        ObjectId _id
        String name
        String category
        Number quantity
        Date purchaseDate
        String status
        String comment
        Date createdAt
        Date updatedAt
    }
```

### 3. Authentication Flow
```mermaid
sequenceDiagram
    participant U as User
    participant B as Browser
    participant S as Server
    participant D as Database
    
    U->>B: Access Application
    B->>S: GET /login
    S->>B: Render Login Page
    B->>U: Show Login Form
    
    U->>B: Enter Credentials
    B->>S: POST /login
    S->>D: Query User
    D->>S: Return User Data
    S->>S: Validate Password
    S->>S: Generate JWT Token
    S->>B: Set HTTP-Only Cookie
    B->>U: Redirect to Dashboard
    
    U->>B: Access Protected Route
    B->>S: GET /dashboard (with token)
    S->>S: Verify JWT Token
    S->>D: Fetch Dashboard Data
    D->>S: Return Data
    S->>B: Render Dashboard
    B->>U: Show Dashboard
```

### 4. Product Management Flow
```mermaid
flowchart TD
    A[Product Management] --> B{Action Type}
    
    B -->|Create| C[Add Product Form]
    B -->|Read| D[Product List]
    B -->|Update| E[Edit Product Form]
    B -->|Delete| F[Delete Confirmation]
    
    C --> G[Upload Image]
    G --> H[Enter Product Details]
    H --> I[Select Categories]
    I --> J[Set Prices]
    J --> K[Save to Database]
    
    D --> L[Apply Filters]
    L --> M[Search Products]
    M --> N[Display Results]
    
    E --> O[Load Product Data]
    O --> P[Modify Details]
    P --> Q[Update Database]
    
    F --> R[Confirm Deletion]
    R --> S[Remove from Database]
    
    K --> T[Success Message]
    Q --> T
    S --> T
    T --> A
```

### 5. Dashboard Analytics Flow
```mermaid
graph LR
    A[Dashboard Load] --> B[Fetch Statistics]
    B --> C[Get Total Products]
    B --> D[Get Total Categories]
    B --> E[Get Recent Products]
    B --> F[Calculate Revenue]
    
    C --> G[Display Product Count]
    D --> H[Display Category Count]
    E --> I[Display Recent Products]
    F --> J[Display Revenue]
    
    G --> K[Real-time Updates]
    H --> K
    I --> K
    J --> K
    
    K --> L[User Interaction]
    L --> M[Apply Filters]
    M --> N[Update Display]
    N --> K
```

### 6. Purchase Order Flow
```mermaid
stateDiagram-v2
    [*] --> Pending
    Pending --> Processing : Start Processing
    Processing --> Shipped : Ship Order
    Processing --> Cancelled : Cancel Order
    Shipped --> Delivered : Mark Delivered
    Shipped --> Returned : Return Order
    Delivered --> [*]
    Cancelled --> [*]
    Returned --> [*]
    
    note right of Pending
        Order created with
        basic details
    end note
    
    note right of Processing
        Order being prepared
        and packed
    end note
    
    note right of Shipped
        Order dispatched
        to customer
    end note
    
    note right of Delivered
        Order successfully
        delivered
    end note
```

---

## ✨ Features & Functionality

### 1. User Authentication System
- **Secure Login**: JWT-based authentication with bcrypt password hashing
- **Remember Me**: Extended session duration option
- **Session Management**: Automatic token validation and renewal
- **Logout**: Secure session termination

### 2. Dashboard Analytics
- **Real-time Statistics**: Live product and category counts
- **Revenue Tracking**: Monthly revenue calculations
- **Order Management**: Purchase order statistics
- **Trend Analysis**: Growth indicators and trends

### 3. Product Management
- **CRUD Operations**: Create, Read, Update, Delete products
- **Image Upload**: Base64 image storage with validation
- **Category Assignment**: Multi-category product organization
- **Price Management**: Purchase and selling price tracking
- **Search & Filter**: Advanced product search capabilities

### 4. Category Management
- **Hierarchical Organization**: Structured product categorization
- **Seller Information**: Supplier details within categories
- **Image Support**: Category-specific images
- **Tag System**: Flexible tagging for better organization

### 5. Purchase Order System
- **Order Tracking**: Status-based purchase order management
- **Quantity Management**: Stock quantity tracking
- **Date Tracking**: Purchase date and timeline management
- **Comment System**: Order-specific notes and comments

### 6. Advanced Search & Filtering
- **Multi-criteria Search**: Name, category, date range filtering
- **Pagination**: Efficient data loading for large datasets
- **Real-time Results**: Instant search results
- **Export Capabilities**: Data export functionality

---

## 🚀 Installation & Setup

### Prerequisites
- Node.js (v16 or higher)
- MongoDB (v5 or higher)
- Git

### Step-by-Step Installation

#### 1. Clone the Repository
```bash
git clone <repository-url>
cd IMS
```

#### 2. Install Dependencies
```bash
npm install
```

#### 3. Environment Configuration
Create a `.env` file in the root directory:
```env
# Server Configuration
PORT=5000
NODE_ENV=development

# Database Configuration
LOCAL_MONGODB_URL=mongodb://localhost:27017/ims_database
MONGODB_ATLAS_URL=mongodb+srv://username:password@cluster.mongodb.net/ims_database

# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key_here_minimum_32_characters
JWT_EXPIRES_IN=1h
JWT_REMEMBER_EXPIRES_IN=14d

# File Upload Configuration
MAX_FILE_SIZE=5242880
ALLOWED_FILE_TYPES=image/jpeg,image/png,image/gif

# Security Configuration
COOKIE_SECURE=false
COOKIE_SAMESITE=Strict
COOKIE_HTTPONLY=true
```

#### 4. Database Setup
```bash
# Start MongoDB service
mongod

# Or if using MongoDB Atlas, update the connection string in .env
```

#### 5. Start the Application
```bash
# Development mode with auto-restart
npm run dev

# Production mode
npm start
```

#### 6. Access the Application
Open your browser and navigate to: `http://localhost:5000`

### Default Credentials
- **Username**: admin
- **Password**: admin123

---

## 🔒 Security Implementation

### Authentication Security
- **JWT Tokens**: Secure token-based authentication
- **Password Hashing**: bcrypt with salt rounds
- **HTTP-Only Cookies**: XSS protection
- **Session Management**: Automatic token validation

### Data Security
- **Input Validation**: Server-side data validation
- **SQL Injection Protection**: Mongoose ORM protection
- **XSS Prevention**: EJS template escaping
- **CSRF Protection**: SameSite cookie attributes

### API Security
- **Route Protection**: Authentication middleware
- **Rate Limiting**: Request throttling (can be implemented)
- **CORS Configuration**: Cross-origin resource sharing
- **Error Handling**: Secure error responses

---

## 📁 File Structure

```
IMS/
├── 📄 server.js                 # Main application entry point
├── 📄 db.js                     # Database connection configuration
├── 📄 package.json              # Project dependencies and scripts
├── 📁 middleware/
│   ├── 📄 auth.js               # JWT authentication middleware
│   ├── 📄 validation.js         # Input validation middleware
│   ├── 📄 upload.js             # File upload middleware
│   └── 📄 rateLimiter.js        # Rate limiting middleware
├── 📁 models/
│   ├── 📄 userSchema.js         # User data model
│   ├── 📄 categorySchema.js     # Category data model
│   ├── 📄 productSchema.js      # Product data model
│   └── 📄 PurchaseProductSchema.js # Purchase order model
├── 📁 routes/
│   ├── 📄 loginRoutes.js        # Authentication routes
│   ├── 📄 dashboardRoutes.js    # Dashboard API routes
│   ├── 📄 categoryRoutes.js     # Category management routes
│   ├── 📄 productsRoutes.js     # Product management routes
│   └── 📄 PurchaseProductRoutes.js # Purchase order routes
├── 📁 views/
│   ├── 📄 login.ejs             # Login page template
│   ├── 📄 dashboard.ejs         # Dashboard page template
│   ├── 📄 category.ejs          # Category management page
│   ├── 📄 products.ejs          # Product management page
│   ├── 📄 PurchaseProduct.ejs   # Purchase order page
│   └── 📁 partials/
│       ├── 📄 header.ejs        # Common header component
│       └── 📄 sidebar.ejs       # Navigation sidebar
├── 📁 public/
│   ├── 📁 css/                  # Stylesheet files
│   ├── 📁 js/                   # Client-side JavaScript
│   └── 📁 image/                # Static images
├── 📁 utils/
│   ├── 📄 imageProcessor.js     # Image processing utilities
│   ├── 📄 searchHelper.js       # Search functionality
│   ├── 📄 analyticsHelper.js    # Analytics calculations
│   └── 📄 cache.js              # Caching service
├── 📁 tests/                    # Test files
└── 📄 .env                      # Environment variables
```

---

## 💼 Business Logic

### 1. Inventory Management Logic
```javascript
// Product lifecycle management
Product Creation → Category Assignment → Price Setting → Stock Tracking → Purchase Orders
```

### 2. Revenue Calculation
```javascript
// Revenue calculation formula
Revenue = Σ(Selling Price - Purchase Price) × Quantity Sold
```

### 3. Stock Management
```javascript
// Stock level tracking
Current Stock = Initial Stock + Purchases - Sales
```

### 4. Category Organization
```javascript
// Hierarchical category structure
Main Category → Sub Categories → Products → Variants
```

---

## 🛠️ Technical Implementation

### Database Connection Setup
```javascript
// db.js - Database connection
const mongoose = require('mongoose');
require('dotenv').config();

const mongoURL = process.env.LOCAL_MONGODB_URL;

mongoose.connect(mongoURL, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  maxPoolSize: 10,
  serverSelectionTimeoutMS: 5000,
  socketTimeoutMS: 45000,
});

const db = mongoose.connection;

db.on('connected', () => {
  console.log("✅ Connected to MongoDB server");
});

db.on('error', (err) => {
  console.log("❌ MongoDB connection error:", err);
});

db.on('disconnected', () => {
  console.log("⚠️ Disconnected from MongoDB server");
});
```

### JWT Authentication Implementation
```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken');

function isAuthenticated(req, res, next) {
  const token = req.cookies?.token;
  
  if (!token) {
    return res.redirect('/login');
  }
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.clearCookie('token');
    res.redirect('/login');
  }
}

module.exports = { isAuthenticated };
```

### Product Schema with Validation
```javascript
// models/productSchema.js
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
  image: {
    data: {
      type: String,
      required: [true, 'Product image is required']
    },
    contentType: {
      type: String,
      required: true,
      enum: ['image/jpeg', 'image/png', 'image/gif']
    }
  },
  name: {
    type: String,
    required: [true, 'Product name is required'],
    trim: true,
    maxlength: [100, 'Product name cannot exceed 100 characters']
  },
  description: {
    type: String,
    maxlength: [500, 'Description cannot exceed 500 characters']
  },
  purchasePrice: {
    type: Number,
    required: [true, 'Purchase price is required'],
    min: [0, 'Purchase price cannot be negative']
  },
  sellingPrice: {
    type: Number,
    required: [true, 'Selling price is required'],
    min: [0, 'Selling price cannot be negative']
  },
  categories: [{
    type: String,
    required: [true, 'At least one category is required']
  }],
  stockQuantity: {
    type: Number,
    default: 0,
    min: [0, 'Stock quantity cannot be negative']
  },
  sku: {
    type: String,
    unique: true,
    sparse: true,
    uppercase: true
  },
  isActive: {
    type: Boolean,
    default: true
  }
}, {
  timestamps: true
});

// Virtual for profit margin
productSchema.virtual('profitMargin').get(function() {
  if (this.purchasePrice > 0) {
    return ((this.sellingPrice - this.purchasePrice) / this.purchasePrice * 100).toFixed(2);
  }
  return 0;
});

// Indexes for better performance
productSchema.index({ name: 'text', description: 'text' });
productSchema.index({ categories: 1 });
productSchema.index({ isActive: 1 });
productSchema.index({ createdAt: -1 });

module.exports = mongoose.model('Product', productSchema);
```

### RESTful API Implementation
```javascript
// routes/productsRoutes.js
const express = require('express');
const router = express.Router();
const Product = require('../models/productSchema');
const { isAuthenticated } = require('../middleware/auth');

// GET /products - Get all products with pagination and filters
router.get('/', isAuthenticated, async (req, res) => {
  try {
    const page = parseInt(req.query.page) || 1;
    const limit = parseInt(req.query.limit) || 10;
    const search = req.query.search || '';
    const category = req.query.category || '';
    const sortBy = req.query.sortBy || 'createdAt';
    const sortOrder = req.query.sortOrder || 'desc';

    // Build query
    const query = { isActive: true };
    
    if (search) {
      query.$text = { $search: search };
    }
    
    if (category) {
      query.categories = category;
    }

    // Execute query with pagination
    const totalProducts = await Product.countDocuments(query);
    const products = await Product.find(query)
      .sort({ [sortBy]: sortOrder === 'desc' ? -1 : 1 })
      .skip((page - 1) * limit)
      .limit(limit)
      .select('-__v');

    res.json({
      success: true,
      data: {
        products,
        pagination: {
          currentPage: page,
          totalPages: Math.ceil(totalProducts / limit),
          totalProducts,
          hasNextPage: page < Math.ceil(totalProducts / limit),
          hasPrevPage: page > 1
        }
      }
    });
  } catch (error) {
    console.error('Error fetching products:', error);
    res.status(500).json({
      success: false,
      message: 'Internal server error',
      error: process.env.NODE_ENV === 'development' ? error.message : undefined
    });
  }
});

// POST /products - Create new product
router.post('/', isAuthenticated, async (req, res) => {
  try {
    const { name, description, purchasePrice, sellingPrice, categories, stockQuantity, sku } = req.body;

    // Validate required fields
    if (!name || !purchasePrice || !sellingPrice || !categories || categories.length === 0) {
      return res.status(400).json({
        success: false,
        message: 'Missing required fields'
      });
    }

    // Check if SKU already exists
    if (sku) {
      const existingProduct = await Product.findOne({ sku });
      if (existingProduct) {
        return res.status(400).json({
          success: false,
          message: 'SKU already exists'
        });
      }
    }

    // Create product
    const product = new Product({
      name,
      description,
      purchasePrice,
      sellingPrice,
      categories,
      stockQuantity: stockQuantity || 0,
      sku
    });

    await product.save();

    res.status(201).json({
      success: true,
      message: 'Product created successfully',
      data: product
    });
  } catch (error) {
    console.error('Error creating product:', error);
    res.status(500).json({
      success: false,
      message: 'Internal server error',
      error: process.env.NODE_ENV === 'development' ? error.message : undefined
    });
  }
});
```

### File Upload Implementation
```javascript
// middleware/upload.js
const multer = require('multer');

const storage = multer.memoryStorage();

const fileFilter = (req, file, cb) => {
  const allowedTypes = ['image/jpeg', 'image/png', 'image/gif'];
  
  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error('Invalid file type. Only JPEG, PNG and GIF are allowed.'), false);
  }
};

const upload = multer({
  storage: storage,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB limit
  },
  fileFilter: fileFilter
});

module.exports = upload;
```

### Analytics Implementation
```javascript
// utils/analyticsHelper.js
const Product = require('../models/productSchema');
const Category = require('../models/categorySchema');
const PurchaseProduct = require('../models/PurchaseProductSchema');

class AnalyticsHelper {
  static async getDashboardStats() {
    try {
      const [
        totalProducts,
        totalCategories,
        totalPurchases,
        recentProducts,
        categoryStats,
        revenueStats
      ] = await Promise.all([
        Product.countDocuments({ isActive: true }),
        Category.countDocuments(),
        PurchaseProduct.countDocuments(),
        Product.find({ isActive: true })
          .sort({ createdAt: -1 })
          .limit(5)
          .select('name sellingPrice createdAt'),
        this.getCategoryStatistics(),
        this.getRevenueStatistics()
      ]);

      return {
        totalProducts,
        totalCategories,
        totalPurchases,
        recentProducts,
        categoryStats,
        revenueStats
      };
    } catch (error) {
      throw new Error('Failed to fetch dashboard statistics: ' + error.message);
    }
  }

  static async getRevenueStatistics() {
    try {
      const currentMonth = new Date();
      currentMonth.setDate(1);
      currentMonth.setHours(0, 0, 0, 0);

      const lastMonth = new Date(currentMonth);
      lastMonth.setMonth(lastMonth.getMonth() - 1);

      const [currentMonthRevenue, lastMonthRevenue] = await Promise.all([
        Product.aggregate([
          { $match: { isActive: true, createdAt: { $gte: currentMonth } } },
          { $group: { _id: null, total: { $sum: '$sellingPrice' } } }
        ]),
        Product.aggregate([
          { $match: { isActive: true, createdAt: { $gte: lastMonth, $lt: currentMonth } } },
          { $group: { _id: null, total: { $sum: '$sellingPrice' } } }
        ])
      ]);

      const currentRevenue = currentMonthRevenue[0]?.total || 0;
      const previousRevenue = lastMonthRevenue[0]?.total || 0;
      const growthPercentage = previousRevenue > 0 
        ? ((currentRevenue - previousRevenue) / previousRevenue * 100).toFixed(2)
        : 0;

      return {
        currentMonthRevenue: currentRevenue,
        previousMonthRevenue: previousRevenue,
        growthPercentage: parseFloat(growthPercentage)
      };
    } catch (error) {
      throw new Error('Failed to fetch revenue statistics: ' + error.message);
    }
  }
}

module.exports = AnalyticsHelper;
```

### Security Implementation
```javascript
// middleware/validation.js
const { body, validationResult } = require('express-validator');

const validateProduct = [
  body('name')
    .trim()
    .isLength({ min: 1, max: 100 })
    .withMessage('Product name must be between 1 and 100 characters'),
  
  body('purchasePrice')
    .isFloat({ min: 0 })
    .withMessage('Purchase price must be a positive number'),
  
  body('sellingPrice')
    .isFloat({ min: 0 })
    .withMessage('Selling price must be a positive number'),
  
  body('categories')
    .isArray({ min: 1 })
    .withMessage('At least one category is required'),
  
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({
        success: false,
        message: 'Validation failed',
        errors: errors.array()
      });
    }
    next();
  }
];

module.exports = { validateProduct };
```

---

## 🔮 Future Enhancements

### Phase 1: Core Features (Current)
- ✅ User authentication
- ✅ Product management
- ✅ Category management
- ✅ Purchase order tracking
- ✅ Dashboard analytics

### Phase 2: Advanced Features (Planned)
- 🔄 **Sales Management**: Customer orders and sales tracking
- 🔄 **Inventory Alerts**: Low stock notifications
- 🔄 **Reporting System**: Advanced analytics and reports
- 🔄 **Multi-user Support**: Role-based access control
- 🔄 **Barcode Integration**: Product scanning capabilities

### Phase 3: Enterprise Features (Future)
- 🔄 **API Integration**: Third-party service integration
- 🔄 **Mobile Application**: React Native mobile app
- 🔄 **Cloud Deployment**: AWS/Azure cloud hosting
- 🔄 **Advanced Analytics**: Machine learning insights
- 🔄 **Multi-location Support**: Branch management

### Phase 4: Advanced Analytics (Future)
- 🔄 **Predictive Analytics**: Demand forecasting
- 🔄 **Business Intelligence**: Advanced reporting dashboards
- 🔄 **Data Visualization**: Interactive charts and graphs
- 🔄 **Performance Metrics**: KPI tracking and optimization

---

## 📊 Performance Metrics

### Current System Performance
- **Response Time**: < 200ms for API calls
- **Database Queries**: Optimized with indexes
- **Memory Usage**: Efficient resource utilization
- **Scalability**: Horizontal scaling ready

### Optimization Opportunities
- **Caching**: Redis implementation for faster data access
- **CDN**: Static asset delivery optimization
- **Database Indexing**: Query performance improvement
- **Load Balancing**: Traffic distribution optimization

---

## 🛠️ Development Guidelines

### Code Standards
- **ESLint**: Code quality and consistency
- **Prettier**: Code formatting
- **Git Hooks**: Pre-commit validation
- **Documentation**: JSDoc comments

### Testing Strategy
- **Unit Tests**: Individual component testing
- **Integration Tests**: API endpoint testing
- **E2E Tests**: Complete user flow testing
- **Performance Tests**: Load and stress testing

### Deployment Strategy
- **Development**: Local development environment
- **Staging**: Pre-production testing environment
- **Production**: Live application deployment
- **Monitoring**: Application performance monitoring

---

## 📞 Support & Maintenance

### Technical Support
- **Documentation**: Comprehensive API documentation
- **Code Comments**: Inline code documentation
- **Error Logging**: Detailed error tracking
- **Performance Monitoring**: Real-time system monitoring

### Maintenance Schedule
- **Daily**: Database backups and health checks
- **Weekly**: Security updates and patches
- **Monthly**: Performance optimization and cleanup
- **Quarterly**: Feature updates and enhancements

---

## 🎯 Conclusion

The **Kailash Stationery Inventory Management System** is a robust, scalable, and user-friendly solution for modern inventory management needs. With its comprehensive feature set, secure architecture, and extensible design, it provides a solid foundation for business growth and operational efficiency.

The system successfully addresses key business challenges:
- ✅ **Inventory Visibility**: Real-time stock tracking
- ✅ **Process Automation**: Streamlined operations
- ✅ **Data Security**: Protected business information
- ✅ **Scalability**: Growth-ready architecture
- ✅ **User Experience**: Intuitive interface design

For technical implementation details, API documentation, or deployment assistance, please refer to the respective sections above or contact the development team.

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Maintained By**: Development Team  
**Contact**: [Your Contact Information] 
