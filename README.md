# Node Feedback App (Data Volume Example)

A simple Node.js and Express application designed to demonstrate file handling, form processing, and Docker volume persistence.

## 🚀 Overview

This application serves a simple HTML form where users can submit feedback (a title and a text body). When feedback is submitted, the app:
1. Writes the content to a `.txt` file inside a `temp/` directory.
2. Checks if a feedback file with the same title already exists.
3. If it does not exist, it safely copies the file to a persistent `feedback/` directory and removes the temporary file. 

*Note: The app uses a copy-and-delete approach (`fs.copyFile` + `fs.unlink`) instead of `fs.rename` to prevent Docker `EXDEV` (cross-device link) errors when moving files between temporary containers and persistent mounted volumes.*

## 📂 Project Structure

- **`server.js`**: The main Express application logic and route handlers.
- **`Dockerfile`**: Docker configuration using `node:22-alpine` to containerize the app.
- **`pages/`**: Contains the HTML views (`feedback.html`, `exists.html`).
- **`public/`**: Static assets served by Express.
- **`temp/`**: Temporary directory for processing incoming text files.
- **`feedback/`**: The target directory for saved feedback files (ideal for mapping to a Docker Volume).

## 🛠️ Getting Started

### Running Locally (Without Docker)

1. **Install dependencies:**
   ```bash
   npm install
   ```
2. **Start the server:**
   ```bash
   node server.js
   ```
   *(Note: The server is configured to listen on port 80. You may need `sudo` privileges on some OS, or you can change the port in `server.js` to `3000` for local testing).*

### Running with Docker

1. **Build the image:**
   ```bash
   docker build -t node-feedback-app .
   ```
2. **Run the container (with a volume for persistent feedback):**
   ```bash
   docker run -d -p 8080:80 -v feedback-data:/app/feedback --name feedback-app node-feedback-app
   ```
3. Access the application in your browser at `http://localhost:8080`.

## 📚 Dependencies
- [Express](https://expressjs.com/) (`^4.17.1`): Web framework for Node.js
- [body-parser](https://www.npmjs.com/package/body-parser) (`^1.19.0`): Middleware to parse incoming request bodies
