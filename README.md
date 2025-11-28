Week 3: Advanced Security and Final Reporting
GitHub Repo: https://github.com/MaryamAmjad-tech/ServerSecure.git

This week covers advanced security practices and final project reporting. Key activities include basic penetration testing, implementing structured logging with Winston, ensuring secure defaults, and compiling a comprehensive security checklist and report.

Overview
Tech stack: Node.js, Express (HTTPS setup optional per environment)
Security practices:
Basic penetration testing (Nmap and manual browser tests)
Centralized logging with Winston (console and file transports)
Robust input validation
Transport security (HTTPS)
Secure password storage (hashing)
Security headers (Helmet)
Deliverables:
Running an HTTPS server
Demonstrating login requests over HTTPS
Nmap scan outputs
Project tree and file structure
A final security checklist and reporting
Repository Structure
ServerSecure/ (root folder)
app.js (or server.js)
package.json
package-lock.json
routes/
auth.js (authentication endpoints)
config/
https.js (if HTTPS is configured)
logs/ ( Winston log outputs if configured to write to file)
tests/ (optional: penetration test notes or scripts)
README.md (this file)
Adjust file names to match your actual project structure if different.

Getting Started
Prerequisites:

Node.js (LTS version recommended)
npm or yarn
Optional for HTTPS in development:

Self-signed certificate (certificate.pem and key.pem) if you enable HTTPS locally
Install:

Clone the repo
git clone https://github.com/MaryamAmjad-tech/ServerSecure.git
Navigate to the project
cd ServerSecure
Install dependencies
npm install
Run (HTTP or HTTPS depending on your setup):

For HTTP (default local setup)

node app.js
Server usually runs on port 3000 (adjust in code if needed)
For HTTPS (if configured)

Ensure certificate and key files exist or are generated
Start server with HTTPS options (e.g., using https.createServer)
Access via https://localhost:3000 (or configured port)
Note: If you plan to demonstrate HTTPS locally, you may need to configure your app to conditionally enable HTTPS in development versus production.

Endpoints (Example)
POST /auth/register

Body: { "email": "...", "password": "..." }
Validates input, hashes password, stores user (in memory or DB depending on your setup)
POST /auth/login

Body: { "email": "...", "password": "..." }
Validates credentials, returns a signed JWT or session
GET /secure (example protected route)

Requires valid JWT or session
Adapt endpoints to match your actual implementation.

Basic Penetration Testing
Nmap: Run a quick scan to identify open ports and services
Example: nmap -sV -T4 -p- localhost -oN nmap_week3.txt
Manual browser tests:
Check for open redirects, insecure endpoints, insecure cookies, improper authentication flows, and error message exposure
Verify that sensitive data is not echoed in responses
Security notes:

Do not rely on in-code comments for security; ensure proper validation, sanitization, and error handling.
For production, run scans against deployed endpoints and review results.
Logging with Winston
Winston is used to log to console and to files.

Typical setup (example):

const { createLogger, transports, format } = require('winston');
const logger = createLogger({
level: 'info',
format: format.combine(format.timestamp(), format.json()),
transports: [
new transports.Console(),
new transports.File({ filename: 'logs/combined.log' })
]
});
Use logger.info(), logger.warn(), logger.error() across the app to standardize log output.

Security Checklist
Input Validation: Validate and sanitize all inputs (server-side).
HTTPS: Serve over HTTPS in production; use strong TLS configuration.
Hashed Passwords: Store passwords using bcrypt or similar with proper salt rounds.
Security Headers: Use Helmet or equivalent to set secure headers.
Logging: Centralized logging with Winston (console + file logging).
Authentication: Use signed tokens (JWT) or sessions; ensure token/session security (httpOnly cookies, short lifetimes, rotation).
Dependency Management: Regularly update dependencies and run vulnerability scans.
Error Handling: Do not leak sensitive error details to clients.
Rate Limiting: Implement rate limiting to prevent brute-force and DoS.
Secrets Management: Use environment variables or secret management service; avoid hardcoding secrets.
Testing and Validation
Functional tests:
Register a new user and ensure password is hashed
Login with correct/incorrect credentials and observe responses
Security tests:
Validate input validation and sanitization on registration
Verify that passwordless or weak password attempts are handled securely
Confirm that login endpoints are protected against basic enumeration (if applicable)
Logging tests:
Ensure login attempts and errors are logged to console and files
Troubleshooting
If the server fails to start:
Check Node.js and npm versions
Ensure dependencies are installed (npm install)
Check for port conflicts; adjust the port in app.js if needed
HTTPS issues:
Ensure certificate/key paths are correct
Browser may show warnings for self-signed certs; you can proceed with exceptions for development
Future Enhancements
Replace in-memory user storage with a persistent database
Implement protected routes requiring valid JWT
Add automated security tests (unit/integration)
Add environment-specific configurations (development, staging, production)
Integrate a CI pipeline to run vulnerability scans and tests
