<h1 align="center">
    🏫 SCHOOL MANAGEMENT SYSTEM
</h1>

<h3 align="center">
Streamline school management, class organization, and add students and faculty.<br>
Seamlessly track attendance, assess performance, and provide feedback. <br>
Access records, view marks, and communicate effortlessly.
</h3>

---

## 📋 Table of Contents
1. [About](#about)
2. [Features](#features)
3. [Architecture](#architecture)
4. [System Diagrams](#system-diagrams)
5. [Technologies](#technologies)
6. [Information Tables](#information-tables)
7. [Installation](#installation)

---

## About

The School Management System is a comprehensive web-based application built using the **MERN** (MongoDB, Express.js, React.js, Node.js) stack. It aims to streamline school management, class organization, and facilitate seamless communication between students, teachers, and administrators.

### Key Objectives:
- ✅ Centralized management of school operations
- ✅ Efficient attendance and performance tracking
- ✅ Real-time communication between stakeholders
- ✅ Data-driven insights through visualization
- ✅ Role-based access control and security

---

## ⚡ Features

### 👨‍💼 Admin Panel
- **User Management:** Add/manage students, teachers, and admin accounts
- **Class Management:** Create and organize classes
- **Subject Management:** Define subjects and curricula
- **System Oversight:** Monitor all system activities

### 📚 Teacher Dashboard
- **Attendance Management:** Mark daily attendance for students
- **Performance Assessment:** Record exam marks and provide feedback
- **Class Management:** View assigned classes and students
- **Report Generation:** View attendance and performance reports

### 👨‍🎓 Student Portal
- **Academic Records:** View marks, attendance, and progress
- **Performance Tracking:** Interactive charts showing academic performance
- **Notifications:** Receive important notices and announcements
- **Communication:** File complaints and communicate with teachers

### 🔐 General Features
- **Role-Based Access Control:** Different permissions for Admin, Teacher, Student
- **Secure Authentication:** Password encryption and session management
- **Real-time Data Sync:** Live updates across the system
- **Data Visualization:** Interactive charts and graphs

---

## 🏗️ Architecture

### System Architecture Overview
```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENT LAYER (Frontend)                   │
│  React.js | Redux | Material-UI | Recharts | Styled-Components
└────────────────┬────────────────────────────────────────────┘
                 │ HTTP/HTTPS
                 ▼
┌─────────────────────────────────────────────────────────────┐
│              API LAYER (Backend)                             │
│  Express.js | Node.js | CORS | Body-Parser | Authentication │
└────────────────┬────────────────────────────────────────────┘
                 │
    ┌────────────┴─────────────┐
    ▼                          ▼
┌─────────────┐          ┌──────────────────┐
│  MongoDB    │          │ Business Logic   │
│  Database   │          │ & Controllers    │
└─────────────┘          └──────────────────┘
```

---

## 🎯 System Diagrams

### 1. 📊 Architecture Diagram
```mermaid
graph TD
    A["🌐 Browser / Client<br/>User Device"] --> B["⚛️ React Frontend<br/>Components | Redux Store | Pages"]
    B --> C["🚀 Express.js API<br/>Routes | Controllers | Middleware"]
    C --> D["💾 MongoDB<br/>Database"]
    C --> E["🔐 Authentication<br/>& Authorization"]
    C --> F["⚙️ Business Logic<br/>Controllers"]
    
    style A fill:#e1f5ff
    style B fill:#f3e5f5
    style C fill:#fff3e0
    style D fill:#e8f5e9
    style E fill:#fce4ec
    style F fill:#f1f8e9
```

### 2. 🔄 User Login & Authentication Flow
```mermaid
flowchart TD
    A["START"] --> B["User Opens<br/>Login Page"]
    B --> C["Enter Credentials<br/>Email/Roll No & Password"]
    C --> D["Submit Login Form"]
    D --> E{"Backend<br/>Validation"}
    E -->|Valid| F["Verify Role<br/>Admin/Teacher/Student"]
    E -->|Invalid| G["Show Error<br/>Message"]
    G --> C
    F --> H["Redirect to<br/>Respective Dashboard"]
    H --> I["✅ LOGGED IN"]
    
    style A fill:#c8e6c9
    style I fill:#c8e6c9
    style E fill:#fff9c4
    style G fill:#ffccbc
```

### 3. 🎭 Sequence Diagram (Student Attendance & Marks Flow)
```mermaid
sequenceDiagram
    participant S as 👨‍🎓 Student
    participant T as 👨‍🏫 Teacher
    participant B as 🖥️ Backend
    participant DB as 💾 Database
    participant F as 📱 Frontend
    
    T->>B: Get Class Students
    B->>DB: Query Students
    DB-->>B: Return List
    B-->>T: Display Roster
    
    T->>B: Mark Attendance
    B->>DB: Store Attendance
    DB-->>B: ✓ Saved
    
    T->>B: Add Marks
    B->>DB: Update Exam Results
    DB-->>B: ✓ Updated
    
    B-->>T: Confirmation
    B-->>F: Send Data
    F-->>S: Show Updated Dashboard
    S->>F: View Marks & Attendance
```

### 4. 👥 User Role Interaction Diagram
```mermaid
graph TB
    ADMIN["👨‍💼 ADMIN<br/>Central Authority"]
    
    ADMIN -->|Manages| SM["👨‍🎓 Student<br/>Management"]
    ADMIN -->|Manages| TM["👨‍🏫 Teacher<br/>Management"]
    ADMIN -->|Creates| CM["📚 Class<br/>Management"]
    
    SM --> SA["Add/Remove<br/>Students"]
    SM --> SV["View All<br/>Students"]
    
    TM --> TA["Add/Remove<br/>Teachers"]
    TM --> TO["Assign<br/>Oversight"]
    
    CM --> CC["Create<br/>Classes"]
    CM --> CU["Update<br/>Classes"]
    
    TO --> AT["Mark<br/>Attendance"]
    TO --> AM["Add Marks"]
    
    style ADMIN fill:#ff9800
    style SM fill:#2196f3
    style TM fill:#4caf50
    style CM fill:#9c27b0
```

### 5. 🗂️ Entity Relationship Diagram (ER Diagram)
```mermaid
erDiagram
    ADMIN ||--o{ STUDENT : manages
    ADMIN ||--o{ TEACHER : manages
    ADMIN ||--o{ SCLASS : creates
    ADMIN ||--o{ SUBJECT : creates
    ADMIN ||--o{ NOTICE : publishes
    
    SCLASS ||--o{ STUDENT : contains
    SUBJECT ||--o{ TEACHER : teaches
    SUBJECT ||--o{ STUDENT : studies
    TEACHER ||--o{ SCLASS : assigned_to
    
    STUDENT ||--o{ EXAM_RESULT : achieves
    SUBJECT ||--o{ EXAM_RESULT : evaluates
    STUDENT ||--o{ ATTENDANCE : has
    SUBJECT ||--o{ ATTENDANCE : tracks
    STUDENT ||--o{ COMPLAIN : files
    
    ADMIN {
        string _id PK
        string name
        string email UK
        string password
        string role
    }
    
    STUDENT {
        string _id PK
        string name
        number rollNum
        string password
        string sclassName FK
        string school FK
        string role
        array examResult
        array attendance
    }
    
    TEACHER {
        string _id PK
        string name
        string email UK
        string password
        string teachSubject FK
        string teachSclass FK
        string school FK
        string role
    }
    
    SCLASS {
        string _id PK
        string sclassName
        string school FK
    }
    
    SUBJECT {
        string _id PK
        string subName
        string school FK
    }
    
    COMPLAIN {
        string _id PK
        string student FK
        string title
        string description
        string file
        string status
    }
    
    NOTICE {
        string _id PK
        string school FK
        string title
        string details
        date date
    }
    
    EXAM_RESULT {
        string _id PK
        string subName FK
        number marksObtained
    }
    
    ATTENDANCE {
        string _id PK
        date date
        string status
        string subName FK
    }
```

### 6. 🔄 State Diagram (Student Account Lifecycle)
```mermaid
stateDiagram-v2
    [*] --> REGISTRATION: Account Created
    REGISTRATION --> ACTIVE: Approved by Admin
    REGISTRATION --> SUSPENDED: Rejected
    
    ACTIVE --> IN_GOOD_STANDING: Passing Grades
    ACTIVE --> SUSPENDED: Conduct Issues
    
    IN_GOOD_STANDING --> GRADUATED: Final Year Passed
    IN_GOOD_STANDING --> SUSPENDED: Behavioral Issues
    
    SUSPENDED --> ACTIVE: Appeal Accepted
    SUSPENDED --> GRADUATED: Final Year Passed
    
    GRADUATED --> ARCHIVED: End of Session
    
    ARCHIVED --> [*]: Record Closed
    
    style REGISTRATION fill:#fff9c4
    style ACTIVE fill:#c8e6c9
    style IN_GOOD_STANDING fill:#a5d6a7
    style SUSPENDED fill:#ffccbc
    style GRADUATED fill:#81c784
    style ARCHIVED fill:#bdbdbd
```

### 7. 🧩 Component Diagram
```mermaid
graph TB
    subgraph Frontend["🎨 FRONTEND LAYER"]
        subgraph Pages["📄 Pages"]
            LP["LoginPage"]
            AD["AdminDashboard"]
            TD["TeacherDashboard"]
            SD["StudentDashboard"]
        end
        
        subgraph Components["🧩 Components"]
            TT["TableTemplate"]
            CB["CustomBarChart"]
            CP["CustomPieChart"]
            AM["AccountMenu"]
            SB["SideBar"]
        end
        
        subgraph Redux["📊 Redux Store"]
            US["userSlice"]
            SS["studentSlice"]
            TS["teacherSlice"]
            CS["sclassSlice"]
            CPS["complainSlice"]
            NS["noticeSlice"]
        end
        
        Pages --> Components
        Components --> Redux
    end
    
    subgraph Backend["🖥️ BACKEND LAYER"]
        subgraph Routes["🛣️ Routes"]
            RT1["POST /api/auth/login"]
            RT2["POST /api/admin/add-student"]
            RT3["POST /api/teacher/mark-attendance"]
        end
        
        subgraph Controllers["⚙️ Controllers"]
            AC["admin-controller.js"]
            SC["student_controller.js"]
            TC["teacher-controller.js"]
        end
        
        subgraph Models["🗄️ Models"]
            AM1["adminSchema.js"]
            SM["studentSchema.js"]
            TM["teacherSchema.js"]
        end
        
        Routes --> Controllers
        Controllers --> Models
    end
    
    subgraph Database["💾 DATABASE"]
        MDB["MongoDB<br/>Collections"]
    end
    
    Frontend -->|HTTP/HTTPS| Backend
    Models --> MDB
    
    style Frontend fill:#f3e5f5
    style Backend fill:#fff3e0
    style Database fill:#e8f5e9
```

### 8. ☁️ Deployment Diagram
```mermaid
graph TB
    subgraph Cloud["☁️ CLOUD DEPLOYMENT ARCHITECTURE"]
        subgraph Users["👥 Users"]
            WEB["🌐 Web Browser"]
            MOBILE["📱 Mobile App"]
        end
        
        subgraph Frontend["🎨 Frontend Deployment"]
            NETLIFY["Netlify / Vercel<br/>or GitHub Pages"]
            CDN["CDN / Static Files<br/>React App Bundle"]
        end
        
        subgraph Backend["🖥️ Backend Deployment"]
            HEROKU["Heroku / Railway<br/>or AWS EC2"]
            NODE["Node.js Server<br/>Express.js"]
        end
        
        subgraph Database["💾 Database Deployment"]
            MONGO["MongoDB Atlas<br/>or Self-Hosted"]
            COLL["Collections<br/>Indexes & Backups"]
        end
    end
    
    WEB -->|HTTPS| NETLIFY
    MOBILE -->|HTTPS| NETLIFY
    NETLIFY --> CDN
    CDN -->|REST API| HEROKU
    HEROKU --> NODE
    NODE -->|Database Queries| MONGO
    MONGO --> COLL
    
    style Cloud fill:#e1f5ff
    style Users fill:#bbdefb
    style Frontend fill:#c8e6c9
    style Backend fill:#fff9c4
    style Database fill:#f8bbd0
```

### 9. 🎬 Activity Diagram (Admin Adding a Student)
```mermaid
flowchart TD
    A["START"] --> B["👨‍💼 Admin Opens<br/>Dashboard"]
    B --> C["Navigate to<br/>Student Management"]
    C --> D["Click 'Add Student'<br/>Button"]
    D --> E["📝 Fill Student Form<br/>Name | Roll No | Password | Class"]
    E --> F{"Validate<br/>Form Data"}
    F -->|Invalid| G["❌ Show Errors<br/>& Hints"]
    G --> E
    F -->|Valid| H["Submit Form<br/>to Backend"]
    H --> I{"Backend<br/>Validation"}
    I -->|Invalid| J["⚠️ Return Error<br/>400 Bad Request"]
    J --> G
    I -->|Valid| K["🔐 Hash Password<br/>using Bcrypt"]
    K --> L["💾 Save Student<br/>to MongoDB"]
    L --> M{"Success?"}
    M -->|No| N["❌ Database Error<br/>Return 500"]
    N --> G
    M -->|Yes| O["✅ Return<br/>Success Response"]
    O --> P["📱 Update UI<br/>on Frontend"]
    P --> Q["Show Success<br/>Message"]
    Q --> R["🔄 Refresh<br/>Student List"]
    R --> S["END"]
    
    style A fill:#c8e6c9
    style S fill:#c8e6c9
    style F fill:#fff9c4
    style I fill:#fff9c4
    style M fill:#fff9c4
    style G fill:#ffccbc
    style J fill:#ffccbc
    style N fill:#ffccbc
    style O fill:#a5d6a7
    style K fill:#bbdefb
    style L fill:#bbdefb
```

### 10. 📡 API Contract Diagram
```mermaid
graph LR
    subgraph Auth["🔐 Authentication"]
        L["POST /api/auth/login<br/>Request: email, password<br/>Response: token, user"]
    end
    
    subgraph Admin["👨‍💼 Admin Operations"]
        A1["POST /api/admin/add-student"]
        A2["POST /api/admin/add-teacher"]
        A3["GET /api/admin/students"]
        A4["POST /api/admin/add-subject"]
    end
    
    subgraph Teacher["👨‍🏫 Teacher Operations"]
        T1["POST /api/teacher/mark-attendance"]
        T2["POST /api/teacher/add-marks"]
        T3["GET /api/teacher/class-students"]
    end
    
    subgraph Student["👨‍🎓 Student Operations"]
        S1["GET /api/student/marks"]
        S2["GET /api/student/attendance"]
        S3["POST /api/student/complain"]
    end
    
    style Auth fill:#fce4ec
    style Admin fill:#fff9c4
    style Teacher fill:#e0f2f1
    style Student fill:#f3e5f5
```

**API Endpoints Details:**

| Endpoint | Method | Purpose | Auth |
|----------|--------|---------|------|
| `/api/auth/login` | POST | User login | ❌ |
| `/api/admin/add-student` | POST | Create student | ✅ |
| `/api/admin/add-teacher` | POST | Create teacher | ✅ |
| `/api/admin/students` | GET | Fetch students | ✅ |
| `/api/teacher/mark-attendance` | POST | Record attendance | ✅ |
| `/api/teacher/add-marks` | POST | Enter marks | ✅ |
| `/api/teacher/class-students` | GET | Get class roster | ✅ |
| `/api/student/marks` | GET | Fetch marks | ✅ |
| `/api/student/attendance` | GET | Fetch attendance | ✅ |
| `/api/student/complain` | POST | File complaint | ✅ |

### 11. 📊 Data Flow Diagram
```mermaid
graph LR
    A["👤 User Input<br/>Form Submit"] --> B["✅ Frontend<br/>Validation"]
    B --> C["📊 Redux<br/>State Update"]
    C --> D["🌐 API Request<br/>Axios HTTP"]
    
    D --> E["🖥️ Backend<br/>Receives"]
    E --> F{"Authentication<br/>Check"}
    
    F -->|Invalid| G["❌ 401/403<br/>Unauthorized"]
    G --> H["📱 Error to<br/>Frontend"]
    
    F -->|Valid| I["✓ Route<br/>Handler"]
    I --> J["🔍 Validate<br/>Input"]
    
    J --> K{"Valid?"}
    K -->|No| L["❌ 400<br/>Bad Request"]
    L --> H
    
    K -->|Yes| M["⚙️ Business<br/>Logic"]
    M --> N["💾 Query/<br/>Modify DB"]
    
    N --> O["✅ Return<br/>Response"]
    O --> H
    
    H --> P["📥 Frontend<br/>Receives"]
    P --> Q["🔄 Update<br/>Redux Store"]
    Q --> R["🎨 Re-render<br/>Components"]
    R --> S["👁️ Display<br/>to User"]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e9
    style C fill:#f3e5f5
    style D fill:#fff3e0
    style E fill:#fce4ec
    style F fill:#fff9c4
    style M fill:#c8e6c9
    style N fill:#bbdefb
    style O fill:#a5d6a7
    style S fill:#81c784
```

---

## 💼 Information Tables for Non-Developers

### User Guide Table
| User Type | Primary Role | Key Features | Main Dashboard |
|-----------|-------------|--------------|-----------------|
| **Admin** | System Management | Add/Remove Users, Manage Classes, View Reports | Overview of all users & system health |
| **Teacher** | Instruction & Assessment | Mark Attendance, Enter Marks, View Roster | Class list, today's attendance, marks entry |
| **Student** | Learning & Participation | View Marks, Check Attendance, File Complaints | Personal dashboard with performance charts |

### Feature Access Matrix
| Feature | Admin | Teacher | Student |
|---------|-------|---------|---------|
| Add/Remove Students | ✅ | ❌ | ❌ |
| Mark Attendance | ✅ | ✅ | ❌ |
| Enter Marks | ✅ | ✅ | ❌ |
| View Personal Marks | ✅ | ✅ | ✅ |
| View All Marks | ✅ | ✅* | ❌ |
| File Complaint | ❌ | ✅ | ✅ |
| View Complaints | ✅ | ✅* | ✅* |
| Manage Classes | ✅ | ❌ | ❌ |
| Add Teachers | ✅ | ❌ | ❌ |

> *Limited to own entries/class

### Common User Workflows
| Scenario | Steps |
|----------|-------|
| **Student Checking Marks** | Login → Dashboard → Click "My Marks" → View Subject-wise Performance |
| **Teacher Taking Attendance** | Login → Select Class → Click "Mark Attendance" → Select Date → Mark Status → Save |
| **Admin Adding Student** | Login → Go to Student Mgmt → Click "Add Student" → Fill Details → Submit |
| **Filing a Complaint** | Login → Navigate to Complaints → Create New → Describe Issue → Attach File → Submit |

---

## 🛠️ Information Tables for Developers

### Technology Stack Detail
| Layer | Technologies | Version | Purpose |
|-------|--------------|---------|---------|
| **Frontend** | React.js | 18.2.0 | UI framework |
| | Redux Toolkit | 1.9.5 | State management |
| | Material-UI | 5.12.1 | Component library |
| | Recharts | 2.6.2 | Data visualization |
| | Axios | 1.3.6 | HTTP client |
| | React Router | 6.10.0 | Navigation & routing |
| | Styled Components | 5.3.10 | CSS-in-JS styling |
| **Backend** | Node.js | LTS | Runtime |
| | Express.js | 4.18.2 | Web framework |
| | Mongoose | 7.0.4 | MongoDB ODM |
| | Bcrypt | 5.1.0 | Password hashing |
| | CORS | 2.8.5 | Cross-origin requests |
| | Dotenv | 16.0.3 | Environment variables |
| | Nodemon | 2.0.22 | Dev auto-reload |
| **Database** | MongoDB | Latest | NoSQL database |

### Database Collections Schema Reference
| Collection | Key Fields | Relationships | Purpose |
|-----------|-----------|-----------------|---------|
| **admin** | _id, name, email, password, role | One:Many with students, teachers, classes | School administration |
| **student** | _id, name, rollNum, password, sclassName(FK), examResult[], attendance[] | Many:One with admin, sclass | Student records & performance |
| **teacher** | _id, name, email, password, teachSubject(FK), teachSclass(FK), attendance[] | Many:One with admin, subject, sclass | Faculty management |
| **sclass** | _id, sclassName, school(FK) | Many:One with admin, One:Many with students | Class grouping |
| **subject** | _id, subName, school(FK) | Many:One with admin, Many:Many with teachers | Curriculum management |
| **complain** | _id, student(FK), title, description, file, status, replies[] | Many:One with student | Student grievances |
| **notice** | _id, school(FK), title, details, date | Many:One with admin | School announcements |

### API Endpoints Reference Table
| HTTP Method | Endpoint | Purpose | Auth |
|--------|----------|---------|---|
| **POST** | `/api/auth/login` | User authentication | ❌ |
| **POST** | `/api/admin/add-student` | Create new student | ✅ |
| **GET** | `/api/admin/students` | Fetch all students | ✅ |
| **PUT** | `/api/admin/update-student/:id` | Update student details | ✅ |
| **DELETE** | `/api/admin/delete-student/:id` | Remove student | ✅ |
| **POST** | `/api/admin/add-teacher` | Create new teacher | ✅ |
| **GET** | `/api/admin/teachers` | Fetch all teachers | ✅ |
| **POST** | `/api/admin/add-class` | Create new class | ✅ |
| **GET** | `/api/admin/classes` | Fetch all classes | ✅ |
| **POST** | `/api/admin/add-subject` | Create subject | ✅ |
| **POST** | `/api/teacher/mark-attendance` | Record student attendance | ✅ |
| **POST** | `/api/teacher/add-marks` | Enter exam marks | ✅ |
| **GET** | `/api/teacher/class-students` | Get class roster | ✅ |
| **GET** | `/api/student/marks` | Fetch personal marks | ✅ |
| **GET** | `/api/student/attendance` | Fetch attendance history | ✅ |
| **POST** | `/api/student/complain` | File complaint | ✅ |
| **GET** | `/api/student/complains` | View filed complaints | ✅ |

### Environment Variables Reference
```env
# Backend Configuration
PORT=5000
MONGO_URL=mongodb://127.0.0.1/school
# OR for MongoDB Atlas:
# MONGO_URL=mongodb+srv://username:password@cluster.mongodb.net/school

NODE_ENV=development
JWT_SECRET=your_secret_key_here

# Frontend Configuration  
REACT_APP_BASE_URL=http://localhost:5000
REACT_APP_API_URL=http://localhost:5000
```

### HTTP Status Codes & Solutions
| Code | Name | Description | Solution |
|------|------|-------------|----------|
| 200 | OK | Request successful | No action needed |
| 201 | Created | Resource created successfully | Check response data |
| 400 | Bad Request | Invalid input data | Validate form inputs |
| 401 | Unauthorized | Authentication failed | Login again or refresh token |
| 403 | Forbidden | Insufficient permissions | Check user role |
| 404 | Not Found | Resource doesn't exist | Verify resource ID |
| 500 | Server Error | Backend error | Check server logs |
| 503 | Service Unavailable | Database connection error | Verify MongoDB connection |

### Redux Store State Structure
```javascript
{
  user: {
    user: { _id, name, email, role },
    loading: false,
    error: null
  },
  student: {
    students: [...],
    student: null,
    loading: false,
    error: null
  },
  teacher: {
    teachers: [...],
    teacher: null,
    loading: false,
    error: null
  },
  sclass: {
    sclasses: [...],
    sclass: null,
    loading: false,
    error: null
  },
  complain: {
    complains: [...],
    complain: null,
    loading: false,
    error: null
  },
  notice: {
    notices: [...],
    loading: false,
    error: null
  }
}
```

---

---

# 📥 Installation & Setup

## Prerequisites
- Node.js (v14 or higher)
- MongoDB (local or Atlas)
- Git
- npm or yarn

## Terminal 1: Setting Up Backend 
```sh
cd backend
npm install
npm start
```

### Create `.env` file in backend folder:
```sh
MONGO_URL = mongodb://127.0.0.1/school
```

**Note:** 
- If using MongoDB Compass: Use the above local database link
- If using MongoDB Atlas: Replace with your own database connection string
  
Example MongoDB Atlas URL:
```
MONGO_URL = mongodb+srv://username:password@cluster.mongodb.net/school
```

## Terminal 2: Setting Up Frontend
```sh
cd frontend
npm install
npm start
```

### Navigate to Application
- **Frontend:** `http://localhost:3000`
- **Backend API:** `http://localhost:5000`

---

# 🐛 Error Solutions

## Network/Loading Error During Signup

If you encounter network or infinite loading errors during signup:

### Solution 1: Update Frontend .env File
1. Navigate to `frontend/.env`
2. Uncomment the first line
3. Terminate frontend terminal
4. Run:
```sh
cd frontend
npm start
```

### Solution 2: Manual API URL Configuration
1. Open `frontend/src/redux/userRelated/userHandle.js`
2. Add after imports:
```javascript
const REACT_APP_BASE_URL = "http://localhost:5000";
```
3. Replace all `process.env.REACT_APP_BASE_URL` with `REACT_APP_BASE_URL`
4. **IMPORTANT:** Repeat for ALL files with "Handle" in their names:
   - `studentRelated/studentHandle.js`
   - `teacherRelated/teacherHandle.js`
   - `complainRelated/complainHandle.js`
   - `noticeRelated/noticeHandle.js`
   - `sclassRelated/sclassHandle.js`

---

## Delete Feature Not Working

The delete function is disabled by default to prevent guest data loss.

### Enable Delete Feature:

1. **Open** `frontend/src/redux/userRelated/userHandle.js`
2. **Find** the `deleteUser` function (around line 87-90) that shows:
```javascript
export const deleteUser = (id, address) => async (dispatch) => {
    dispatch(getRequest());
    dispatch(getFailed("Sorry the delete function has been disabled for now."));
}
```
3. **Comment it out** and **uncomment** the original function above it
4. **Repeat for all "Show" component files** in `frontend/src/pages/admin/[Name]Related/`:
   - `ShowClasses.js`
   - `ShowStudents.js`
   - `ShowTeachers.js`
   - etc.

5. **In each "Show" file**, find and update `deleteHandler`:
```javascript
// Comment out:
setMessage("Sorry, the delete function has been disabled for now.");
setShowPopup(true);

// Uncomment:
dispatch(deleteUser(deleteID, address))
    .then(() => {
      dispatch(getAllSclasses(adminID, "Sclass"));
    })
```

---

## Testing Tips

- **First Time:** Create a new account via signup rather than using login
- **Guest Mode:** For quick testing, use existing credentials from the demo database
- **Valid Credentials Format:**
  - Admin: `email@example.com` + password
  - Teacher: `email@example.com` + password
  - Student: Roll Number + password

---

# 🚀 Quick Start Guide

### For Non-Developers:
1. Admin creates an account
2. Admin adds classes, subjects, and teachers
3. Admin adds students to classes
4. Teachers mark attendance and enter marks
5. Students check their performance

### For Developers:
1. Clone the repository
2. Install dependencies in both frontend and backend
3. Set up MongoDB connection
4. Run backend server (`npm start` in backend)
5. Run frontend app (`npm start` in frontend)
6. Check `http://localhost:3000`

---

# 📞 Support & Contribution

If you encounter any issues:
1. Check the Error Solutions section above
2. Verify all environment variables are set correctly
3. Ensure MongoDB is running
4. Check backend logs for errors
5. Open an issue on GitHub with details

---

# ⭐ Don't Forget!

If this project helped you, please consider giving it a **star** ⭐ on GitHub!

Thank you for using the School Management System!

---

# 📄 Create `.env` in the backend folder.
Inside it write this :

```sh
MONGO_URL = mongodb://127.0.0.1/school
```
If you are using MongoDB Compass you can use this database link but if you are using MongoDB Atlas then instead of this link write your own database link.

Terminal 2: Setting Up Frontend
```sh
cd frontend
npm install
npm start
```
Now, navigate to `localhost:3000` in your browser. 
The Backend API will be running at `localhost:5000`.
<br>
# Error Solution

You might encounter an error while signing up, either a network error or a loading error that goes on indefinitely.

To resolve it:

1. Navigate to the `frontend > .env` file.

2. Uncomment the first line. After that, terminate the frontend terminal. Open a new terminal and execute the following commands:
```sh
cd frontend
npm start
```

After completing these steps, try signing up again. If the issue persists, follow these additional steps to resolve it:

1. Navigate to the `frontend > src > redux > userRelated > userHandle.js` file.

2. Add the following line after the import statements:

```javascript
const REACT_APP_BASE_URL = "http://localhost:5000";
```

3. Replace all instances of `process.env.REACT_APP_BASE_URL` with `REACT_APP_BASE_URL`.

**IMPORTANT:** Repeat the same process for all other files with "Handle" in their names.

For example, in the `redux` folder, there are other folders like `userRelated`. In the `teacherRelated` folder, you'll find a file named `teacherHandle`. Similarly, other folders contain files with "Handle" in their names. Make sure to update these files as well.

The issue arises because the `.env` file in the frontend may not work for all users, while it works for me.

Additionally:

- When testing the project, start by signing up rather than logging in as a guest or using regular login if you haven't created an account yet.
  
  To use guest mode, navigate to `LoginPage.js` and provide an email and password from a project already created in the system. This simplifies the login process, and after creating your account, you can use your credentials.

These steps should resolve the network error in the frontend. If the issue persists, feel free to contact me for further assistance.

# Delete Feature Not Working Solution

When attempting to delete items, you may encounter a popup message stating, "Sorry, the delete function has been disabled for now." This message appears because I have disabled the delete function on my live site to prevent guests from deleting items. If you wish to enable the delete feature, please follow these steps:

1. Navigate to the `frontend > src > redux > userRelated > userHandle.js` file.

2. If you haven't made any changes, you should find the `deleteUser` function at line 71. It may be commented out. It might look like this:

```javascript
// export const deleteUser = (id, address) => async (dispatch) => {
//     dispatch(getRequest());

//     try {
//         const result = await axios.delete(`${process.env.REACT_APP_BASE_URL}/${address}/${id}`);
//         if (result.data.message) {
//             dispatch(getFailed(result.data.message));
//         } else {
//             dispatch(getDeleteSuccess());
//         }
//     } catch (error) {
//         dispatch(getError(error));
//     }
// }
```

3. Uncomment above `deleteUser` function and comment out this `deleteUser` function that is currently running from line 87 to line 90 :

```javascript
export const deleteUser = (id, address) => async (dispatch) => {
    dispatch(getRequest());
    dispatch(getFailed("Sorry the delete function has been disabled for now."));
}
```

4. If you have previously modified the code, you may find the `deleteUser` functions at different lines. In this case, uncomment the original code and comment out the current one.

5. Next, navigate to the `frontend > src > pages > admin` folder. Here, you will find different folders suffixed with "Related". Open each folder and locate files prefixed with "Show".

6. Open each file with "Show" as a prefix and search for a function named `deleteHandler`. For example:
   
```javascript
const deleteHandler = (deleteID, address) => {
  console.log(deleteID);
  console.log(address);
  setMessage("Sorry, the delete function has been disabled for now.");
  setShowPopup(true);
  // dispatch(deleteUser(deleteID, address))
  //   .then(() => {
  //     dispatch(getAllSclasses(adminID, "Sclass"));
  //   })
}
```

7. This is an example snippet from `ShowClasses`. In other files with "Show" as a prefix, it may differ.

8. Uncomment the commented-out code inside the `deleteHandler` function and comment out the existing code. It should resemble this:

```javascript
const deleteHandler = (deleteID, address) => {
  // console.log(deleteID);
  // console.log(address);
  // setMessage("Sorry, the delete function has been disabled for now.");
  // setShowPopup(true);
  dispatch(deleteUser(deleteID, address))
    .then(() => {
      dispatch(getAllSclasses(adminID, "Sclass"));
    })
}
```

9. Repeat these steps for every other file. In some cases, the `deleteHandler` function may also be found in files prefixed with "View". Check those files and repeat the same process.

If the issue persists, feel free to contact me for further assistance.

Don't forget to leave a star for this project if you found the solution helpful. Thank you!
