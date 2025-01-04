# Typescript Backend Setup

This doc contains only very generic backend structure from which inspiration can be drawn but not followed religiously. Get Idea what file and configuration can be kept where but modify according to your project requirements and needs.

My personal usage Snippets will linked slowly and steady as I get time.

## 1. Project Structure

```bash
src/                # Main source code for the application
├── config/         # Configuration files for external services and app setup
│   ├── aws.config.ts         # AWS service configuration
│   ├── database.config.ts    # Database connection configuration
│   ├── firebase.config.ts    # Firebase service configuration
│   ├── mail.config.ts        # Mail service configuration
│   └── socket.config.ts      # WebSocket configuration
├── constants.ts    # Reusable constants for the application
├── controllers/    # Application logic for handling requests
│   ├── auth.controller.ts    # Authentication-related request handling
│   ├── user.controller.ts    # User-related request handling
│   └── ai.controller.ts      # AI-related request handling
├── middleware/     # Middleware functions for request/response processing
│   ├── auth.middleware.ts    # Authentication checks middleware
│   ├── error.middleware.ts   # Error handling middleware
│   ├── validation.middleware.ts  # Request validation middleware
│   └── rateLimiter.middleware.ts # Rate-limiting middleware
├── models/         # Database schemas/models
│   ├── user.model.ts         # User schema
│   ├── chat.model.ts         # Chat schema
│   └── session.model.ts      # Session schema
├── routes/         # API endpoint routes
│   ├── auth.routes.ts        # Authentication routes
│   ├── user.routes.ts        # User-related routes
│   └── ai.routes.ts          # AI-related routes
├── services/       # Business logic for app functionalities
│   ├── auth.service.ts       # Authentication logic
│   ├── user.service.ts       # User-related logic
│   ├── mail.service.ts       # Mail-handling logic
│   ├── ai.service.ts         # AI-related logic
│   └── socket.service.ts     # WebSocket handling logic
├── types/          # Type definitions for TypeScript
│   ├── express.d.ts          # Express.js type extensions
│   ├── user.types.ts         # User-related types
│   └── environment.d.ts      # Environment variable types
├── utils/          # Utility functions
│   ├── logger.ts             # Logger utility
│   ├── errors.ts             # Error handling utilities
│   └── validators.ts         # Validation utilities
├── app.ts          # Application initialization
└── server.ts       # Server entry point

tests/              # Test files for application
├── integration/    # Integration tests
│   └── auth.test.ts           # Authentication integration tests
└── unit/           # Unit tests
    └── services/   # Unit tests for services
        └── auth.service.test.ts # Unit tests for auth service

scripts/            # Helper scripts for migrations and deployments
├── migration/      # Database migration scripts
└── deployment/     # Deployment automation scripts

docs/               # Documentation for the project
├── api/            # API documentation
└── setup/          # Setup and configuration documentation
```

## 2. Initial Setup

1. First, initialize your TypeScript project:

    ```bash
    mkdir typescript-backend 
    cd typescript-backend 
    npm init -y 
    npm install typescript @types/node ts-node-dev -D 
    npx tsc --init
    ```

2. Update `tsconfig.json`:

    ```json
    {
      "compilerOptions": {
        /* Language and Environment */
        "target": "ES2022" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
        "lib": [
          "ES2022"
        ] /* Specify a set of bundled library declaration files that describe the target runtime environment. */,

        /* Modules */
        "module": "commonjs" /* Specify what module code is generated. */,
        "rootDir": "./src" /* Specify the root folder within your source files. */,
        "moduleResolution": "node" /* Specify how TypeScript looks up a file from a given module specifier. */,
        "baseUrl": "./" /* Specify the base directory to resolve non-relative module names. */,
        "paths": {
          "@/*": ["src/*"]
        } /* Specify a set of entries that re-map imports to additional lookup locations. */,
        "resolveJsonModule": true /* Enable importing .json files. */,

        /* Emit */
        "outDir": "./dist" /* Specify an output folder for all emitted files. */,

        /* Interop Constraints */
        "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
        "forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,

        /* Type Checking */
        "strict": true /* Enable all strict type-checking options. */,
        "skipLibCheck": true /* Skip type checking all .d.ts files. */
      },
      "include": ["src/**/*"],
      "exclude": ["node_modules", "dist"]
    }
    ```

3. Install core dependencies:

    ```bash
    npm install express dotenv cors helmet compression express-rate-limit mongoose
    npm install @types/express @types/cors -D
    ```

## 3. Configuration Management

1. Create `.env` file:

    ```bash
    # Server
    PORT=3000
    NODE_ENV=development

    # MongoDB
    MONGODB_URI=mongodb://localhost:27017/your_database

    # AWS
    AWS_ACCESS_KEY_ID=your_access_key
    AWS_SECRET_ACCESS_KEY=your_secret_key
    AWS_REGION=your_region

    # Firebase
    FIREBASE_PROJECT_ID=your_project_id
    FIREBASE_PRIVATE_KEY=your_private_key
    FIREBASE_CLIENT_EMAIL=your_client_email

    # Email
    SMTP_HOST=smtp.gmail.com
    SMTP_PORT=587
    SMTP_USER=your_email
    SMTP_PASS=your_password

    # Google AI
    GOOGLE_API_KEY=your_api_key

    # JWT
    JWT_SECRET=your_jwt_secret
    JWT_EXPIRES_IN=24h
    ```

2. Database configuration

    ```typescript
    import mongoose from 'mongoose';
    import { config } from 'dotenv';

    config();

    export const connectDatabase = async (): Promise<void> => {
      try {
        await mongoose.connect(process.env.MONGODB_URI!);
        console.log('Connected to MongoDB');
      } catch (error) {
        console.error('MongoDB connection error:', error);
        process.exit(1);
      }
    };
    ```

## 4. Core Dependencies Setup

1. Set up Express application (`src/app.ts`):

    ```typescript
    import express from 'express';
    import cors from 'cors';
    import helmet from 'helmet';
    import compression from 'compression';
    import rateLimit from 'express-rate-limit';
    import { errorHandler } from './middleware/error.middleware';
    import { authRoutes } from './routes/auth.routes';
    import { userRoutes } from './routes/user.routes';
    import { aiRoutes } from './routes/ai.routes';

    const app = express();

    // Middleware
    app.use(cors());
    app.use(helmet());
    app.use(compression());
    app.use(express.json());
    app.use(express.urlencoded({ extended: true }));

    // Rate limiting
    const limiter = rateLimit({
      windowMs: 15 * 60 * 1000, // 15 minutes
      max: 100 // limit each IP to 100 requests per windowMs
    });
    app.use(limiter);

    // Routes
    app.use('/api/auth', authRoutes);
    app.use('/api/users', userRoutes);
    app.use('/api/ai', aiRoutes);

    // Error handling
    app.use(errorHandler);

    export default app;
    ```

## 5. Database Integration

1. Create a MongoDB model (`src/models/user.model.ts`):

    ```typescript
    import mongoose, { Schema, Document } from 'mongoose';
    import bcrypt from 'bcryptjs';

    export interface IUser extends Document {
      email: string;
      password: string;
      name: string;
      comparePassword(candidatePassword: string): Promise<boolean>;
    }

    const userSchema = new Schema({
      email: {
        type: String,
        required: true,
        unique: true,
        lowercase: true,
        trim: true
      },
      password: {
        type: String,
        required: true,
        minlength: 8
      },
      name: {
        type: String,
        required: true,
        trim: true
      }
    }, {
      timestamps: true
    });

    userSchema.pre('save', async function(next) {
      if (!this.isModified('password')) return next();
      
      const salt = await bcrypt.genSalt(10);
      this.password = await bcrypt.hash(this.password, salt);
      next();
    });

    userSchema.methods.comparePassword = async function(candidatePassword: string): Promise<boolean> {
      return bcrypt.compare(candidatePassword, this.password);
    };

    export const User = mongoose.model<IUser>('User', userSchema);
    ```

## 6. Authentication & Authorization

1. Create JWT authentication middleware (`src/middleware/auth.middleware.ts`):

    ```typescript
    import { Request, Response, NextFunction } from 'express';
    import jwt from 'jsonwebtoken';
    import { User } from '../models/user.model';

    export const protect = async (req: Request, res: Response, next: NextFunction) => {
      try {
        const token = req.headers.authorization?.replace('Bearer ', '');

        if (!token) {
          return res.status(401).json({ message: 'Not authorized' });
        }

        const decoded = jwt.verify(token, process.env.JWT_SECRET!) as { id: string };
        const user = await User.findById(decoded.id);

        if (!user) {
          return res.status(401).json({ message: 'User not found' });
        }

        req.user = user;
        next();
      } catch (error) {
        res.status(401).json({ message: 'Not authorized' });
      }
    };
    ```

## 7. Cloud Services Integration

1. AWS S3 integration (`src/services/aws.service.ts`):

    ```typescript
    import { S3 } from 'aws-sdk';
    import { config } from '../config/aws.config';

    export class AwsService {
      private s3: S3;

      constructor() {
        this.s3 = new S3({
          accessKeyId: process.env.AWS_ACCESS_KEY_ID,
          secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
          region: process.env.AWS_REGION
        });
      }

      async uploadFile(file: Buffer, key: string): Promise<string> {
        const params = {
          Bucket: process.env.AWS_BUCKET_NAME!,
          Key: key,
          Body: file,
          ACL: 'public-read'
        };

        const result = await this.s3.upload(params).promise();
        return result.Location;
      }
    }
    ```

## 8. AI Integration

1. Gemini integration using LangChain (`src/services/ai.service.ts`):

    ```typescript
    import { ChatGoogleGenerativeAI } from "@langchain/google-genai";
    import { PromptTemplate } from "@langchain/core/prompts";

    export class AiService {
      private model: ChatGoogleGenerativeAI;

      constructor() {
        this.model = new ChatGoogleGenerativeAI({
          apiKey: process.env.GOOGLE_API_KEY!,
          modelName: "gemini-pro"
        });
      }

      async generateResponse(prompt: string): Promise<string> {
        const template = PromptTemplate.fromTemplate(
          "Answer the following question: {question}"
        );
        
        const formattedPrompt = await template.format({
          question: prompt
        });

        const response = await this.model.invoke(formattedPrompt);
        return response.content;
      }
    }
    ```

## 9. Real-time Communication

1. Socket.IO integration (`src/services/socket.service.ts`):

    ```typescript
    import { Server } from 'socket.io';
    import { Server as HttpServer } from 'http';
    import { verify } from 'jsonwebtoken';

    export class SocketService {
      private io: Server;

      constructor(server: HttpServer) {
        this.io = new Server(server, {
          cors: {
            origin: process.env.CLIENT_URL,
            methods: ['GET', 'POST']
          }
        });

        this.io.use((socket, next) => {
          const token = socket.handshake.auth.token;
          
          try {
            const decoded = verify(token, process.env.JWT_SECRET!);
            socket.data.user = decoded;
            next();
          } catch (err) {
            next(new Error('Authentication error'));
          }
        });

        this.setupEventHandlers();
      }

      private setupEventHandlers(): void {
        this.io.on('connection', (socket) => {
          console.log(`User connected: ${socket.id}`);

          socket.on('join-room', (roomId: string) => {
            socket.join(roomId);
          });

          socket.on('message', (data: { room: string; message: string }) => {
            this.io.to(data.room).emit('message', {
              user: socket.data.user,
              message: data.message,
              timestamp: new Date()
            });
          });

          socket.on('disconnect', () => {
            console.log(`User disconnected: ${socket.id}`);
          });
        });
      }
    }
    ```

## 10. Email Services

1. Nodemailer setup (`src/services/mail.service.ts`):

    ```typescript
    import nodemailer from 'nodemailer';
    import { config } from '../config/mail.config';

    export class MailService {
      private transporter: nodemailer.Transporter;

      constructor() {
        this.transporter = nodemailer.createTransport({
          host: process.env.SMTP_HOST,
          port: parseInt(process.env.SMTP_PORT!),
          secure: false,
          auth: {
            user: process.env.SMTP_USER,
            pass: process.env.SMTP_PASS
          }
        });
      }

      async sendMail(to: string, subject: string, html: string): Promise<void> {
        await this.transporter.sendMail({
          from: process.env.SMTP_USER,
          to,
          subject,
          html
        });
      }

      async sendWelcomeEmail(user: { email: string; name: string }): Promise<void> {
        const html = `
          <h1>Welcome to our platform, ${user.name}!</h1>
          <p>We're excited to have you on board.</p>
        `;

        await this.sendMail(user.email, 'Welcome!', html);
      }
    }
    ```

## 11. Testing & Quality Assurance

1. Install testing dependencies:

    ```bash
    npm install jest @types/jest ts-jest supertest @types/supertest -D
    ```

2. Create `jest.config.js`:

    ```javascript
    module.exports = {
      preset: 'ts-jest',
      testEnvironment: 'node',
      roots: ['<rootDir>/src'],
      testMatch: ['**/*.test.ts'],
      moduleNameMapper: {
        '^@/(.*)$': '<rootDir>/src/$1'
      }
    };
    ```

3. Example test (`tests/integration/auth.test.ts`):

    ```typescript
    import request from 'supertest';
    import app from '../../src/app';
    import { connectDatabase } from '../../src/config/database.config';
    import { User } from '../../src/models/user.model';

    describe('Auth endpoints', () => {
      beforeAll(async () => {
        await connectDatabase();
      });

      beforeEach(async () => {
        await User.deleteMany({});
      });

      describe('POST /api/auth/register', () => {
        it('should register a new user', async () => {
          const res = await request(app)
            .post('/api/auth/register')
            .send({
              email: 'test@example.com',
              password: 'password123',
              name: 'Test User'
            });

          expect(res.status).toBe(201);
          expect(res.body).toHaveProperty('token');
        });
      });
    });
    ```

## 12. Deployment & CI/CD

1. Create `Dockerfile`:

    ```dockerfile
    FROM node:18-alpine

    WORKDIR /app

    COPY package*.json ./
    RUN npm install

    COPY . .
    RUN npm run build

    EXPOSE 3000

    CMD ["npm", "start"]
    ```

2. Create `.github/workflows/ci.yml` for GitHub Actions:

    ```yaml
    name: CI/CD

    on:
      push:
        branches: [ main ]
      pull_request:
        branches: [ main ]

    jobs:
      build:
        runs-on: ubuntu-latest

        steps:
        - uses: actions/checkout@v2
        
        - name: Use Node.js
          uses: actions/setup-node@v2
          with:
            node-version: '18'
            
        - name: Install dependencies
          run: npm ci
          
        - name: Run tests
          run: npm test
          
        - name: Build
          run: npm run build

        - name: Deploy to AWS ECR
          uses: aws-actions/amazon-ecr-login@v1
          env:
            AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
            AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            AWS_REGION: ${{ secrets.AWS_REGION }}
          
        - name: Build and push Docker image
          run: |
            docker build -t your-app .
            docker tag your-app:latest ${{ secrets.ECR_REGISTRY }}/your-app:latest
            docker push ${{ secrets.ECR_REGISTRY }}/your-app:latest
    ```

## 13. Error Handling

1. Create a centralized error handling system (`src/utils/errors.ts`):

    ```typescript
    export class AppError extends Error {
      constructor(
        public statusCode: number,
        public message: string,
        public isOperational = true
      ) {
        super(message);
        Object.setPrototypeOf(this, AppError.prototype);
      }
    }

    export class ValidationError extends AppError {
      constructor(message: string) {
        super(400, message);
      }
    }

    export class AuthenticationError extends AppError {
      constructor(message: string = 'Authentication failed') {
        super(401, message);
      }
    }

    export class AuthorizationError extends AppError {
      constructor(message: string = 'Not authorized') {
        super(403, message);
      }
    }
    ```

2. Error middleware (`src/middleware/error.middleware.ts`):

    ```typescript
    import { Request, Response, NextFunction } from 'express';
    import { AppError } from '../utils/errors';
    import { logger } from '../utils/logger';

    export const errorHandler = (
      err: Error,
      req: Request,
      res: Response,
      next: NextFunction
    ) => {
      if (err instanceof AppError) {
        return res.status(err.statusCode).json({
          status: 'error',
          message: err.message,
          ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
        });
      }

      logger.error(err);

      res.status(500).json({
        status: 'error',
        message: 'Internal server error'
      });
    };
    ```

My Snippet: [Logger Setup - Using Winston and Morgan](Logger%20Setup%20-%20Using%20Winston%20and%20Morgan.md)

## 14. Logging System

1. Create a logging utility (`src/utils/logger.ts`):

    ```typescript
    import winston from 'winston';

    const logger = winston.createLogger({
      level: 'info',
      format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.json()
      ),
      transports: [
        new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
        new winston.transports.File({ filename: 'logs/combined.log' })
      ]
    });

    if (process.env.NODE_ENV !== 'production') {
      logger.add(new winston.transports.Console({
        format: winston.format.simple()
      }));
    }

    export { logger };
    ```

## 15. API Documentation

1. Install Swagger for API documentation:

    ```bash
    npm install swagger-jsdoc swagger-ui-express
    npm install @types/swagger-jsdoc @types/swagger-ui-express -D
    ```

2. Create Swagger configuration (`src/config/swagger.config.ts`):

    ```typescript
    import swaggerJsdoc from 'swagger-jsdoc';

    const options = {
      definition: {
        openapi: '3.0.0',
        info: {
          title: 'TypeScript Backend API',
          version: '1.0.0',
          description: 'API documentation for TypeScript Backend'
        },
        servers: [
          {
            url: 'http://localhost:3000',
            description: 'Development server'
          }
        ]
      },
      apis: ['./src/routes/*.ts']
    };

    export const swaggerSpec = swaggerJsdoc(options);
    ```

    Example route documentation (`src/routes/auth.routes.ts`):

    ```typescript
        /**
        * @swagger
        * /api/auth/register:
        *   post:
        *     summary: Register a new user
        *     tags: [Auth]
        *     requestBody:
        *       required: true
        *       content:
        *         application/json:
        *           schema:
        *             type: object
        *             required:
        *               - email
        *               - password
        *               - name
        *             properties:
        *               email:
        *                 type: string
        *               password:
        *                 type: string
        *               name:
        *                 type: string
        *     responses:
        *       201:
        *         description: User registered successfully
        *       400:
        *         description: Invalid input
        */
        router.post('/register', registerUser);
        ```

    ## 16. Input Validation

    1. Install validation library:
    ```bash
    npm install zod
    ```

    Create validation schemas (`src/validators/auth.validator.ts`):

    ```typescript
    import { z } from 'zod';

    export const registerSchema = z.object({
      email: z.string().email().nonempty(),
      password: z.string().min(8),
      name: z.string().nonempty(),
    });

    export const loginSchema = z.object({
      email: z.string().email().nonempty(),
      password: z.string().nonempty(),
    });
    ```

3. Validation middleware (`src/middleware/validation.middleware.ts`):

    ```typescript
    import { Request, Response, NextFunction } from 'express';
    import { ZodError } from 'zod';
    import { registerSchema, loginSchema } from '../validators/auth.validator';
    import { ValidationError } from '../utils/errors';

    export const validate = (schema: any) => {
      return (req: Request, res: Response, next: NextFunction) => {
        try {
          schema.parse(req.body);
          next();
        } catch (error) {
          if (error instanceof ZodError) {
            throw new ValidationError(error.errors[0].message);
          }
          next(error);
        }
      };
    };
    ```

## 17. Rate Limiting and Security

1. Create rate limiting middleware (`src/middleware/rateLimiter.middleware.ts`):

    ```typescript
    import rateLimit from 'express-rate-limit';
    import RedisStore from 'rate-limit-redis';
    import Redis from 'ioredis';

    const redis = new Redis({
      host: process.env.REDIS_HOST,
      port: parseInt(process.env.REDIS_PORT!),
      password: process.env.REDIS_PASSWORD
    });

    export const createRateLimiter = (
      windowMs: number = 15 * 60 * 1000,
      max: number = 100
    ) => {
      return rateLimit({
        store: new RedisStore({
          client: redis,
          prefix: 'rate-limit:'
        }),
        windowMs,
        max,
        message: 'Too many requests from this IP, please try again later.'
      });
    };
    ```

## 18. Monitoring and Performance

1. Install monitoring tools:

    ```bash
    npm install prometheus-client express-prometheus-middleware
    ```

2. Setup monitoring (`src/config/monitoring.config.ts`):

    ```typescript
    import prometheusMiddleware from 'express-prometheus-middleware';

    export const metricsMiddleware = prometheusMiddleware({
      metricsPath: '/metrics',
      collectDefaultMetrics: true,
      requestDurationBuckets: [0.1, 0.5, 1, 1.5, 2, 3, 5],
      requestLengthBuckets: [512, 1024, 5120, 10240, 51200, 102400],
      responseLengthBuckets: [512, 1024, 5120, 10240, 51200, 102400]
    });
    ```

## 19. Script Management

1. Create npm scripts in `package.json`:

    ```json
    {
      "scripts": {
        "start": "node dist/server.js",
        "dev": "ts-node-dev --respawn --transpile-only src/server.ts",
        "build": "tsc",
        "test": "jest",
        "test:watch": "jest --watch",
        "lint": "eslint . --ext .ts",
        "lint:fix": "eslint . --ext .ts --fix",
        "migrate": "ts-node scripts/migration/run.ts",
        "seed": "ts-node scripts/migration/seed.ts",
        "doc": "typedoc --out docs src"
      }
    }
    ```
