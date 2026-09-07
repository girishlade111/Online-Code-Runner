# Online Code Runner

<p align="center">
  <img src="https://img.shields.io/badge/React-18.2.0-61DAFB?style=for-the-badge&logo=react&logoColor=white" alt="React" />
  <img src="https://img.shields.io/badge/Node.js-Express-339933?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/MongoDB-6.8.1-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB" />
  <img src="https://img.shields.io/badge/Redis-Bull-FF4438?style=for-the-badge&logo=redis&logoColor=white" alt="Redis" />
  <img src="https://img.shields.io/badge/Monaco-Editor-007ACC?style=for-the-badge&logo=monaco&logoColor=white" alt="Monaco Editor" />
  <img src="https://img.shields.io/badge/License-ISC-blue?style=for-the-badge" alt="License" />
</p>

<p align="center">
  A full-stack online code editor and runner supporting <strong>C++</strong> and <strong>Python</strong> with real-time execution, syntax highlighting, and theme switching.
</p>

---

## 📸 Screenshots

### Main Editor Interface
![Main Interface](https://github.com/jigyansunanda/Online-Code-Runner/blob/master/media/app-screengrab.png)

### Language Selection
![Language Selection](https://github.com/jigyansunanda/Online-Code-Runner/blob/master/media/language-selection.gif)

### Code Execution Status
![Execution Status](https://github.com/jigyansunanda/Online-Code-Runner/blob/master/media/execution-status.gif)

### Theme Switching
![Theme Switch](https://github.com/jigyansunanda/Online-Code-Runner/blob/master/media/theme-switch.gif)

---

## ✨ Features

- **Multi-language Support** — Run C++ and Python code
- **Real-time Execution** — Powered by Bull queue and Redis for async job processing
- **Monaco Editor** — VS Code-like editing experience with syntax highlighting
- **Theme Switching** — Light/Dark mode support
- **Execution History** — Track previous code runs with timestamps
- **Responsive Design** — Works on desktop and mobile
- **Secure Sandbox** — Isolated code execution environment

---

## 🏗️ Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Client        │     │   Server        │     │   Worker        │
│   (React)       │────▶│   (Express)     │────▶│   (Bull/Redis)  │
│   Port: 3000    │     │   Port: 5000    │     │   (Execution)   │
└─────────────────┘     └────────┬────────┘     └─────────────────┘
                                 │
                                 ▼
                        ┌─────────────────┐
                        │   MongoDB       │
                        │   (Storage)     │
                        └─────────────────┘
```

### Tech Stack

| Layer | Technology | Version |
|-------|------------|---------|
| **Frontend** | React | 18.2.0 |
| **Editor** | Monaco Editor | 0.34.1 |
| **State Management** | React Hooks | Built-in |
| **HTTP Client** | Axios | 1.2.1 |
| **Backend** | Node.js + Express | 4.18.2 |
| **Queue** | Bull (Redis) | 4.10.2 |
| **Database** | MongoDB + Mongoose | 6.8.1 |
| **Process Manager** | Nodemon (dev) | 2.0.20 |

---

## 🚀 Quick Start

### Prerequisites

Ensure you have the following installed:

| Tool | Version | Purpose |
|------|---------|---------|
| **Node.js** | ≥ 16.x | JavaScript runtime |
| **npm** | ≥ 8.x | Package manager |
| **MongoDB** | ≥ 5.x | Database |
| **Redis** | ≥ 6.x | Message queue |
| **GCC** | ≥ 9.x | C++ compiler |
| **Python** | ≥ 3.8 | Python interpreter |

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/jigyansunanda/Online-Code-Runner.git
   cd Online-Code-Runner
   ```

2. **Install backend dependencies**
   ```bash
   cd backend
   npm install
   ```

3. **Install frontend dependencies**
   ```bash
   cd ../client
   npm install
   ```

4. **Configure environment variables**
   
   Create `.env` file in the `backend` directory:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/code-runner
   REDIS_URL=redis://localhost:6379
   NODE_ENV=development
   ```

5. **Start MongoDB and Redis**
   ```bash
   # MongoDB
   mongod
   
   # Redis (in separate terminal)
   redis-server
   ```

---

## 🏃 Running the Application

### Development Mode

**Terminal 1 - Backend:**
```bash
cd backend
npm run dev
# Server runs on http://localhost:5000
```

**Terminal 2 - Frontend:**
```bash
cd client
npm start
# Client runs on http://localhost:3000
```

### Production Mode

**Build frontend:**
```bash
cd client
npm run build
```

**Start backend:**
```bash
cd backend
npm start
# Serves both API and static frontend on http://localhost:5000
```

---

## 📁 Project Structure

```
Online-Code-Runner/
├── backend/
│   ├── index.js              # Express server entry point
│   ├── package.json          # Backend dependencies
│   ├── routes/
│   │   └── api.js            # API route handlers
│   ├── controllers/
│   │   └── executionController.js  # Code execution logic
│   ├── models/
│   │   └── Execution.js      # MongoDB schema
│   ├── queue/
│   │   └── executionQueue.js # Bull queue configuration
│   └── workers/
│       └── executionWorker.js # Code execution worker
│
├── client/
│   ├── public/
│   │   └── index.html        # HTML template
│   ├── src/
│   │   ├── components/
│   │   │   ├── Editor.jsx    # Monaco editor component
│   │   │   ├── Header.jsx    # Navigation header
│   │   │   ├── LanguageSelector.jsx  # Language dropdown
│   │   │   ├── ThemeToggle.jsx       # Theme switcher
│   │   │   └── History.jsx   # Execution history panel
│   │   ├── App.jsx           # Main app component
│   │   ├── App.css           # Global styles
│   │   └── index.js          # React entry point
│   ├── package.json          # Frontend dependencies
│   └── tailwind.config.js    # Tailwind configuration
│
├── media/                    # Screenshots and GIFs
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🔌 API Endpoints

### Execute Code
```http
POST /api/execute
Content-Type: application/json

{
  "code": "print('Hello, World!')",
  "language": "python",
  "input": ""
}
```

**Response:**
```json
{
  "success": true,
  "output": "Hello, World!\n",
  "executionTime": 45,
  "memoryUsed": 12.5,
  "executionId": "uuid-v4"
}
```

### Get Execution History
```http
GET /api/history
```

### Get Single Execution
```http
GET /api/execution/:id
```

---

## ⚙️ Configuration

### Environment Variables (Backend)

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `PORT` | No | 5000 | Server port |
| `MONGODB_URI` | Yes | - | MongoDB connection string |
| `REDIS_URL` | Yes | - | Redis connection string |
| `NODE_ENV` | No | development | Environment mode |
| `EXECUTION_TIMEOUT` | No | 5000 | Max execution time (ms) |
| `MEMORY_LIMIT` | No | 128 | Memory limit (MB) |

### Supported Languages

| Language | Compiler/Interpreter | File Extension |
|----------|---------------------|----------------|
| C++ | GCC (g++) | `.cpp` |
| Python | Python 3 | `.py` |

---

## 🛠️ Development

### Adding a New Language

1. **Add compiler/interpreter check** in `backend/workers/executionWorker.js`
2. **Update language selector** in `client/src/components/LanguageSelector.jsx`
3. **Add Monaco language support** in `client/src/components/Editor.jsx`
4. **Test execution** with sample code

### Running Tests

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd client
npm test
```

### Code Style

- **Backend**: ESLint with Airbnb config
- **Frontend**: React-scripts ESLint + Prettier

---

## 📦 Deployment

### Docker (Recommended)

```dockerfile
# docker-compose.yml
version: '3.8'
services:
  mongodb:
    image: mongo:6
    ports:
      - "27017:27017"
    volumes:
      - mongodb-data:/data/db
  
  redis:
    image: redis:7
    ports:
      - "6379:6379"
  
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - MONGODB_URI=mongodb://mongodb:27017/code-runner
      - REDIS_URL=redis://redis:6379
    depends_on:
      - mongodb
      - redis
  
  frontend:
    build: ./client
    ports:
      - "3000:3000"
    depends_on:
      - backend

volumes:
  mongodb-data:
```

```bash
docker-compose up -d
```

### Manual Deployment

1. **Build frontend:** `cd client && npm run build`
2. **Configure production env** in backend
3. **Use PM2:** `pm2 start backend/index.js --name code-runner`
4. **Setup Nginx** reverse proxy for ports 3000/5000
5. **Configure SSL** with Let's Encrypt

---

## 🔒 Security Considerations

- **Code Isolation** — Each execution runs in a separate process
- **Resource Limits** — CPU time and memory constraints enforced
- **Input Sanitization** — All user input validated and sanitized
- **Rate Limiting** — API endpoints protected against abuse
- **No Persistent Storage** — Temporary files cleaned after execution

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

### Code of Conduct

Please read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) before contributing.

---

## 📄 License

This project is licensed under the **ISC License** - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Jigyansu Nanda**
- GitHub: [@jigyansunanda](https://github.com/jigyansunanda)
- Email: jigyansunanda@example.com

---

## 🙏 Acknowledgments

- [Monaco Editor](https://microsoft.github.io/monaco-editor/) — Code editor component
- [Bull](https://github.com/OptimalBits/bull) — Redis-based queue
- [React](https://reactjs.org/) — Frontend framework
- [Express](https://expressjs.com/) — Backend framework
- [MongoDB](https://www.mongodb.com/) — Database
- [Redis](https://redis.io/) — In-memory data store

---

## 📞 Support

If you encounter any issues or have questions:

- 🐛 [Report a Bug](https://github.com/jigyansunanda/Online-Code-Runner/issues)
- 💡 [Request a Feature](https://github.com/jigyansunanda/Online-Code-Runner/issues/new)
- 📧 Email: jigyansunanda@example.com

---

<p align="center">
  Made with ❤️ by <a href="https://github.com/jigyansunanda">Jigyansu Nanda</a>
</p>