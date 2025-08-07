#  Implementation Guide

## Understanding the Tech Stack First

### What we're building and why each technology:

**Backend (Server-side):**

- **Node.js**: JavaScript runtime that lets us run JavaScript on the server

- **Express.js**: Framework that makes creating web APIs easier

- **PostgreSQL**: Database to store all our data (users, groups, etc.)

- **Prisma**: Tool that makes working with databases easier (instead of writing raw SQL)

- **JWT**: Tokens for user authentication (like digital ID cards)

**Frontend (Client-side):**

- **React**: Library for building user interfaces

- **Vite**: Fast development server and build tool

- **Tailwind CSS**: Utility-first CSS framework for styling

---

# PART 1: BACKEND DEVELOPMENT

## Prerequisites Setup (Do this first!)

### Step 1: Install Required Software

#### Install Node.js

1\. Go to [nodejs.org](https://nodejs.org)

2\. Download the LTS version (Long Term Support)

3\. Install it with default settings

4\. Open terminal/command prompt and verify:

   ```bash

   node --version    # Should show v18.x.x or higher

   npm --version     # Should show v9.x.x or higher

   ```

#### Install PostgreSQL

1\. Go to [postgresql.org](https://postgresql.org/download/)

2\. Download for your operating system

3\. During installation, remember the password you set for 'postgres' user

4\. Install with default settings (port 5432)

5\. Verify installation by opening "pgAdmin" (comes with PostgreSQL)

#### Install Git (if not already installed)

1\. Go to [git-scm.com](https://git-scm.com)

2\. Download and install

3\. Verify: `git --version`

### Step 2: Setup Development Environment

#### Install VS Code (Recommended Editor)

1\. Download from [code.visualstudio.com](https://code.visualstudio.com)

2\. Install these extensions:

   - Prisma (for database schema)

   - ESLint (code quality)

   - Prettier (code formatting)

   - Thunder Client (API testing - alternative to Postman)

---

## Backend Implementation

### Phase 1: Project Foundation

#### Step 3: Create the Backend Project

**What we're doing:** Creating the basic structure for our backend application.

```bash

# Create a folder for your entire project

mkdir chaicode-groups-project

cd chaicode-groups-project

# Create backend folder

mkdir backend

cd backend

# Initialize a new Node.js project

npm init -y

```

**What just happened:**

- `mkdir`: Creates a new folder

- `cd`: Changes into that folder

- `npm init -y`: Creates a `package.json` file (this tracks our project dependencies)

#### Step 4: Create Project Folder Structure

```bash

# Create all the folders we'll need

mkdir src

mkdir src/controllers    # Functions that handle API requests

mkdir src/middleware     # Functions that run before/after requests

mkdir src/routes        # URL definitions and routing

mkdir src/utils         # Helper functions

mkdir src/config        # Configuration files

mkdir src/validators    # Input validation rules

mkdir tests            # Test files

mkdir logs             # Log files will go here

# Create main files

touch src/app.js        # Main application setup

touch src/server.js     # Server startup file

touch .env             # Environment variables (secrets)

touch .env.example     # Example environment file

touch .gitignore       # Files Git should ignore

touch README.md        # Project documentation

```

**Understanding the structure:**

- `src/`: Contains all our source code

- `controllers/`: Handle what happens when someone visits an API endpoint

- `middleware/`: Code that runs between request and response (like security checks)

- `routes/`: Define which URLs our API responds to

#### Step 5: Install Required Packages

**What we're doing:** Adding the tools/libraries our project needs.

```bash

# Main production dependencies

npm install express          # Web framework

npm install cors            # Allow frontend to communicate with backend

npm install helmet          # Security headers

npm install morgan          # HTTP request logger

npm install compression     # Compress responses

npm install jsonwebtoken    # Create and verify JWT tokens

npm install bcryptjs        # Hash passwords securely

npm install joi             # Validate user input

npm install nodemailer      # Send emails

npm install dotenv          # Load environment variables

npm install prisma          # Database toolkit

npm install @prisma/client  # Prisma client for database queries

npm install winston         # Advanced logging

npm install express-async-errors # Better error handling

npm install express-rate-limit   # Prevent abuse

# Development dependencies (only needed during development)

npm install -D nodemon      # Auto-restart server on changes

npm install -D eslint       # Code quality checker

npm install -D prettier     # Code formatter

npm install -D jest         # Testing framework

npm install -D supertest    # API testing

```

**What each package does:**

- **express**: Creates our web server and handles HTTP requests

- **cors**: Lets our frontend (running on port 3000) talk to backend (port 5000)

- **helmet**: Adds security headers to protect against common attacks

- **bcryptjs**: Safely hashes passwords so we never store plain text passwords

- **jsonwebtoken**: Creates tokens to identify logged-in users

- **prisma**: Makes database operations much easier than raw SQL

#### Step 6: Configure Development Tools

**Setting up code quality tools:**

```bash

# Initialize ESLint (code quality checker)

npx eslint --init

```

When prompted, choose:

- "To check syntax, find problems, and enforce code style"

- "JavaScript modules (import/export)" or "CommonJS (require/exports)" - choose CommonJS

- "None of these" (we're not using React/Vue in backend)

- "Node"

- "Use a popular style guide"

- "Standard"

- "JavaScript"

- "Yes" to install dependencies

**Create Prettier configuration** (code formatter):

Create `.prettierrc` file:

```json

{

  "semi": false,

  "singleQuote": true,

  "tabWidth": 2,

  "trailingComma": "none",

  "printWidth": 80

}

```

**Update package.json scripts:**

Open `package.json` and replace the "scripts" section:

```json

"scripts": {

  "start": "node src/server.js",

  "dev": "nodemon src/server.js",

  "test": "jest",

  "lint": "eslint src/",

  "lint:fix": "eslint src/ --fix",

  "format": "prettier --write src/"

}

```

#### Step 7: Environment Configuration

**What we're doing:** Setting up configuration that changes between development and production.

Create `.env` file (this contains secrets - never commit to Git):

```env

# Server Configuration

NODE_ENV=development

PORT=5000

HOST=localhost

# Database Configuration (we'll set this up next)

DATABASE_URL="postgresql://postgres:your_password@localhost:5432/chaicode_groups?schema=public"

# JWT Configuration (replace with random long strings)

JWT_SECRET=your-super-secret-jwt-key-minimum-32-characters-long-make-it-random

JWT_REFRESH_SECRET=your-refresh-token-secret-also-32-characters-long-different-from-above

JWT_ACCESS_EXPIRE=15m

JWT_REFRESH_EXPIRE=7d

# Email Configuration (we'll configure this later)

EMAIL_HOST=smtp.gmail.com

EMAIL_PORT=587

EMAIL_SECURE=false

EMAIL_USER=your-email@gmail.com

EMAIL_PASSWORD=your-app-password

EMAIL_FROM=noreply@chaicode.com

# Frontend URL (for CORS)

FRONTEND_URL=http://localhost:3000

# Security

BCRYPT_ROUNDS=12

RATE_LIMIT_WINDOW_MS=900000

RATE_LIMIT_MAX=100

# Logging

LOG_LEVEL=info

```

**IMPORTANT:** Replace `your_password` with the PostgreSQL password you set during installation.

**Create .env.example** (safe to commit):

Copy `.env` to `.env.example` and replace all actual values with placeholders:

```env

DATABASE_URL="postgresql://username:password@localhost:5432/chaicode_groups?schema=public"

JWT_SECRET=your-jwt-secret-here

# ... etc

```

**Create .gitignore** (files Git should ignore):

```gitignore

# Dependencies

node_modules/

npm-debug.log*

# Environment variables

.env

.env.local

# Logs

logs/

*.log

# Runtime data

pids/

*.pid

*.seed

# IDE

.vscode/

.idea/

# OS

.DS_Store

Thumbs.db

# Prisma

prisma/migrations/

```

### Phase 2: Database Setup

#### Step 8: Initialize Database

**What we're doing:** Setting up PostgreSQL database and Prisma.

1\. **Create Database:**

   - Open pgAdmin (installed with PostgreSQL)

   - Right-click "Databases" → "Create" → "Database"

   - Name: `chaicode_groups`

   - Click "Save"

2\. **Initialize Prisma:**

```bash

# Initialize Prisma in your project

npx prisma init

```

This creates:

- `prisma/schema.prisma` (database structure definition)

- Updates `.env` with DATABASE_URL

#### Step 9: Define Database Schema

**What we're doing:** Defining the structure of our data (tables, relationships).

Replace contents of `prisma/schema.prisma`:

```prisma

// This is your Prisma schema file

generator client {

  provider = "prisma-client-js"

}

datasource db {

  provider = "postgresql"

  url      = env("DATABASE_URL")

}

// Users table - stores all user information

model User {

  id          String   @id @default(cuid())

  email       String   @unique

  name        String

  password    String

  githubUrl   String?

  twitterUrl  String?

  hashnodeUrl String?

  bio         String?

  isVerified  Boolean  @default(false)

  createdAt   DateTime @default(now())

  updatedAt   DateTime @updatedAt

  // Relations (connections to other tables)

  ownedGroups         Group[]             // Groups this user leads

  groupMemberships    GroupMember[]       // Groups this user is member of

  membershipRequests  MembershipRequest[] // Join requests by this user

  noticeboards        NoticeBoard[]       // Notices posted by this user

  activityLogs        ActivityLog[]       // Activities by this user

  @@map("users")

}

// Whitelist table - approved emails that can register

model Whitelist {

  id           String   @id @default(cuid())

  email        String   @unique

  courseId     String?

  courseName   String?

  isRegistered Boolean  @default(false)

  createdAt    DateTime @default(now())

  @@map("whitelist")

}

// Groups table - study/project groups

model Group {

  id          String   @id @default(cuid())

  name        String   @unique

  description String?

  purpose     Purpose  // Enum defined below

  maxMembers  Int      @default(4)

  leaderId    String

  isActive    Boolean  @default(true)

  createdAt   DateTime @default(now())

  updatedAt   DateTime @updatedAt

  // Relations

  leader             User                @relation(fields: [leaderId], references: [id], onDelete: Cascade)

  members            GroupMember[]

  membershipRequests MembershipRequest[]

  noticeboards       NoticeBoard[]

  activityLogs       ActivityLog[]

  @@map("groups")

}

// Group membership tracking

model GroupMember {

  id            String       @id @default(cuid())

  groupId       String

  userId        String

  joinDate      DateTime     @default(now())

  status        MemberStatus @default(ACTIVE)

  removalReason String?

  leaveReason   String?

  createdAt     DateTime     @default(now())

  // Relations

  group Group @relation(fields: [groupId], references: [id], onDelete: Cascade)

  user  User  @relation(fields: [userId], references: [id], onDelete: Cascade)

  // Constraint: one user can only be in one group

  @@unique([groupId, userId])

  @@map("group_members")

}

// Join requests to groups

model MembershipRequest {

  id              String        @id @default(cuid())

  groupId         String

  userId          String

  message         String

  status          RequestStatus @default(PENDING)

  responseMessage String?

  createdAt       DateTime      @default(now())

  updatedAt       DateTime      @updatedAt

  expiresAt       DateTime      @default(dbgenerated("NOW() + INTERVAL '7 days'"))

  // Relations

  group Group @relation(fields: [groupId], references: [id], onDelete: Cascade)

  user  User  @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([groupId, userId])

  @@map("membership_requests")

}

// Notice board for group communications

model NoticeBoard {

  id        String     @id @default(cuid())

  groupId   String

  authorId  String

  title     String

  content   String

  type      NoticeType

  isPinned  Boolean    @default(false)

  createdAt DateTime   @default(now())

  updatedAt DateTime   @updatedAt

  // Relations

  group  Group @relation(fields: [groupId], references: [id], onDelete: Cascade)

  author User  @relation(fields: [authorId], references: [id], onDelete: Cascade)

  @@map("notice_board")

}

// Activity logging for accountability

model ActivityLog {

  id         String     @id @default(cuid())

  userId     String

  groupId    String?

  actionType ActionType

  message    String

  metadata   Json?

  createdAt  DateTime   @default(now())

  // Relations

  user  User   @relation(fields: [userId], references: [id], onDelete: Cascade)

  group Group? @relation(fields: [groupId], references: [id], onDelete: Cascade)

  @@map("activity_logs")

}

// Enums (predefined values)

enum Purpose {

  LEARNING

  PROJECT

  DISCUSSION

  NETWORKING

  OTHER

}

enum MemberStatus {

  ACTIVE

  REMOVED

  LEFT

}

enum RequestStatus {

  PENDING

  APPROVED

  REJECTED

  EXPIRED

}

enum NoticeType {

  MEETING

  ANNOUNCEMENT

  RESOURCE

  DISCUSSION

}

enum ActionType {

  USER_REGISTERED

  USER_VERIFIED

  GROUP_CREATED

  GROUP_DELETED

  JOIN_REQUEST_SENT

  JOIN_REQUEST_APPROVED

  JOIN_REQUEST_REJECTED

  MEMBER_REMOVED

  MEMBER_LEFT

  NOTICE_POSTED

  NOTICE_UPDATED

  NOTICE_DELETED

}

```

**Understanding the schema:**

- `@id`: Primary key (unique identifier)

- `@unique`: This field must be unique across all records

- `@default()`: Default value when creating new records

- `String?`: Question mark means optional field

- Relations define how tables connect to each other

#### Step 10: Create and Run Database Migration

```bash

# Generate Prisma client (JavaScript functions to interact with database)

npx prisma generate

# Create database tables based on our schema

npx prisma migrate dev --name init

# Optional: View your database in browser

npx prisma studio

```

**What happened:**

- `prisma generate`: Created JavaScript functions based on our schema

- `prisma migrate dev`: Created actual database tables

- `prisma studio`: Opens a web interface to view/edit data

### Phase 3: Basic Server Setup

#### Step 11: Create Database Connection

Create `src/config/database.js`:

```javascript

const { PrismaClient } = require('@prisma/client')

// Create database connection

const prisma = new PrismaClient({

  log: process.env.NODE_ENV === 'development'

    ? ['query', 'info', 'warn', 'error']  // Show all logs in development

    : ['error'],                          // Only show errors in production

  errorFormat: 'colorless'

})

// Graceful shutdown - close database connection when app closes

process.on('SIGINT', async () => {

  await prisma.$disconnect()

  process.exit()

})

process.on('SIGTERM', async () => {

  await prisma.$disconnect()

  process.exit()

})

module.exports = prisma

```

**What this does:**

- Creates a connection to our PostgreSQL database

- Configures logging (what information to show)

- Handles cleanup when the application shuts down

#### Step 12: Create Basic Express Server

Create `src/app.js`:

```javascript

const express = require('express')

const cors = require('cors')

const helmet = require('helmet')

const morgan = require('morgan')

const compression = require('compression')

const rateLimit = require('express-rate-limit')

require('express-async-errors') // Better error handling for async functions

require('dotenv').config()

const app = express()

// Security middleware

app.use(helmet()) // Adds security headers

// CORS - allows frontend to communicate with backend

app.use(cors({

  origin: process.env.FRONTEND_URL || 'http://localhost:3000',

  credentials: true

}))

// Request logging

app.use(morgan('combined'))

// Compress responses for better performance

app.use(compression())

// Parse JSON requests

app.use(express.json({ limit: '10mb' }))

app.use(express.urlencoded({ extended: true }))

// Rate limiting - prevent abuse

const limiter = rateLimit({

  windowMs: parseInt(process.env.RATE_LIMIT_WINDOW_MS) || 15 * 60 * 1000, // 15 minutes

  max: parseInt(process.env.RATE_LIMIT_MAX) || 100, // limit each IP to 100 requests per windowMs

  message: {

    success: false,

    error: {

      code: 'RATE_LIMIT_EXCEEDED',

      message: 'Too many requests, please try again later.'

    }

  }

})

app.use('/api/', limiter)

// Health check endpoint

app.get('/health', (req, res) => {

  res.json({

    success: true,

    message: 'Server is healthy',

    timestamp: new Date().toISOString()

  })

})

// API routes will be added here

app.use('/api', (req, res) => {

  res.status(404).json({

    success: false,

    error: {

      code: 'ENDPOINT_NOT_FOUND',

      message: 'API endpoint not found'

    }

  })

})

// Global error handler

app.use((error, req, res, next) => {

  console.error('Error:', error)

  // Don't send error details in production

  const message = process.env.NODE_ENV === 'production'

    ? 'Something went wrong'

    : error.message

  res.status(500).json({

    success: false,

    error: {

      code: 'INTERNAL_ERROR',

      message: message

    }

  })

})

module.exports = app

```

Create `src/server.js`:

```javascript

const app = require('./app')

const PORT = process.env.PORT || 5000

const HOST = process.env.HOST || 'localhost'

app.listen(PORT, HOST, () => {

  console.log(`

🚀 Server running on http://${HOST}:${PORT}

📊 Health check: http://${HOST}:${PORT}/health

🌍 Environment: ${process.env.NODE_ENV || 'development'}

  `)

})

```

#### Step 13: Test Basic Server

```bash

# Start the development server

npm run dev

```

**You should see:**

```

🚀 Server running on http://localhost:5000

📊 Health check: http://localhost:5000/health

🌍 Environment: development

```

**Test it:**

- Open browser and go to `http://localhost:5000/health`

- You should see: `{"success":true,"message":"Server is healthy","timestamp":"..."}`

### Phase 4: Authentication System

#### Step 14: Create Validation Schemas

**What we're doing:** Defining rules for validating user input.

Create `src/validators/authValidator.js`:

```javascript

const Joi = require('joi')

// Rules for user registration

const registerSchema = Joi.object({

  email: Joi.string()

    .email()

    .lowercase()

    .max(255)

    .required()

    .messages({

      'string.email': 'Please provide a valid email address',

      'any.required': 'Email is required'

    }),

  name: Joi.string()

    .min(2)

    .max(100)

    .trim()

    .required()

    .messages({

      'string.min': 'Name must be at least 2 characters long',

      'string.max': 'Name cannot exceed 100 characters',

      'any.required': 'Name is required'

    }),

  password: Joi.string()

    .min(8)

    .max(128)

    .pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)

    .required()

    .messages({

      'string.min': 'Password must be at least 8 characters long',

      'string.pattern.base': 'Password must contain at least one lowercase letter, one uppercase letter, and one number',

      'any.required': 'Password is required'

    }),

  githubUrl: Joi.string()

    .uri()

    .pattern(/^https:\/\/(www\.)?github\.com\/[a-zA-Z0-9-_]+\/?$/)

    .allow('', null)

    .messages({

      'string.pattern.base': 'Please provide a valid GitHub profile URL'

    }),

  twitterUrl: Joi.string()

    .uri()

    .pattern(/^https:\/\/(www\.)?twitter\.com\/[a-zA-Z0-9_]+\/?$/)

    .allow('', null)

    .messages({

      'string.pattern.base': 'Please provide a valid Twitter profile URL'

    }),

  hashnodeUrl: Joi.string()

    .uri()

    .pattern(/^https:\/\/[a-zA-Z0-9-_]+\.hashnode\.(dev|com)\/?$/)

    .allow('', null)

    .messages({

      'string.pattern.base': 'Please provide a valid Hashnode blog URL'

    }),

  bio: Joi.string()

    .max(500)

    .allow('', null)

    .messages({

      'string.max': 'Bio cannot exceed 500 characters'

    })

})

// Rules for user login

const loginSchema = Joi.object({

  email: Joi.string()

    .email()

    .lowercase()

    .required()

    .messages({

      'string.email': 'Please provide a valid email address',

      'any.required': 'Email is required'

    }),

  password: Joi.string()

    .required()

    .messages({

      'any.required': 'Password is required'

    })

})

module.exports = {

  registerSchema,

  loginSchema

}

```

**What this does:**

- Defines rules for what valid registration/login data looks like

- Provides custom error messages for validation failures

- Checks email format, password strength, URL patterns, etc.

#### Step 15: Create Validation Middleware

Create `src/middleware/validation.js`:

```javascript

// Middleware to validate request data against schemas

const validate = (schema) => {

  return (req, res, next) => {

    const { error } = schema.validate(req.body, {

      abortEarly: false, // Show all validation errors, not just first one

      stripUnknown: true // Remove fields not in schema

    })

    if (error) {

      const errors = error.details.map(detail => ({

        field: detail.path[0],

        message: detail.message

      }))

      return res.status(400).json({

        success: false,

        error: {

          code: 'VALIDATION_ERROR',

          message: 'Please fix the following validation errors',

          details: errors

        }

      })

    }

    next() // Validation passed, continue to next middleware

  }

}

module.exports = { validate }

```

#### Step 16: Create Authentication Controller

**What we're doing:** Creating functions to handle user registration and login.

Create `src/controllers/authController.js`:

```javascript

const bcrypt = require('bcryptjs')

const jwt = require('jsonwebtoken')

const prisma = require('../config/database')

// Generate JWT tokens

const generateTokens = (userId) => {

  const accessToken = jwt.sign(

    { userId },

    process.env.JWT_SECRET,

    { expiresIn: process.env.JWT_ACCESS_EXPIRE || '15m' }

  )

  const refreshToken = jwt.sign(

    { userId },

    process.env.JWT_REFRESH_SECRET,

    { expiresIn: process.env.JWT_REFRESH_EXPIRE || '7d' }

  )

  return { accessToken, refreshToken }

}

// Register new user

const register = async (req, res) => {

  const { email, name, password, githubUrl, twitterUrl, hashnodeUrl, bio } = req.body

  try {

    // Check if email is in whitelist

    const whitelistEntry = await prisma.whitelist.findUnique({

      where: { email }

    })

    if (!whitelistEntry) {

      return res.status(403).json({

        success: false,

        error: {

          code: 'AUTH001',

          message: 'Email not found in course registration list'

        }

      })

    }

    // Check if user already registered

    const existingUser = await prisma.user.findUnique({

      where: { email }

    })

    if (existingUser) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'AUTH002',

          message: 'Email already registered'

        }

      })

    }

    // Hash password

    const hashedPassword = await bcrypt.hash(password, parseInt(process.env.BCRYPT_ROUNDS) || 12)

    // Create user

    const user = await prisma.user.create({

      data: {

        email,

        name,

        password: hashedPassword,

        githubUrl: githubUrl || null,

        twitterUrl: twitterUrl || null,

        hashnodeUrl: hashnodeUrl || null,

        bio: bio || null

      },

      select: {

        id: true,

        email: true,

        name: true,

        githubUrl: true,

        twitterUrl: true,

        hashnodeUrl: true,

        bio: true,

        isVerified: true,

        createdAt: true

      }

    })

    // Update whitelist to mark as registered

    await prisma.whitelist.update({

      where: { email },

      data: { isRegistered: true }

    })

    // Log activity

    await prisma.activityLog.create({

      data: {

        userId: user.id,

        actionType: 'USER_REGISTERED',

        message: `User ${user.name} registered successfully`

      }

    })

    // Generate tokens

    const { accessToken, refreshToken } = generateTokens(user.id)

    res.status(201).json({

      success: true,

      message: 'Registration successful',

      data: {

        user,

        tokens: {

          accessToken,

          refreshToken

        }

      }

    })

  } catch (error) {

    console.error('Registration error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Registration failed'

      }

    })

  }

}

// Login user

const login = async (req, res) => {

  const { email, password } = req.body

  try {

    // Find user

    const user = await prisma.user.findUnique({

      where: { email }

    })

    if (!user) {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH003',

          message: 'Invalid email or password'

        }

      })

    }

    // Check password

    const isValidPassword = await bcrypt.compare(password, user.password)

    if (!isValidPassword) {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH003',

          message: 'Invalid email or password'

        }

      })

    }

    // Generate tokens

    const { accessToken, refreshToken } = generateTokens(user.id)

    // Return user data (without password)

    const userData = {

      id: user.id,

      email: user.email,

      name: user.name,

      githubUrl: user.githubUrl,

      twitterUrl: user.twitterUrl,

      hashnodeUrl: user.hashnodeUrl,

      bio: user.bio,

      isVerified: user.isVerified

    }

    res.json({

      success: true,

      message: 'Login successful',

      data: {

        user: userData,

        tokens: {

          accessToken,

          refreshToken

        }

      }

    })

  } catch (error) {

    console.error('Login error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Login failed'

      }

    })

  }

}

module.exports = {

  register,

  login

}

```

**What this does:**

- `register`: Checks whitelist, hashes password, creates user, generates tokens

- `login`: Verifies credentials, generates tokens

- `generateTokens`: Creates JWT access and refresh tokens

- Includes proper error handling and logging

#### Step 17: Create Authentication Routes

Create `src/routes/authRoutes.js`:

```javascript

const express = require('express')

const { register, login } = require('../controllers/authController')

const { validate } = require('../middleware/validation')

const { registerSchema, loginSchema } = require('../validators/authValidator')

const router = express.Router()

// POST /api/auth/register

router.post('/register', validate(registerSchema), register)

// POST /api/auth/login

router.post('/login', validate(loginSchema), login)

module.exports = router

```

#### Step 18: Connect Routes to App

Update `src/app.js` - replace the API routes section:

```javascript

// ... (previous code stays the same)

// API routes

const authRoutes = require('./routes/authRoutes')

app.use('/api/auth', authRoutes)

// Handle unmatched API routes

app.use('/api', (req, res) => {

  res.status(404).json({

    success: false,

    error: {

      code: 'ENDPOINT_NOT_FOUND',

      message: 'API endpoint not found'

    }

  })

})

// ... (rest of the code stays the same)

```

#### Step 19: Add Test Data to Database

**We need to add some emails to whitelist so users can register.**

Create `src/utils/seedData.js`:

```javascript

const prisma = require('../config/database')

const seedWhitelist = async () => {

  const whitelistEmails = [

    {

      email: 'test@example.com',

      courseId: 'CHAI2024-001',

      courseName: 'Complete Web Development Bootcamp'

    },

    {

      email: 'john.doe@example.com',

      courseId: 'CHAI2024-001',

      courseName: 'Complete Web Development Bootcamp'

    },

    {

      email: 'jane.smith@example.com',

      courseId: 'CHAI2024-002',

      courseName: 'Advanced JavaScript Course'

    }

  ]

  for (const emailData of whitelistEmails) {

    await prisma.whitelist.upsert({

      where: { email: emailData.email },

      update: {},

      create: emailData

    })

  }

  console.log('Whitelist seeded successfully!')

}

#### Step 19: Add Test Data to Database (continued)

```javascript

// Add to src/utils/seedData.js

const seedWhitelist = async () => {

  // ... (previous code)

  console.log('Whitelist seeded successfully!')

}

// Run seeding if this file is executed directly

if (require.main === module) {

  seedWhitelist()

    .then(() => {

      console.log('Database seeding completed')

      process.exit(0)

    })

    .catch((error) => {

      console.error('Database seeding failed:', error)

      process.exit(1)

    })

}

module.exports = { seedWhitelist }

```

**Run the seeding:**

```bash

node src/utils/seedData.js

```

#### Step 20: Test Authentication Endpoints

**Start your server:**

```bash

npm run dev

```

**Test using VS Code Thunder Client extension or curl:**

1\. **Test Registration:**

   - Method: POST

   - URL: `http://localhost:5000/api/auth/register`

   - Headers: `Content-Type: application/json`

   - Body:

   ```json

   {

     "email": "test@example.com",

     "name": "Test User",

     "password": "Password123",

     "githubUrl": "https://github.com/testuser",

     "bio": "I'm learning web development"

   }

   ```

2\. **Test Login:**

   - Method: POST

   - URL: `http://localhost:5000/api/auth/login`

   - Headers: `Content-Type: application/json`

   - Body:

   ```json

   {

     "email": "test@example.com",

     "password": "Password123"

   }

   ```

**Expected Results:**

- Registration should return user data and tokens

- Login should return user data and tokens

- Invalid emails should be rejected (not in whitelist)

- Invalid data should return validation errors

### Phase 5: Authentication Middleware

#### Step 21: Create Authentication Middleware

Create `src/middleware/auth.js`:

```javascript

const jwt = require('jsonwebtoken')

const prisma = require('../config/database')

// Middleware to check if user is authenticated

const authenticate = async (req, res, next) => {

  try {

    // Get token from Authorization header

    const authHeader = req.headers.authorization

    if (!authHeader || !authHeader.startsWith('Bearer ')) {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH006',

          message: 'Access token required'

        }

      })

    }

    // Extract token (remove 'Bearer ' prefix)

    const token = authHeader.substring(7)

    if (!token) {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH006',

          message: 'Access token required'

        }

      })

    }

    // Verify JWT token

    const decoded = jwt.verify(token, process.env.JWT_SECRET)

    // Check if user still exists in database

    const user = await prisma.user.findUnique({

      where: { id: decoded.userId },

      select: {

        id: true,

        email: true,

        name: true,

        isVerified: true,

        githubUrl: true,

        twitterUrl: true,

        hashnodeUrl: true,

        bio: true

      }

    })

    if (!user) {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH005',

          message: 'Invalid or expired token'

        }

      })

    }

    // For now, we'll skip email verification requirement

    // Later you can uncomment this if you implement email verification

    /*

    if (!user.isVerified) {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH004',

          message: 'Please verify your email before accessing this resource'

        }

      })

    }

    */

    // Attach user to request object

    req.user = user

    next()

  } catch (error) {

    console.error('Authentication error:', error)

    if (error.name === 'JsonWebTokenError') {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH005',

          message: 'Invalid token'

        }

      })

    }

    if (error.name === 'TokenExpiredError') {

      return res.status(401).json({

        success: false,

        error: {

          code: 'AUTH005',

          message: 'Token expired'

        }

      })

    }

    return res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Authentication failed'

      }

    })

  }

}

module.exports = { authenticate }

```

**What this does:**

- Extracts JWT token from request headers

- Verifies the token is valid and not expired

- Looks up the user in the database

- Attaches user info to the request for use in other routes

- Handles various error scenarios

#### Step 22: Test Protected Route

Add a test route to `src/routes/authRoutes.js`:

```javascript

const express = require('express')

const { register, login } = require('../controllers/authController')

const { validate } = require('../middleware/validation')

const { registerSchema, loginSchema } = require('../validators/authValidator')

const { authenticate } = require('../middleware/auth')

const router = express.Router()

// POST /api/auth/register

router.post('/register', validate(registerSchema), register)

// POST /api/auth/login

router.post('/login', validate(loginSchema), login)

// GET /api/auth/me - Test protected route

router.get('/me', authenticate, (req, res) => {

  res.json({

    success: true,

    data: {

      user: req.user

    }

  })

})

module.exports = router

```

**Test the protected route:**

1\. First, login to get a token

2\. Test GET `http://localhost:5000/api/auth/me`

3\. Headers: `Authorization: Bearer YOUR_ACCESS_TOKEN`

### Phase 6: Group Management System

#### Step 23: Create Group Validators

Create `src/validators/groupValidator.js`:

```javascript

const Joi = require('joi')

// Validation for creating a group

const createGroupSchema = Joi.object({

  name: Joi.string()

    .min(3)

    .max(100)

    .trim()

    .required()

    .messages({

      'string.min': 'Group name must be at least 3 characters long',

      'string.max': 'Group name cannot exceed 100 characters',

      'any.required': 'Group name is required'

    }),

  description: Joi.string()

    .max(1000)

    .allow('', null)

    .messages({

      'string.max': 'Description cannot exceed 1000 characters'

    }),

  purpose: Joi.string()

    .valid('LEARNING', 'PROJECT', 'DISCUSSION', 'NETWORKING', 'OTHER')

    .required()

    .messages({

      'any.only': 'Purpose must be one of: LEARNING, PROJECT, DISCUSSION, NETWORKING, OTHER',

      'any.required': 'Purpose is required'

    }),

  maxMembers: Joi.number()

    .integer()

    .min(2)

    .max(10)

    .default(4)

    .messages({

      'number.min': 'Maximum members must be at least 2',

      'number.max': 'Maximum members cannot exceed 10'

    })

})

// Validation for join request

const joinRequestSchema = Joi.object({

  message: Joi.string()

    .min(50)

    .max(1000)

    .trim()

    .required()

    .messages({

      'string.min': 'Join request message must be at least 50 characters long',

      'string.max': 'Join request message cannot exceed 1000 characters',

      'any.required': 'Join request message is required'

    })

})

// Validation for responding to join request

const requestResponseSchema = Joi.object({

  action: Joi.string()

    .valid('approve', 'reject')

    .required()

    .messages({

      'any.only': 'Action must be either approve or reject',

      'any.required': 'Action is required'

    }),

  responseMessage: Joi.string()

    .max(500)

    .allow('', null)

    .messages({

      'string.max': 'Response message cannot exceed 500 characters'

    })

})

module.exports = {

  createGroupSchema,

  joinRequestSchema,

  requestResponseSchema

}

```

#### Step 24: Create Group Controller

Create `src/controllers/groupController.js`:

```javascript

const prisma = require('../config/database')

// Get all groups (with search and filtering)

const getAllGroups = async (req, res) => {

  try {

    const {

      page = 1,

      limit = 10,

      search = '',

      purpose = '',

      hasSpots = ''

    } = req.query

    const skip = (parseInt(page) - 1) * parseInt(limit)

    // Build where conditions

    const where = {

      isActive: true

    }

    // Add search functionality

    if (search) {

      where.OR = [

        { name: { contains: search, mode: 'insensitive' } },

        { description: { contains: search, mode: 'insensitive' } }

      ]

    }

    // Filter by purpose

    if (purpose && purpose !== 'ALL') {

      where.purpose = purpose

    }

    // Get groups with member count

    const groups = await prisma.group.findMany({

      where,

      include: {

        leader: {

          select: {

            id: true,

            name: true,

            githubUrl: true,

            twitterUrl: true,

            hashnodeUrl: true

          }

        },

        members: {

          where: { status: 'ACTIVE' },

          select: { id: true }

        },

        _count: {

          select: {

            members: {

              where: { status: 'ACTIVE' }

            }

          }

        }

      },

      skip,

      take: parseInt(limit),

      orderBy: { createdAt: 'desc' }

    })

    // Filter by available spots if requested

    let filteredGroups = groups

    if (hasSpots === 'true') {

      filteredGroups = groups.filter(group =>

        group._count.members < group.maxMembers

      )

    }

    // Format response data

    const formattedGroups = filteredGroups.map(group => ({

      id: group.id,

      name: group.name,

      description: group.description,

      purpose: group.purpose,

      maxMembers: group.maxMembers,

      memberCount: group._count.members,

      hasSpots: group._count.members < group.maxMembers,

      leader: group.leader,

      createdAt: group.createdAt

    }))

    // Get total count for pagination

    const totalItems = await prisma.group.count({ where })

    const totalPages = Math.ceil(totalItems / parseInt(limit))

    res.json({

      success: true,

      data: {

        groups: formattedGroups,

        pagination: {

          page: parseInt(page),

          totalPages,

          totalItems,

          hasNext: parseInt(page) < totalPages,

          hasPrev: parseInt(page) > 1

        }

      }

    })

  } catch (error) {

    console.error('Get groups error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to fetch groups'

      }

    })

  }

}

// Create a new group

const createGroup = async (req, res) => {

  try {

    const { name, description, purpose, maxMembers = 4 } = req.body

    const userId = req.user.id

    // Check if user is already in a group (as leader or member)

    const existingMembership = await prisma.groupMember.findFirst({

      where: {

        userId,

        status: 'ACTIVE'

      }

    })

    if (existingMembership) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'GROUP001',

          message: 'User already belongs to a group'

        }

      })

    }

    // Check if user is leader of any group

    const existingLeadership = await prisma.group.findFirst({

      where: {

        leaderId: userId,

        isActive: true

      }

    })

    if (existingLeadership) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'GROUP001',

          message: 'User already leads a group'

        }

      })

    }

    // Check if group name is unique

    const existingGroup = await prisma.group.findUnique({

      where: { name }

    })

    if (existingGroup) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'GROUP006',

          message: 'Group name already exists'

        }

      })

    }

    // Create group and add creator as member in a transaction

    const result = await prisma.$transaction(async (tx) => {

      // Create the group

      const group = await tx.group.create({

        data: {

          name,

          description: description || null,

          purpose,

          maxMembers,

          leaderId: userId

        },

        include: {

          leader: {

            select: {

              id: true,

              name: true,

              email: true,

              githubUrl: true,

              twitterUrl: true,

              hashnodeUrl: true

            }

          }

        }

      })

      // Add creator as first member

      await tx.groupMember.create({

        data: {

          groupId: group.id,

          userId: userId,

          status: 'ACTIVE'

        }

      })

      // Log activity

      await tx.activityLog.create({

        data: {

          userId,

          groupId: group.id,

          actionType: 'GROUP_CREATED',

          message: `Group "${group.name}" created successfully`

        }

      })

      return group

    })

    res.status(201).json({

      success: true,

      message: 'Group created successfully',

      data: { group: result }

    })

  } catch (error) {

    console.error('Create group error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to create group'

      }

    })

  }

}

// Get single group details

const getGroupDetails = async (req, res) => {

  try {

    const { id } = req.params

    const group = await prisma.group.findUnique({

      where: { id },

      include: {

        leader: {

          select: {

            id: true,

            name: true,

            email: true,

            githubUrl: true,

            twitterUrl: true,

            hashnodeUrl: true,

            bio: true

          }

        },

        members: {

          where: { status: 'ACTIVE' },

          include: {

            user: {

              select: {

                id: true,

                name: true,

                email: true,

                githubUrl: true,

                twitterUrl: true,

                hashnodeUrl: true,

                bio: true

              }

            }

          },

          orderBy: { joinDate: 'asc' }

        },

        _count: {

          select: {

            members: {

              where: { status: 'ACTIVE' }

            }

          }

        }

      }

    })

    if (!group) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'GROUP002',

          message: 'Group not found'

        }

      })

    }

    // Format member data

    const formattedMembers = group.members.map(member => ({

      id: member.user.id,

      name: member.user.name,

      email: member.user.email,

      githubUrl: member.user.githubUrl,

      twitterUrl: member.user.twitterUrl,

      hashnodeUrl: member.user.hashnodeUrl,

      bio: member.user.bio,

      joinDate: member.joinDate,

      isLeader: member.user.id === group.leaderId

    }))

    const response = {

      id: group.id,

      name: group.name,

      description: group.description,

      purpose: group.purpose,

      maxMembers: group.maxMembers,

      memberCount: group._count.members,

      isActive: group.isActive,

      leader: group.leader,

      members: formattedMembers,

      createdAt: group.createdAt,

      updatedAt: group.updatedAt

    }

    res.json({

      success: true,

      data: { group: response }

    })

  } catch (error) {

    console.error('Get group details error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to fetch group details'

      }

    })

  }

}

module.exports = {

  getAllGroups,

  createGroup,

  getGroupDetails

}

```

#### Step 25: Create Group Routes

Create `src/routes/groupRoutes.js`:

```javascript

const express = require('express')

const { getAllGroups, createGroup, getGroupDetails } = require('../controllers/groupController')

const { authenticate } = require('../middleware/auth')

const { validate } = require('../middleware/validation')

const { createGroupSchema } = require('../validators/groupValidator')

const router = express.Router()

// GET /api/groups - Get all groups (public)

router.get('/', getAllGroups)

// GET /api/groups/:id - Get group details (public)

router.get('/:id', getGroupDetails)

// POST /api/groups - Create group (authenticated)

router.post('/', authenticate, validate(createGroupSchema), createGroup)

module.exports = router

```

#### Step 26: Add Group Routes to App

Update `src/app.js`:

```javascript

// ... (previous code)

// API routes

const authRoutes = require('./routes/authRoutes')

const groupRoutes = require('./routes/groupRoutes')

app.use('/api/auth', authRoutes)

app.use('/api/groups', groupRoutes)

// ... (rest of the code)

```

#### Step 27: Test Group Management

**Test creating a group:**

1\. First login to get access token

2\. POST to `http://localhost:5000/api/groups`

3\. Headers: `Authorization: Bearer YOUR_TOKEN`

4\. Body:

   ```json

   {

     "name": "React Study Group",

     "description": "Learning React best practices together",

     "purpose": "LEARNING",

     "maxMembers": 4

   }

   ```

**Test getting groups:**

- GET `http://localhost:5000/api/groups`

- Should return list of groups

**Test group details:**

- GET `http://localhost:5000/api/groups/GROUP_ID`

- Should return detailed group info with members

### Phase 7: Membership Request System

#### Step 28: Create Membership Controller

Create `src/controllers/membershipController.js`:

```javascript

const prisma = require('../config/database')

// Send join request

const sendJoinRequest = async (req, res) => {

  try {

    const { id: groupId } = req.params

    const { message } = req.body

    const userId = req.user.id

    // Check if group exists

    const group = await prisma.group.findUnique({

      where: { id: groupId },

      include: {

        _count: {

          select: {

            members: { where: { status: 'ACTIVE' } }

          }

        }

      }

    })

    if (!group || !group.isActive) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'GROUP002',

          message: 'Group not found'

        }

      })

    }

    // Check if group has space

    if (group._count.members >= group.maxMembers) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'GROUP003',

          message: 'Group has reached maximum capacity'

        }

      })

    }

    // Check if user is already in any group

    const existingMembership = await prisma.groupMember.findFirst({

      where: {

        userId,

        status: 'ACTIVE'

      }

    })

    if (existingMembership) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'GROUP001',

          message: 'User already belongs to a group'

        }

      })

    }

    // Check if user is the group leader

    if (group.leaderId === userId) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'MEMBER004',

          message: 'Cannot send join request to your own group'

        }

      })

    }

    // Check if user already has pending request

    const existingRequest = await prisma.membershipRequest.findFirst({

      where: {

        groupId,

        userId,

        status: 'PENDING'

      }

    })

    if (existingRequest) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'MEMBER001',

          message: 'Join request already pending for this group'

        }

      })

    }

    // Create join request

    const joinRequest = await prisma.$transaction(async (tx) => {

      const request = await tx.membershipRequest.create({

        data: {

          groupId,

          userId,

          message,

          status: 'PENDING'

        },

        include: {

          user: {

            select: {

              id: true,

              name: true,

              email: true,

              githubUrl: true,

              twitterUrl: true,

              hashnodeUrl: true,

              bio: true

            }

          },

          group: {

            select: {

              id: true,

              name: true

            }

          }

        }

      })

      // Log activity

      await tx.activityLog.create({

        data: {

          userId,

          groupId,

          actionType: 'JOIN_REQUEST_SENT',

          message: `Join request sent to group "${group.name}"`

        }

      })

      return request

    })

    res.status(201).json({

      success: true,

      message: 'Join request sent successfully',

      data: { request: joinRequest }

    })

  } catch (error) {

    console.error('Send join request error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to send join request'

      }

    })

  }

}

// Get pending requests for a group (leaders only)

const getPendingRequests = async (req, res) => {

  try {

    const { id: groupId } = req.params

    const userId = req.user.id

    // Check if user is group leader

    const group = await prisma.group.findUnique({

      where: { id: groupId }

    })

    if (!group) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'GROUP002',

          message: 'Group not found'

        }

      })

    }

    if (group.leaderId !== userId) {

      return res.status(403).json({

        success: false,

        error: {

          code: 'GROUP004',

          message: 'Only group leader can view join requests'

        }

      })

    }

    // Get pending requests

    const requests = await prisma.membershipRequest.findMany({

      where: {

        groupId,

        status: 'PENDING'

      },

      include: {

        user: {

          select: {

            id: true,

            name: true,

            email: true,

            githubUrl: true,

            twitterUrl: true,

            hashnodeUrl: true,

            bio: true

          }

        }

      },

      orderBy: { createdAt: 'asc' }

    })

    res.json({

      success: true,

      data: { requests }

    })

  } catch (error) {

    console.error('Get pending requests error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to fetch pending requests'

      }

    })

  }

}

// Approve or reject join request

const respondToRequest = async (req, res) => {

  try {

    const { id: groupId, requestId } = req.params

    const { action, responseMessage } = req.body

    const userId = req.user.id

    // Check if user is group leader

    const group = await prisma.group.findUnique({

      where: { id: groupId },

      include: {

        _count: {

          select: {

            members: { where: { status: 'ACTIVE' } }

          }

        }

      }

    })

    if (!group) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'GROUP002',

          message: 'Group not found'

        }

      })

    }

    if (group.leaderId !== userId) {

      return res.status(403).json({

        success: false,

        error: {

          code: 'GROUP004',

          message: 'Only group leader can respond to join requests'

        }

      })

    }

    // Find the request

    const request = await prisma.membershipRequest.findUnique({

      where: { id: requestId },

      include: {

        user: {

          select: { id: true, name: true, email: true }

        }

      }

    })

    if (!request) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'MEMBER002',

          message: 'Membership request not found'

        }

      })

    }

    if (request.status !== 'PENDING') {

      return res.status(409).json({

        success: false,

        error: {

          code: 'MEMBER003',

          message: 'Request has already been processed'

        }

      })

    }

    // If approving, check if group still has space

    if (action === 'approve' && group._count.members >= group.maxMembers) {

      return res.status(409).json({

        success: false,

        error: {

          code: 'GROUP003',

          message: 'Group has reached maximum capacity'

        }

      })

    }

    // Process the request

    const result = await prisma.$transaction(async (tx) => {

      // Update request status

      const updatedRequest = await tx.membershipRequest.update({

        where: { id: requestId },

        data: {

          status: action === 'approve' ? 'APPROVED' : 'REJECTED',

          responseMessage: responseMessage || null

        }

      })

      let activityMessage = ''

      if (action === 'approve') {

        // Add user as group member

        await tx.groupMember.create({

          data: {

            groupId,

            userId: request.userId,

            status: 'ACTIVE'

          }

        })

        activityMessage = `Join request approved for user "${request.user.name}"`

        // Log activity for the new member

        await tx.activityLog.create({

          data: {

            userId: request.userId,

            groupId,

            actionType: 'JOIN_REQUEST_APPROVED',

            message: `Joined group "${group.name}"`

          }

        })

      } else {

        activityMessage = `Join request rejected for user "${request.user.name}"`

        // Log activity for the rejected user

        await tx.activityLog.create({

          data: {

            userId: request.userId,

            groupId,

            actionType: 'JOIN_REQUEST_REJECTED',

            message: `Join request rejected for group "${group.name}"`

          }

        })

      }

      // Log activity for the leader

      await tx.activityLog.create({

        data: {

          userId,

          groupId,

          actionType: action === 'approve' ? 'JOIN_REQUEST_APPROVED' : 'JOIN_REQUEST_REJECTED',

          message: activityMessage

        }

      })

      return updatedRequest

    })

    res.json({

      success: true,

      message: `Join request ${action}d successfully`,

      data: { request: result }

    })

  } catch (error) {

    console.error('Respond to request error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to process join request'

      }

    })

  }

}

module.exports = {

  sendJoinRequest,

  getPendingRequests,

  respondToRequest

}

```

#### Step 29: Create Membership Routes

Create `src/routes/membershipRoutes.js`:

```javascript

const express = require('express')

const {

  sendJoinRequest,

  getPendingRequests,

  respondToRequest

} = require('../controllers/membershipController')

const { authenticate } = require('../middleware/auth')

const { validate } = require('../middleware/validation')

const { joinRequestSchema, requestResponseSchema } = require('../validators/groupValidator')

const router = express.Router()

// POST /api/groups/:id/request - Send join request

router.post('/:id/request', authenticate, validate(joinRequestSchema), sendJoinRequest)

// GET /api/groups/:id/requests - Get pending requests (leaders only)

router.get('/:id/requests', authenticate, getPendingRequests)

// PUT /api/groups/:id/requests/:requestId - Respond to request

router.put('/:id/requests/:requestId', authenticate, validate(requestResponseSchema), respondToRequest)

module.exports = router

```

#### Step 30: Update Main Routes

Update `src/app.js`:

```javascript

// ... (previous code)

// API routes

const authRoutes = require('./routes/authRoutes')

const groupRoutes = require('./routes/groupRoutes')

const membershipRoutes = require('./routes/membershipRoutes')

app.use('/api/auth', authRoutes)

app.use('/api/groups', groupRoutes)

app.use('/api/groups', membershipRoutes) // Membership routes use group prefix

// ... (rest of the code)

```

### Phase 8: Testing Complete Backend

#### Step 31: Test Complete Workflow

**Test the complete user flow:**

1\. **Register User 1 (Group Leader):**

   ```json

   POST /api/auth/register

   {

     "email": "john.doe@example.com",

     "name": "John Doe",

     "password": "Password123"

   }

   ```

2\. **Register User 2 (Joiner):**

   ```json

   POST /api/auth/register

   {

     "email": "jane.smith@example.com",

     "name": "Jane Smith",

     "password": "Password123"

   }

   ```

3\. **User 1 Creates Group:**

   ```json

   POST /api/groups

   Headers

   #### Step 31: Test Complete Workflow (continued)

3\. **User 1 Creates Group:**

   ```json

   POST /api/groups

   Headers: Authorization: Bearer USER1_TOKEN

   {

     "name": "React Study Group",

     "description": "Learning React together",

     "purpose": "LEARNING",

     "maxMembers": 4

   }

   ```

4\. **User 2 Sends Join Request:**

   ```json

   POST /api/groups/GROUP_ID/request

   Headers: Authorization: Bearer USER2_TOKEN

   {

     "message": "Hi! I'm passionate about React and have been learning for 3 months. I'd love to join this study group to learn from others and share my knowledge. I'm committed to participating actively in discussions and study sessions."

   }

   ```

5\. **User 1 (Leader) Views Pending Requests:**

   ```

   GET /api/groups/GROUP_ID/requests

   Headers: Authorization: Bearer USER1_TOKEN

   ```

6\. **User 1 Approves Request:**

   ```json

   PUT /api/groups/GROUP_ID/requests/REQUEST_ID

   Headers: Authorization: Bearer USER1_TOKEN

   {

     "action": "approve",

     "responseMessage": "Welcome to the group! Your enthusiasm for React is exactly what we're looking for."

   }

   ```

7\. **Verify Group Details:**

   ```

   GET /api/groups/GROUP_ID

   ```

   Should now show 2 members (leader + approved member).

**Expected Results:**

- All endpoints should return proper success/error responses

- Group membership should be updated correctly

- Activity logs should be created for all actions

- Only authorized users can perform their respective actions

---

## Summary of Backend Completion

**What we've built so far:**

✅ **Project Setup**

- Node.js project with modern tooling (ESLint, Prettier)

- PostgreSQL database with Prisma ORM

- Environment configuration

- Development scripts and workflows

✅ **Database Schema**

- Users, Whitelist, Groups, GroupMembers

- MembershipRequests, NoticeBoard, ActivityLogs

- Proper relationships and constraints

- Database migrations and seeding

✅ **Authentication System**

- User registration with whitelist validation

- Secure password hashing with bcrypt

- JWT token-based authentication

- Login/logout functionality

- Protected route middleware

✅ **Group Management**

- Create groups (leaders only)

- List groups with search and filtering

- View group details with member information

- One group per user policy enforcement

✅ **Membership System**

- Send join requests with messages

- Approve/reject requests (leaders only)

- Automatic member addition on approval

- Activity logging for all actions

✅ **Error Handling & Validation**

- Comprehensive input validation with Joi

- Structured error responses

- Proper HTTP status codes

- Activity logging system

**What we still need to build:**

- Notice Board system

- User profile management

- Activity feed endpoints

- Advanced authorization middleware

- Email verification system (optional for MVP)

---

## Next Steps: Notice Board & Profile System

### Phase 9: Notice Board Implementation

#### Step 32: Create Notice Board Controller

Create `src/controllers/noticeBoardController.js`:

```javascript

const prisma = require('../config/database')

// Get all notices for a group (members only)

const getGroupNotices = async (req, res) => {

  try {

    const { id: groupId } = req.params

    const { page = 1, limit = 20, type = '' } = req.query

    const userId = req.user.id

    // Check if user is a member of this group

    const membership = await prisma.groupMember.findFirst({

      where: {

        groupId,

        userId,

        status: 'ACTIVE'

      }

    })

    if (!membership) {

      return res.status(403).json({

        success: false,

        error: {

          code: 'GROUP005',

          message: 'Only group members can view notices'

        }

      })

    }

    const skip = (parseInt(page) - 1) * parseInt(limit)

    // Build where conditions

    const where = { groupId }

    if (type && type !== 'ALL') {

      where.type = type

    }

    // Get notices

    const notices = await prisma.noticeBoard.findMany({

      where,

      include: {

        author: {

          select: {

            id: true,

            name: true,

            githubUrl: true

          }

        }

      },

      orderBy: [

        { isPinned: 'desc' }, // Pinned notices first

        { createdAt: 'desc' }  // Then by newest

      ],

      skip,

      take: parseInt(limit)

    })

    // Get total count for pagination

    const totalItems = await prisma.noticeBoard.count({ where })

    const totalPages = Math.ceil(totalItems / parseInt(limit))

    res.json({

      success: true,

      data: {

        notices,

        pagination: {

          page: parseInt(page),

          totalPages,

          totalItems,

          hasNext: parseInt(page) < totalPages,

          hasPrev: parseInt(page) > 1

        }

      }

    })

  } catch (error) {

    console.error('Get group notices error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to fetch notices'

      }

    })

  }

}

// Create new notice (members only)

const createNotice = async (req, res) => {

  try {

    const { id: groupId } = req.params

    const { title, content, type, isPinned = false } = req.body

    const userId = req.user.id

    // Check if user is a member of this group

    const membership = await prisma.groupMember.findFirst({

      where: {

        groupId,

        userId,

        status: 'ACTIVE'

      },

      include: {

        group: {

          select: { name: true, leaderId: true }

        }

      }

    })

    if (!membership) {

      return res.status(403).json({

        success: false,

        error: {

          code: 'GROUP005',

          message: 'Only group members can create notices'

        }

      })

    }

    // Only leaders can pin notices

    const canPin = membership.group.leaderId === userId

    const shouldPin = isPinned && canPin

    // Create notice

    const notice = await prisma.$transaction(async (tx) => {

      const newNotice = await tx.noticeBoard.create({

        data: {

          groupId,

          authorId: userId,

          title,

          content,

          type,

          isPinned: shouldPin

        },

        include: {

          author: {

            select: {

              id: true,

              name: true,

              githubUrl: true

            }

          }

        }

      })

      // Log activity

      await tx.activityLog.create({

        data: {

          userId,

          groupId,

          actionType: 'NOTICE_POSTED',

          message: `Posted notice: "${title}" in group "${membership.group.name}"`

        }

      })

      return newNotice

    })

    res.status(201).json({

      success: true,

      message: 'Notice created successfully',

      data: { notice }

    })

  } catch (error) {

    console.error('Create notice error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to create notice'

      }

    })

  }

}

// Update notice (author only)

const updateNotice = async (req, res) => {

  try {

    const { id: groupId, noticeId } = req.params

    const { title, content, type, isPinned } = req.body

    const userId = req.user.id

    // Find notice and check permissions

    const notice = await prisma.noticeBoard.findUnique({

      where: { id: noticeId },

      include: {

        group: {

          select: { name: true, leaderId: true }

        }

      }

    })

    if (!notice) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'NOTICE_NOT_FOUND',

          message: 'Notice not found'

        }

      })

    }

    if (notice.authorId !== userId) {

      return res.status(403).json({

        success: false,

        error: {

          code: 'UNAUTHORIZED',

          message: 'Only notice author can update this notice'

        }

      })

    }

    // Only leaders can pin/unpin notices

    const isLeader = notice.group.leaderId === userId

    const updateData = { title, content, type }

    if (isLeader && isPinned !== undefined) {

      updateData.isPinned = isPinned

    }

    // Update notice

    const updatedNotice = await prisma.$transaction(async (tx) => {

      const updated = await tx.noticeBoard.update({

        where: { id: noticeId },

        data: updateData,

        include: {

          author: {

            select: {

              id: true,

              name: true,

              githubUrl: true

            }

          }

        }

      })

      // Log activity

      await tx.activityLog.create({

        data: {

          userId,

          groupId,

          actionType: 'NOTICE_UPDATED',

          message: `Updated notice: "${title}" in group "${notice.group.name}"`

        }

      })

      return updated

    })

    res.json({

      success: true,

      message: 'Notice updated successfully',

      data: { notice: updatedNotice }

    })

  } catch (error) {

    console.error('Update notice error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to update notice'

      }

    })

  }

}

// Delete notice (author only)

const deleteNotice = async (req, res) => {

  try {

    const { id: groupId, noticeId } = req.params

    const userId = req.user.id

    // Find notice and check permissions

    const notice = await prisma.noticeBoard.findUnique({

      where: { id: noticeId },

      include: {

        group: {

          select: { name: true }

        }

      }

    })

    if (!notice) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'NOTICE_NOT_FOUND',

          message: 'Notice not found'

        }

      })

    }

    if (notice.authorId !== userId) {

      return res.status(403).json({

        success: false,

        error: {

          code: 'UNAUTHORIZED',

          message: 'Only notice author can delete this notice'

        }

      })

    }

    // Delete notice

    await prisma.$transaction(async (tx) => {

      await tx.noticeBoard.delete({

        where: { id: noticeId }

      })

      // Log activity

      await tx.activityLog.create({

        data: {

          userId,

          groupId,

          actionType: 'NOTICE_DELETED',

          message: `Deleted notice: "${notice.title}" from group "${notice.group.name}"`

        }

      })

    })

    res.json({

      success: true,

      message: 'Notice deleted successfully'

    })

  } catch (error) {

    console.error('Delete notice error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to delete notice'

      }

    })

  }

}

module.exports = {

  getGroupNotices,

  createNotice,

  updateNotice,

  deleteNotice

}

```

#### Step 33: Create Notice Board Validators

Create `src/validators/noticeBoardValidator.js`:

```javascript

const Joi = require('joi')

const createNoticeSchema = Joi.object({

  title: Joi.string()

    .min(5)

    .max(200)

    .trim()

    .required()

    .messages({

      'string.min': 'Title must be at least 5 characters long',

      'string.max': 'Title cannot exceed 200 characters',

      'any.required': 'Title is required'

    }),

  content: Joi.string()

    .min(10)

    .max(5000)

    .required()

    .messages({

      'string.min': 'Content must be at least 10 characters long',

      'string.max': 'Content cannot exceed 5000 characters',

      'any.required': 'Content is required'

    }),

  type: Joi.string()

    .valid('MEETING', 'ANNOUNCEMENT', 'RESOURCE', 'DISCUSSION')

    .required()

    .messages({

      'any.only': 'Type must be one of: MEETING, ANNOUNCEMENT, RESOURCE, DISCUSSION',

      'any.required': 'Type is required'

    }),

  isPinned: Joi.boolean()

    .default(false)

})

const updateNoticeSchema = Joi.object({

  title: Joi.string()

    .min(5)

    .max(200)

    .trim()

    .required()

    .messages({

      'string.min': 'Title must be at least 5 characters long',

      'string.max': 'Title cannot exceed 200 characters',

      'any.required': 'Title is required'

    }),

  content: Joi.string()

    .min(10)

    .max(5000)

    .required()

    .messages({

      'string.min': 'Content must be at least 10 characters long',

      'string.max': 'Content cannot exceed 5000 characters',

      'any.required': 'Content is required'

    }),

  type: Joi.string()

    .valid('MEETING', 'ANNOUNCEMENT', 'RESOURCE', 'DISCUSSION')

    .required()

    .messages({

      'any.only': 'Type must be one of: MEETING, ANNOUNCEMENT, RESOURCE, DISCUSSION',

      'any.required': 'Type is required'

    }),

  isPinned: Joi.boolean()

    .optional()

})

module.exports = {

  createNoticeSchema,

  updateNoticeSchema

}

```

#### Step 34: Create Notice Board Routes

Create `src/routes/noticeBoardRoutes.js`:

```javascript

const express = require('express')

const {

  getGroupNotices,

  createNotice,

  updateNotice,

  deleteNotice

} = require('../controllers/noticeBoardController')

const { authenticate } = require('../middleware/auth')

const { validate } = require('../middleware/validation')

const { createNoticeSchema, updateNoticeSchema } = require('../validators/noticeBoardValidator')

const router = express.Router()

// GET /api/groups/:id/notices - Get group notices (members only)

router.get('/:id/notices', authenticate, getGroupNotices)

// POST /api/groups/:id/notices - Create notice (members only)

router.post('/:id/notices', authenticate, validate(createNoticeSchema), createNotice)

// PUT /api/groups/:id/notices/:noticeId - Update notice (author only)

router.put('/:id/notices/:noticeId', authenticate, validate(updateNoticeSchema), updateNotice)

// DELETE /api/groups/:id/notices/:noticeId - Delete notice (author only)

router.delete('/:id/notices/:noticeId', authenticate, deleteNotice)

module.exports = router

```

#### Step 35: Update Main App Routes

Update `src/app.js`:

```javascript

// ... (previous code)

// API routes

const authRoutes = require('./routes/authRoutes')

const groupRoutes = require('./routes/groupRoutes')

const membershipRoutes = require('./routes/membershipRoutes')

const noticeBoardRoutes = require('./routes/noticeBoardRoutes')

app.use('/api/auth', authRoutes)

app.use('/api/groups', groupRoutes)

app.use('/api/groups', membershipRoutes)

app.use('/api/groups', noticeBoardRoutes)

// ... (rest of the code)

```

### Phase 10: User Profile & Activity System

#### Step 36: Create User Profile Controller

Create `src/controllers/userController.js`:

```javascript

const prisma = require('../config/database')

// Get current user profile

const getMyProfile = async (req, res) => {

  try {

    const userId = req.user.id

    const user = await prisma.user.findUnique({

      where: { id: userId },

      select: {

        id: true,

        email: true,

        name: true,

        githubUrl: true,

        twitterUrl: true,

        hashnodeUrl: true,

        bio: true,

        isVerified: true,

        createdAt: true

      }

    })

    // Get user's current group membership

    const groupMembership = await prisma.groupMember.findFirst({

      where: {

        userId,

        status: 'ACTIVE'

      },

      include: {

        group: {

          select: {

            id: true,

            name: true,

            purpose: true,

            leaderId: true

          }

        }

      }

    })

    // Get activity count

    const activityCount = await prisma.activityLog.count({

      where: { userId }

    })

    const profile = {

      ...user,

      currentGroup: groupMembership ? {

        id: groupMembership.group.id,

        name: groupMembership.group.name,

        purpose: groupMembership.group.purpose,

        role: groupMembership.group.leaderId === userId ? 'leader' : 'member',

        joinDate: groupMembership.joinDate

      } : null,

      activityCount

    }

    res.json({

      success: true,

      data: { user: profile }

    })

  } catch (error) {

    console.error('Get my profile error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to fetch profile'

      }

    })

  }

}

// Update user profile

const updateProfile = async (req, res) => {

  try {

    const userId = req.user.id

    const { name, githubUrl, twitterUrl, hashnodeUrl, bio } = req.body

    const updatedUser = await prisma.user.update({

      where: { id: userId },

      data: {

        name,

        githubUrl: githubUrl || null,

        twitterUrl: twitterUrl || null,

        hashnodeUrl: hashnodeUrl || null,

        bio: bio || null

      },

      select: {

        id: true,

        email: true,

        name: true,

        githubUrl: true,

        twitterUrl: true,

        hashnodeUrl: true,

        bio: true,

        isVerified: true,

        createdAt: true,

        updatedAt: true

      }

    })

    res.json({

      success: true,

      message: 'Profile updated successfully',

      data: { user: updatedUser }

    })

  } catch (error) {

    console.error('Update profile error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to update profile'

      }

    })

  }

}

// Get public user profile

const getUserProfile = async (req, res) => {

  try {

    const { id } = req.params

    const user = await prisma.user.findUnique({

      where: { id },

      select: {

        id: true,

        name: true,

        githubUrl: true,

        twitterUrl: true,

        hashnodeUrl: true,

        bio: true,

        createdAt: true

      }

    })

    if (!user) {

      return res.status(404).json({

        success: false,

        error: {

          code: 'USER_NOT_FOUND',

          message: 'User not found'

        }

      })

    }

    // Get user's current group membership

    const groupMembership = await prisma.groupMember.findFirst({

      where: {

        userId: id,

        status: 'ACTIVE'

      },

      include: {

        group: {

          select: {

            id: true,

            name: true,

            purpose: true,

            leaderId: true

          }

        }

      }

    })

    // Get activity count

    const activityCount = await prisma.activityLog.count({

      where: { userId: id }

    })

    const profile = {

      ...user,

      currentGroup: groupMembership ? {

        id: groupMembership.group.id,

        name: groupMembership.group.name,

        purpose: groupMembership.group.purpose,

        role: groupMembership.group.leaderId === id ? 'leader' : 'member',

        joinDate: groupMembership.joinDate

      } : null,

      activityCount

    }

    res.json({

      success: true,

      data: { user: profile }

    })

  } catch (error) {

    console.error('Get user profile error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to fetch user profile'

      }

    })

  }

}

// Get user activity history

const getUserActivity = async (req, res) => {

  try {

    const userId = req.user.id

    const { page = 1, limit = 20, actionType = '' } = req.query

    const skip = (parseInt(page) - 1) * parseInt(limit)

    // Build where conditions

    const where = { userId }

    if (actionType && actionType !== 'ALL') {

      where.actionType = actionType

    }

    const activities = await prisma.activityLog.findMany({

      where,

      include: {

        group: {

          select: {

            id: true,

            name: true

          }

        }

      },

      orderBy: { createdAt: 'desc' },

      skip,

      take: parseInt(limit)

    })

    // Get total count for pagination

    const totalItems = await prisma.activityLog.count({ where })

    const totalPages = Math.ceil(totalItems / parseInt(limit))

    res.json({

      success: true,

      data: {

        activities,

        pagination: {

          page: parseInt(page),

          totalPages,

          totalItems,

          hasNext: parseInt(page) < totalPages,

          hasPrev: parseInt(page) > 1

        }

      }

    })

  } catch (error) {

    console.error('Get user activity error:', error)

    res.status(500).json({

      success: false,

      error: {

        code: 'INTERNAL_ERROR',

        message: 'Failed to fetch activity history'

      }

    })

  }

}

module.exports = {

  getMyProfile,

  updateProfile,

  getUserProfile,

  getUserActivity

}

```

#### Step 37: Create User Profile Validators

Create `src/validators/userValidator.js`:

```javascript

const Joi = require('joi')

const updateProfileSchema = Joi.object({

  name: Joi.string()

    .min(2)

    .max(100)

    .trim()

    .required()

    .messages({

      'string.min': 'Name must be at least 2 characters long',

      'string.max': 'Name cannot exceed 100 characters',

      'any.required': 'Name is required'

    }),

  githubUrl: Joi.string()

    .uri()

    .pattern(/^https:\/\/(www\.)?github\.com\/[a-zA-Z0-9-_]+\/?$/)

    .allow('', null)

    .messages({

      'string.pattern.base': 'Please provide a valid GitHub profile URL'

    }),

  twitterUrl: Joi.string()

    .uri()

    .pattern(/^https:\/\/(www\.)?twitter\.com\/[a-zA-Z0-9_]+\/?$/)

    .allow('', null)

    .messages({

      'string.pattern.base': 'Please provide a valid Twitter profile URL'

    }),

  hashnodeUrl: Joi.string()

    .uri()

    .pattern(/^https:\/\/[a-zA-Z0-9-_]+\.hashnode\.(dev|com)\/?$/)

    .allow('', null)

    .messages({

      'string.pattern.base': 'Please provide a valid Hashnode blog URL'

    }),

  bio: Joi.string()

    .max(500)

    .allow('', null)

    .messages({

      'string.max': 'Bio cannot exceed 500 characters'

    })

})

module.exports = {

  updateProfileSchema

}

```

#### Step 38: Create User Routes

Create `src/routes/userRoutes.js`:

```javascript

const express = require('express')

const {

  getMyProfile,

  updateProfile,

  getUserProfile,

  getUserActivity

} = require('../controllers/userController')

const { authenticate } = require('../middleware/auth')

const { validate } = require('../middleware/validation')

const { updateProfileSchema } = require('../validators/userValidator')

const router = express.Router()

// GET /api/users/me - Get current user profile

router.get('/me', authenticate, getMyProfile)

// PUT /api/users/me - Update current user profile

router.put('/me', authenticate, validate(updateProfileSchema), updateProfile)

// GET /api/users/me/activity - Get current user activity history

router.get('/me/activity', authenticate, getUserActivity)

// GET /api/users/:id - Get public user profile

router.get('/:id', getUserProfile)

module.exports = router

```

#### Step 39: Final Route Integration

Update `src/app.js`:

```javascript

// ... (previous code)

// API routes

const authRoutes = require('./routes/authRoutes')

const groupRoutes = require('./routes/groupRoutes')

const membershipRoutes = require('./routes/membershipRoutes')

const noticeBoardRoutes = require('./routes/noticeBoardRoutes')

const userRoutes = require('./routes/userRoutes')

app.use('/api/auth', authRoutes)

app.use('/api/groups', groupRoutes)

app.use('/api/groups', membershipRoutes)

app.use('/api/groups', noticeBoardRoutes)

app.use('/api/users', userRoutes)

// ... (rest of the code)

```

---

## Backend Completion Summary

**🎉 Congratulations! Your backend is now complete!**

### What You've Built:

✅ **Complete Authentication System**

- User registration with whitelist validation

- Secure JWT-based authentication

- Password hashing and security

✅ **Full Group Management**

- Create, view, and manage groups

- One group per user policy

- Group search and filtering

✅ **Complete Membership System**

- Join request workflow

- Leader approval/rejection system

- Member management

✅ **Notice Board System**

- Create, edit, delete notices

- Group-specific notice boards

- Notice categorization and pinning

✅ **User Profile System**

- Public and private profile views

- Profile editing capabilities

- Activity history tracking

✅ **Activity Logging System**

- Comprehensive activity tracking

- Accountability through transparency

- Activity filtering and pagination

### API Endpoints Summary:

**Authentication:**

- `POST /api/auth/register` - Register new user

- `POST /api/auth/login` - User login

- `GET /api/auth/me` - Get current user info

**Groups:**

- `GET /api/groups` - List all groups

- `POST /api/groups` - Create group

- `GET /api/groups/:id` - Group details

**Membership:**

- `POST /api/groups/:id/request` - Send join request

- `GET /api/groups/:id/requests` - View pending requests (leaders)

- `PUT /api/groups/:id/requests/:requestId` - Approve/reject request

**Notice Board:**

- `GET /api/groups/:id/notices` - Get group notices

- `POST /api/groups/:id/notices` - Create notice

- `PUT /api/groups/:id/notices/:noticeId` - Update notice

- `DELETE /api/groups/:id/notices/:noticeId` - Delete notice

**Users:**

- `GET /api/users/me` - Get my profile

- `PUT /api/users/me` - Update my profile

- `GET /api/users/me/activity` - My activity history

- `GET /api/users/:id` - View user profile

### Final Testing:

**Test the complete workflow one more time:**

1\. Register multiple users

2\. Create groups

3\. Send and approve join requests

4\. Create and manage notices

5\. Update profiles

6\. View activity history

Your backend is now production-ready! Next, we'll build the React frontend with Vite to create a beautiful user interface for all these features.

---

**Ready to move on to frontend development?** Let me know and I'll provide the detailed React + Vite implementation guide!
