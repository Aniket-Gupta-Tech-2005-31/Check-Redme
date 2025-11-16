
# eBook App

eBook App is a full-stack web application for creating, editing, and exporting eBooks with AI-powered assistance. It features a modern React frontend, Node.js/Express backend, MongoDB database, and integration with Google Gemini AI for book outline generation.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [Database Model](#database-model)
4. [Use Case Diagram](#use-case-diagram)
5. [Setup & Installation](#setup--installation)
6. [How to Use](#how-to-use)
7. [API & Features](#api--features)
8. [Contributing](#contributing)
9. [License](#license)

---

## Project Overview

eBook Forge enables users to:

- Sign up, log in, and manage their profile
- Create, edit, and organize eBooks with chapters
- Use AI to generate book outlines
- Export eBooks to PDF or DOCX
- Store book covers and content securely

---

## System Architecture
```mermaid
flowchart TD
    FE["Frontend (React + Vite)"] -- "REST API" --> BE["Backend (Node.js/Express)"]
    BE -- "MongoDB Driver" --> MDB[("MongoDB")]
    BE -- "Cloudinary SDK" --> CLD["Cloudinary Storage"]
    BE -- "Google Gemini API" --> AI["AI Outline Generation"]
    FE -- "Auth Token" --> BE
    FE -- "Static Assets" --> PUB["Public/Assets"]
```

---

## Developer Diagrams
---

### 5. Folder Structure
```mermaid
flowchart TD
    Root["eBook-forge/"]
    Root --> Backend["backend/"]
    Root --> Frontend["frontend/"]
    Backend --> BSrc["src/"]
    BSrc --> BControllers["controllers/"]
    BSrc --> BModels["models/"]
    BSrc --> BRoutes["routes/"]
    BSrc --> BConfig["config/"]
    BSrc --> BMiddlewares["middlewares/"]
    BSrc --> BUtils["utils/"]
    Frontend --> FSrc["src/"]
    FSrc --> FComponents["components/"]
    FSrc --> FPages["pages/"]
    FSrc --> FContext["context/"]
    FSrc --> FUtils["utils/"]
    FSrc --> FAssets["assets/"]
    Frontend --> FPublic["public/"]
```

### 6. Class Diagram (Backend Models)
```mermaid
classDiagram
    class User {
        +String name
        +String email
        +String password
        +String avatar
        +Boolean isPro
    }
    class Book {
        +String title
        +String subtitle
        +String author
        +String coverImage
        +String coverImagePublicId
        +String status
        +ObjectId userId
        +List~Chapter~ chapters
    }
    class Chapter {
        +String title
        +String description
        +String content
    }
    User "1" --o "*" Book : owns
    Book "1" --o "*" Chapter : contains
```

### 7. State Diagram (Book Status)
```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Published : publish()
    Published --> Draft : unpublish()
```

### 8. Component Diagram
```mermaid
flowchart TD
    FE["Frontend (React)"]
    BE["Backend (Express)"]
    DB["MongoDB"]
    Cloud["Cloudinary"]
    AI["Gemini AI"]
    FE -->|REST API| BE
    BE --> DB
    BE --> Cloud
    BE --> AI
```

### 9. Deployment Diagram
```mermaid
flowchart TD
    User["User"]
    Browser["Web Browser"]
    Vercel["Vercel (Frontend)"]
    Render["Render (Backend)"]
    Mongo["MongoDB Atlas"]
    Cloudinary["Cloudinary"]
    Gemini["Gemini AI"]
    User --> Browser
    Browser --> Vercel
    Browser --> Render
    Render --> Mongo
    Render --> Cloudinary
    Render --> Gemini
```

### 10. Activity Diagram (Book Creation)
```mermaid
flowchart TD
    Start([Start]) --> Login["User Login"]
    Login --> Dashboard["Open Dashboard"]
    Dashboard --> CreateBook["Click Create Book"]
    CreateBook --> EnterDetails["Enter Book Details"]
    EnterDetails --> OptionalAI["Use AI Outline"]
    OptionalAI --> SaveBook["Save Book"]
    EnterDetails --> SaveBook
    SaveBook --> EditChapters["Edit/Add Chapters"]
    EditChapters --> Export["Export Book"]
    Export --> End([End])
```

### 11. API Contract Diagram (Sample)
```mermaid
flowchart TD
    Client["Client"] -->|POST /api/books| Server["Server"]
    Server -->|201 Created, Book JSON| Client
    Client -->|GET /api/books/:id| Server
    Server -->|200 OK, Book JSON| Client
```

### 12. Data Flow Diagram
```mermaid
flowchart TD
    User["User"] --> FE["Frontend"]
    FE --> BE["Backend"]
    BE --> DB["MongoDB"]
    BE --> Cloud["Cloudinary"]
    BE --> AI["Gemini AI"]
    FE -->|Static| Assets["Assets"]
```

### 1. Backend Route Structure
```mermaid
flowchart TD
    AUTH_SIGNUP["/api/auth/signup"]
    AUTH_LOGIN["/api/auth/login"]
    AUTH_PROFILE["/api/auth/profile"]
    BOOKS_GET["/api/books (GET)"]
    BOOKS_POST["/api/books (POST)"]
    BOOKS_ID_GET["/api/books/:id (GET)"]
    BOOKS_ID_PUT["/api/books/:id (PUT)"]
    BOOKS_ID_DELETE["/api/books/:id (DELETE)"]
    AI_OUTLINE["/api/ai/generate-outline"]
    EXPORT_PDF["/api/export/pdf/:id"]
    EXPORT_DOCX["/api/export/docx/:id"]
    AUTH_SIGNUP --> AUTH_LOGIN
    AUTH_LOGIN --> AUTH_PROFILE
    AUTH_PROFILE --> BOOKS_GET
    BOOKS_GET --> BOOKS_POST
    BOOKS_POST --> BOOKS_ID_GET
    BOOKS_ID_GET --> BOOKS_ID_PUT
    BOOKS_ID_PUT --> BOOKS_ID_DELETE
    BOOKS_ID_GET --> AI_OUTLINE
    BOOKS_ID_GET --> EXPORT_PDF
    BOOKS_ID_GET --> EXPORT_DOCX
```

### 2. Frontend Component Hierarchy
```mermaid
mindmap
    root((App))
        DashboardPage
            DashboardLayout
                BookCard
                CreateBookModal
        EditorPage
            ChapterSidebar
            ChapterEditorTab
            BookDetailsTab
            SimpleMDEditor
        LandingPage
            NavBar
            Hero
            Features
            Footer
        LoginPage
        SignupPage
        ProfilePage
        ViewBookPage
            ViewBook
            ViewChapterSidebar
        ui
            Button
            DropDown
            InputField
            Modal
            SelectField
```

### 3. API Request Flow
```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant DB as MongoDB
    participant Cloud as Cloudinary
    participant AI as GeminiAI
    User->>Frontend: Clicks 'Create Book'
    Frontend->>Backend: POST /api/books
    Backend->>DB: Save book
    Backend-->>Frontend: Book created
    User->>Frontend: Clicks 'Generate Outline'
    Frontend->>Backend: POST /api/ai/generate-outline
    Backend->>AI: Generate outline
    AI-->>Backend: Outline JSON
    Backend-->>Frontend: Outline data
    User->>Frontend: Uploads cover
    Frontend->>Backend: POST /api/books (with image)
    Backend->>Cloud: Upload image
    Cloud-->>Backend: Image URL
    Backend-->>Frontend: Book updated
```

### 4. Deployment/DevOps Overview
```mermaid
flowchart LR
    Dev[Developer]
    Dev -->|Push code| GitHub
    GitHub -->|CI/CD| Vercel[Frontend Hosting]
    GitHub -->|CI/CD| Render[Backend Hosting]
    Render -->|Connects| MongoDBAtlas[(MongoDB Atlas)]
    Render -->|Connects| Cloudinary
    Render -->|Connects| GeminiAI
    Vercel -->|User Access| Browser
```

---

## Database Model

```mermaid
erDiagram
    USER ||--o{ BOOK : owns
    BOOK ||--|{ CHAPTER : contains
    USER {
        string name
        string email
        string password_hashed
        string avatar
        boolean isPro
    }
    BOOK {
        string title
        string subtitle
        string author
        string coverImage
        string coverImagePublicId
        string status_draft_published
        string userId
        string timestamps
    }
    CHAPTER {
        string title
        string description
        string content
    }
```

---

## Use Case Diagram

```mermaid
flowchart TD
    User((User))
    SignUp["Sign Up / Login"]
    CreateBook["Create Book"]
    EditBook["Edit Book"]
    GenerateAI["Generate Outline with AI"]
    ExportBook["Export Book"]
    ViewDashboard["View Dashboard"]
    ManageProfile["Manage Profile"]
    User --> SignUp
    User --> CreateBook
    User --> EditBook
    User --> GenerateAI
    User --> ExportBook
    User --> ViewDashboard
    User --> ManageProfile
```

---

## Setup & Installation

### Prerequisites

| Requirement   | Version/Notes           |
|--------------|------------------------|
| Node.js      | >= 18.x                |
| npm          | >= 9.x                 |
| MongoDB      | Cloud/local instance   |
| Cloudinary   | Account & API keys     |
| Google Gemini| API key                |

### 1. Clone the Repository

```sh
git clone https://github.com/Mr-Aniket-Gupta/eBook-forge.git
cd eBook-forge
```

### 2. Backend Setup

```sh
cd backend
npm install
```

Create a `.env` file in `backend/` with:

```env
PORT=4000
MONGO_URI=your_mongodb_uri
JWT_SECRET=your_jwt_secret
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
GEMINI_API_KEY=your_gemini_api_key
```

Start the backend server:

```sh
npm server.js
# or
nodemon server.js
```

### 3. Frontend Setup

```sh
cd ../frontend
npm install
npm run dev
```

The frontend will run on [http://localhost:5173](http://localhost:5173)

---

## How to Use

### For Non-Coders

1. **Sign Up/Login:** Create an account or log in.
2. **Dashboard:** View your books and create a new one.
3. **Create Book:** Enter book details, add chapters, or use AI to generate an outline.
4. **Edit Book:** Edit chapters, reorder, and update content.
5. **Export:** Download your book as PDF or DOCX.

### For Developers

- Use the REST API for custom integrations (see API table below).
- Extend frontend components in `frontend/src/components`.
- Backend endpoints in `backend/src/routes`.

---

## API & Features

### Main API Endpoints

| Endpoint                | Method | Description                  | Auth Required |
|-------------------------|--------|------------------------------|---------------|
| /api/auth/signup        | POST   | Register new user            | No            |
| /api/auth/login         | POST   | Login user                   | No            |
| /api/auth/profile       | GET    | Get user profile             | Yes           |
| /api/books              | GET    | Get all books (user)         | Yes           |
| /api/books              | POST   | Create new book              | Yes           |
| /api/books/:id          | GET    | Get book by ID               | Yes           |
| /api/books/:id          | PUT    | Update book                  | Yes           |
| /api/books/:id          | DELETE | Delete book                  | Yes           |
| /api/ai/generate-outline| POST   | Generate book outline (AI)   | Yes           |
| /api/export/pdf/:id     | GET    | Export book as PDF           | Yes           |
| /api/export/docx/:id    | GET    | Export book as DOCX          | Yes           |

### Key Features Table

| Feature                | Description                                  |
|------------------------|----------------------------------------------|
| AI Outline Generation  | Generate book structure using Google Gemini   |
| Cloud Storage          | Book covers/images stored on Cloudinary      |
| Export Formats         | Download as PDF or DOCX                      |
| Auth & JWT             | Secure login and protected routes            |
| Responsive UI          | Modern, mobile-friendly React interface      |

---

## Contributing

1. Fork the repo and create your branch.
2. Commit your changes and push.
3. Open a Pull Request.

---

## License

ISC License. See [LICENSE](LICENSE) for details.
