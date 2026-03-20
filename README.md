# 🚀 Vue + Node CI/CD Project (Jenkins + Docker + GitHub)

## 📌 Overview

This project demonstrates a full CI/CD pipeline using:

* Vue 3 (Frontend)
* Node.js + Express (Backend)
* Docker & Docker Compose
* Jenkins (CI/CD)
* GitHub (Source Control + Webhook)

---

# 🧱 1. Project Setup

## Folder Structure

```
vue-node-cicd/
 ├── client/
 ├── server/
 ├── docker-compose.yml
 ├── Jenkinsfile
```

---

# ⚙️ 2. Backend Setup (Node.js)

## Steps

1. Create folder:

```
mkdir server
cd server
npm init -y
npm install express cors
```

2. Create `index.js`

```
const express = require('express');
const cors = require('cors');

const app = express();
app.use(cors());

app.get('/api/hello', (req, res) => {
  res.json({ message: 'Hello from Node 🚀' });
});

app.listen(5000, () => console.log('Server running on 5000'));
```

---

# 🎨 3. Frontend Setup (Vue)

## Steps

```
npm create vite@latest client
cd client
npm install
```

## Update `App.vue`

```
onMounted(async () => {
  const res = await fetch('http://localhost:5000/api/hello')
  const data = await res.json()
  message.value = data.message
})
```

---

# 🐳 4. Docker Setup

## server/Dockerfile

```
FROM node:22
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["node", "index.js"]
```

## client/Dockerfile

```
FROM node:22
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
RUN npm install -g serve
EXPOSE 3000
CMD ["serve", "-s", "dist", "-l", "3000"]
```

## docker-compose.yml

```
version: "3"

services:
  server:
    build: ./server
    ports:
      - "5000:5000"

  client:
    build: ./client
    ports:
      - "3000:3000"
    depends_on:
      - server
```

---

# ▶️ 5. Run Project with Docker

```
docker compose up --build
```

Access:

* Frontend: [http://localhost:3000](http://localhost:3000)
* Backend: [http://localhost:5000/api/hello](http://localhost:5000/api/hello)

---

# 🤖 6. Jenkins Setup

## Jenkinsfile

```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'docker compose build'
            }
        }
        stage('Run') {
            steps {
                bat 'docker compose up -d'
            }
        }
        stage('Test') {
            steps {
                bat 'curl http://localhost:5000/api/hello'
            }
        }
    }
}
```

---

# 🔗 7. GitHub Integration

## Steps

1. Push code to GitHub
2. Create Jenkins Pipeline from SCM
3. Add Webhook:

```
https://ngrok-url/github-webhook/
```

---

# 🌐 8. Ngrok Setup (for local webhook)

```
ngrok http 8080
```

Use generated URL in GitHub webhook.

---

# ❌ 9. Issues Faced & Solutions

## 1. docker-compose file empty

* Cause: File not saved
* Fix: Recreate and save properly

## 2. Port already in use

* Error: port 5000 already in use
* Fix:

```
docker compose down
```

OR change port

## 3. Frontend not loading

* Cause: serve not binding port
* Fix: add `-l 3000`

## 4. Jenkins 'sh' not working

* Cause: Windows system
* Fix: use `bat`

## 5. docker not recognized in Jenkins

* Cause: PATH issue
* Fix:
* Add Docker to System PATH
* Restart Jenkins

## 6. Webhook not triggering

* Cause: local IP not accessible
* Fix: use ngrok

## 7. Branch error (master vs main)

* Fix: change branch to main

## 8. Jenkinsfile not found

* Fix: place Jenkinsfile in root

---

# 🧠 10. Key Learnings

* Docker containerization
* Jenkins pipeline creation
* GitHub webhook integration
* Debugging CI/CD issues
* Windows vs Linux command differences

---

# 🎯 11. Final CI/CD Flow

```
GitHub Push → Webhook → Jenkins → Docker Build → Run → Test
```



