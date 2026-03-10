# SonarQube Code Quality Analysis with Jenkins CI/CD (Java Application)

## 📌 Project Overview

This project demonstrates how to **automatically analyze the code quality of a Java application using SonarQube integrated with a Jenkins CI/CD pipeline running on AWS EC2 with Docker**.

The goal of this project is to simulate a **real-world DevOps workflow** where every code change pushed to GitHub is automatically:

* Built using **Maven**
* Analyzed for **code quality, bugs, vulnerabilities, and code smells**
* Evaluated using **SonarQube Quality Gates**

This ensures that **only high-quality code moves forward in the deployment pipeline**.

---

# 🧱 Architecture

```
Developer → GitHub → Jenkins Pipeline → Maven Build → SonarQube Analysis → Quality Report
```

Infrastructure Setup:

```
AWS EC2 Instance
│
├── Docker
│    └── SonarQube Container
│
├── Jenkins Server
│
└── Java Application Repository
```

---

# 🛠️ Technologies Used

| Tool      | Purpose                            |
| --------- | ---------------------------------- |
| Java      | Application source code            |
| Maven     | Build automation                   |
| Jenkins   | CI/CD automation                   |
| SonarQube | Code quality & security analysis   |
| Docker    | Containerized SonarQube deployment |
| AWS EC2   | Cloud infrastructure               |
| GitHub    | Source code repository             |

---

# 🚀 Step-by-Step Implementation

## 1️⃣ Launch EC2 Instance

Launch an Ubuntu EC2 instance and connect via SSH.

```
ssh -i key.pem ubuntu@<EC2-IP>
```

Update packages:

```
sudo apt update
```

---

# 2️⃣ Install Docker

```
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Verify:

```
docker --version
```

---

# 3️⃣ Deploy SonarQube Using Docker

Pull the SonarQube image:

```
docker pull sonarqube:9.9-community
```

Run the container:

```
docker run -d \
--name sonarqube \
-p 9000:9000 \
sonarqube:9.9-community
```

Check container:

```
docker ps
```

Access SonarQube:

```
http://<EC2-Public-IP>:9000
```

Default login:

```
Username: admin
Password: admin
```

---

# 4️⃣ Install Jenkins

Install Jenkins on the EC2 instance and access:

```
http://<EC2-IP>:8080
```

Install recommended plugins.

---

# 5️⃣ Configure SonarQube in Jenkins

Navigate:

```
Manage Jenkins → Configure System
```

Add SonarQube server:

```
Name: SonarQube
URL: http://localhost:9000
Token: <generated_token>
```

---

# 6️⃣ Create SonarQube Token

In SonarQube:

```
My Account → Security → Generate Token
```

Store the token securely in Jenkins credentials.

---

# 7️⃣ Configure Maven in Jenkins

```
Manage Jenkins → Global Tool Configuration
```

Add Maven:

```
Name: Maven
Version: 3.x
```

---

# 8️⃣ Create Jenkins Pipeline

Jenkins pulls code from GitHub and executes the pipeline.

### Jenkinsfile

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/milind55555/Sonarqube_java_application.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                    mvn sonar:sonar \
                    -Dsonar.projectKey=java-app \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=$SONAR_TOKEN
                    """
                }
            }
        }
    }
}
```

---

# 📊 SonarQube Analysis Output

After pipeline execution, SonarQube generates a dashboard displaying:

| Metric          | Description                            |
| --------------- | -------------------------------------- |
| Bugs            | Code errors causing incorrect behavior |
| Vulnerabilities | Security risks in the code             |
| Code Smells     | Maintainability issues                 |
| Coverage        | Percentage of code covered by tests    |
| Duplications    | Repeated code blocks                   |

Example results:

```
Bugs: 3
Vulnerabilities: 1
Code Smells: 12
Coverage: 75%
```

---

# 🔍 Key Features of the Pipeline

✔ Automated code quality analysis
✔ Secure token management using Jenkins credentials
✔ Integration with GitHub repositories
✔ Continuous inspection of Java code
✔ Dockerized SonarQube deployment

---

# 🎯 Learning Outcomes

Through this project, the following DevOps concepts were implemented:

* CI/CD pipeline creation with Jenkins
* Static code analysis using SonarQube
* Docker container deployment
* Secure credential management
* Automated quality gate evaluation

---

# 📈 Real-World DevOps Workflow

```
Developer pushes code → GitHub
        ↓
Jenkins Pipeline Triggered
        ↓
Build Application (Maven)
        ↓
SonarQube Code Quality Scan
        ↓
Quality Gate Evaluation
        ↓
Report Available in SonarQube Dashboard
```

This approach is commonly used in **enterprise DevOps pipelines** to maintain high code quality standards.

---

# 📂 Repository Structure

```
Sonarqube_java_application
│
├── src/
├── pom.xml
├── Jenkinsfile
└── README.md
```

---

# 📌 Future Improvements

* Integrate **Docker image build and deployment**
* Add **Quality Gate checks to fail builds on poor code quality**
* Integrate with **Kubernetes deployment pipeline**

---

# 👨‍💻 Author

**Milind Hanchate**

DevOps & Cloud Enthusiast
Passionate about building scalable CI/CD pipelines and automated infrastructure.

---
