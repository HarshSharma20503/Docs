# Node Logger Setup using Winston and Morgan

## Install Dependencies

```bash
npm install winston winston-daily-rotate-file morgan
```

Create a logger.js in the utils of the backend source folder

```javascript
const { createLogger, format, transports } = require("winston");
require("winston-daily-rotate-file"); // For rotating logs
const path = require("path");

const { combine, timestamp, json, printf, colorize, errors } = format;
const env = process.env.NODE_ENV || "development";

// Custom formats
const consoleLogFormat = printf(({ level, message, timestamp, stack }) => {
  return `${timestamp} ${level}: ${stack || message}`;
});

const devFormat = combine(
  colorize(),
  timestamp({ format: "YYYY-MM-DD HH:mm:ss" }),
  errors({ stack: true }), // Print stack trace for errors
  consoleLogFormat
);

const prodFormat = combine(
  timestamp(),
  errors({ stack: true }),
  json()
);

// Create logger
const logger = createLogger({
  level: env === "development" ? "debug" : "info", // Verbose in dev
  format: env === "development" ? devFormat : prodFormat,
  transports: [
    new transports.Console(), // Console logging for all environments

    // File transport with daily rotation
    new transports.DailyRotateFile({
      dirname: path.join(path.resolve(), "logs"), // Logs directory
      filename: "%DATE%-app.log",
      datePattern: "YYYY-MM-DD",
      maxFiles: "7d", // Retain logs for 7 days
      level: "info", // Log 'info' level and above
    }),
  ],
});

module.exports = logger;
```

now add the middleware in the app.js

```javascript
const morgan = require("morgan");
const logger = require("./logger"); // Assuming logger is defined in logger.js

const morganFormat = ":method :url :status :response-time ms";

// Setup morgan to log requests using winston
app.use(
  morgan(morganFormat, {
    stream: {
      write: (message) => {
        const logObject = {
          method: message.split(" ")[0],        // HTTP method (GET, POST, etc.)
          url: message.split(" ")[1],           // Request URL
          status: message.split(" ")[2],        // HTTP status code
          responseTime: message.split(" ")[3],  // Response time in milliseconds
        };
        logger.info(JSON.stringify(logObject)); // Log the object using winston
      },
    },
  })
);
```
