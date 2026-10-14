# Jenkins Flask Web Application

## 💡 Introduction

This project is a simple web application built with **Python** and **Flask**. It starts a web server (port **5000**) and returns a text message when a user opens the home page. A second route, `/health`, returns a JSON status response that can be used for health checks.

The home page message confirms a successful deployment through Jenkins and mentions that a webhook was added. The project is intended for practicing Python application deployment and Jenkins-based CI/CD workflows.

## 🎯 Objectives

- Create a web server using Python and Flask.
- Handle HTTP requests with Flask routes.
- Display responses on the `/` and `/health` pages.
- Manage application dependencies using `requirements.txt`.
- Define a build and deploy pipeline using a `Jenkinsfile`.
- Practice Jenkins integration and webhook-triggered automation.

## 🏗️ Architecture

```
User/Browser → HTTP GET / or /health → AWS EC2 (Linux) → Flask (app.py, port 5000) → Response
```

With the CI/CD setup:

```
GitHub push → Webhook → Jenkins → Build & deploy to EC2 → Restart app
```

### Components

| Component | Role |
|-----------|------|
| **Frontend – Web Browser** | Sends requests and displays the response |
| **Backend – Python** | Runs the application code |
| **Web Framework – Flask** | Routes HTTP requests and returns responses |
| **Configuration – requirements.txt** | Lists the Python dependencies |
| **CI/CD – Jenkins and Webhook** | Triggers the pipeline automatically on every GitHub push |
| **Pipeline – Jenkinsfile** | Defines the checkout, install and deploy stages |

## 📁 Project Structure

```
.
├── app.py             # Flask application
├── requirements.txt   # Python dependencies
├── Jenkinsfile        # Jenkins pipeline definition
└── README.md
```

## 📥 Input

The application receives an HTTP GET request from the user's browser.

| Input | Description |
|-------|-------------|
| URL | `http://localhost:5000/` or `http://localhost:5000/health` |
| HTTP Method | `GET` |
| Port | `5000` |
| Routes | `/` and `/health` |
| Request Body | Not required |

### Input Process

1. The user opens a web browser.
2. The user accesses `http://<server-ip>:5000/` or `/health`.
3. The browser sends an HTTP GET request.
4. Flask matches the request to the correct route function.
5. The function prepares the response.

## 📤 Output

| Route | Function | Response |
|-------|----------|----------|
| `/` | `home()` | `successfully deployed python application through jenkins!!!!!!!!!, added webhook` |
| `/health` | `health()` | `{"status": "ok"}` |

### Output Process

1. Flask matches the incoming request to a route.
2. The route function returns the configured response.
3. The browser displays the message.
4. The terminal displays the Flask startup logs, including the address the server is running on.

## 📦 Dependencies

Listed in `requirements.txt`:

| Package | Version |
|---------|---------|
| Flask | >= 3.0 |

## 🔁 Background – Jenkins Pipeline

**Jenkins** is an open-source automation server used to build, test and deploy applications. The `Jenkinsfile` in this project defines a declarative pipeline.

### Pipeline Stages

| Stage | What it does |
|-------|--------------|
| **Checkout** | Pulls the latest code from the repository |
| **Install dependencies** | Creates a virtual environment and installs `requirements.txt` |
| **Deploy** | Stops the previous instance and starts the new one in the background |

### Trigger

The pipeline uses `githubPush()`, so a GitHub webhook starts a build every time code is pushed.

## 🛠️ Deployment Guide

### 1. Launch an EC2 instance on AWS

Use a Linux AMI (e.g. Ubuntu or Amazon Linux). In the security group, allow inbound traffic on:

- **22** (SSH)
- **8080** (Jenkins)
- **5000** (Flask app)

### 2. Push the code to GitHub

Commit `app.py`, `requirements.txt`, `Jenkinsfile` and `README.md` to a GitHub repository.

### 3. Install the required packages

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv git -y
```

Then install Jenkins following the official instructions for your OS.

### 4. Create the Jenkins pipeline job

1. In Jenkins, create a new **Pipeline** job.
2. Under **Pipeline**, choose *Pipeline script from SCM* and select **Git**.
3. Enter your repository URL and branch.
4. Enable **GitHub hook trigger for GITScm polling**.

### 5. Add the GitHub webhook

In your GitHub repository, go to **Settings → Webhooks → Add webhook**:

- **Payload URL:** `http://<jenkins-server-ip>:8080/github-webhook/`
- **Content type:** `application/json`
- **Event:** Just the push event

### 6. Push a commit

Jenkins picks up the push and runs the pipeline automatically.

### 7. Verify the deployment

Open `http://<server-public-ip>:5000` in your browser. You should see:

```
successfully deployed python application through jenkins!!!!!!!!!, added webhook
```

## 💻 Run Locally

```bash
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Open http://localhost:5000 in your browser.

## 🧯 Troubleshooting

| Problem | Fix |
|---------|-----|
| Page doesn't load | Check that port 5000 is open in the security group and the app is bound to `0.0.0.0` |
| Webhook not triggering | Confirm the payload URL ends with `/github-webhook/` and GitHub can reach Jenkins |
| App stops after the build ends | Keep the `JENKINS_NODE_COOKIE=dontKillMe` setting in the `Deploy` stage |
| Need logs | Check `app.log` in the Jenkins workspace |

## ✅ Conclusion

This project demonstrates the basic working of a web application using Python and Flask, deployed automatically with Jenkins. It gives practical experience with HTTP requests, routing, dependency management, CI/CD pipelines and webhook integration.

## 🧰 Technologies Used

- **Language:** Python
- **Framework:** Flask
- **Package Management:** pip (`requirements.txt`)
- **Default Application Port:** 5000
- **CI/CD Tool:** Jenkins
- **Trigger:** GitHub Webhook
- **Hosting:** AWS EC2

## 👤 Author

**Your Name**
