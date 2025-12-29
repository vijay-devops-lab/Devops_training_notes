# Jenkins CI/CD Pipeline Documentation

  

## 1. Introduction

This document describes the complete implementation of a CI/CD pipeline using **Jenkins**, **GitHub**, **Maven**, and **SSH-based deployment**. The pipeline automatically builds and deploys a Java web application whenever code is pushed to the `main` branch of the GitHub repository.

  

---

  

## 2. Tools & Technologies Used

- **GitHub** – Source code repository

- **Jenkins** – CI/CD automation server

- **Maven** – Build and dependency management tool

- **Java (WAR application)** – Application type

- **Publish Over SSH Plugin** – Artifact deployment

- **Linux (Ubuntu)** – Jenkins & deployment servers

  

---

  

## 3. Architecture Overview  

### Pipeline Flow:

1. Developer pushes code to the `main` branch in GitHub

2. GitHub Webhook notifies Jenkins

3. Jenkins pulls the latest code

4. Maven builds the project and generates a WAR file

5. Jenkins transfers the WAR file to a remote server using SSH

6. Deployment is verified on the target server  

---

  

## 4. Project Structure

  

```

build_and_deploy/

├── pom.xml                (Parent POM)

├── server/                (Backend module)

│   └── pom.xml

├── webapp/                (Web application module)

│   ├── pom.xml

│   └── src/main/webapp

└── Dockerfile

```

  

The project is a **multi-module Maven project**, where the `webapp` module generates the WAR file.

  

---

  

## 5. Jenkins Job Configuration

  

### 5.1 Source Code Management

- SCM: **Git**

- Repository URL: `https://github.com/vijay-devops-lab/my_first_pipeline.git`

- Branch to build: `*/main`

- Credentials: GitHub credentials stored in Jenkins

  

---

  

### 5.2 Build Trigger

- Trigger type: **GitHub hook trigger for GITScm polling**

- Purpose: Automatically trigger the Jenkins job when code is pushed to GitHub

  

---

  

### 5.3 Build Step (Maven)

- Build tool: Maven

- Goals:

  ```

  clean package

  ```

- Output:

  - WAR file generated at:

    ```

    webapp/target/webapp.war

    ```

  

---

  

## 6. Post-Build Deployment (SSH)

  

### SSH Publisher Configuration

- SSH Server Name: `dockerhost`

- Remote Server Directory: `/opt/docker`

  

### Artifact Transfer Settings

- Source files:

  ```

  webapp/target/webapp.war

  ```

- Flatten files: Enabled

  

### Exec Command (Normalization)

```bash

mv /opt/docker/opt/docker/webapp.war /opt/docker/ || true

rm -rf /opt/docker/opt

ls -lh /opt/docker

```

  

This ensures the WAR file is placed directly inside `/opt/docker`.

  

---

  

## 7. Verification Steps

  

### On Jenkins Server

```bash

ls /var/lib/jenkins/workspace/build_and_deploy/webapp/target

```

Expected output:

```

webapp.war

```

  

### On Deployment Server

```bash

ls -lh /opt/docker

```

Expected output:

```

webapp.war

```

  

---

  

## 8. GitHub Webhook Configuration

  

- Payload URL:

  ```

  http://<JENKINS_PUBLIC_IP>:8080/github-webhook/

  ```

- Content type: `application/json`

- Events: Push events

- Status: Active

  

This webhook triggers Jenkins automatically on every push to the `main` branch.

  

---

  

## 9. Pipeline Validation

  

The pipeline is considered successful if:

- Jenkins job starts automatically on Git push

- Maven build completes successfully

- WAR file is generated

- WAR file is transferred to the remote server

- Artifact exists in `/opt/docker`

  

---

  

## 10. Conclusion

  

This CI/CD pipeline successfully automates the build and deployment process for a Maven-based Java web application. By integrating GitHub webhooks with Jenkins, the pipeline ensures continuous integration and continuous deployment with minimal manual intervention.

  

---

  

## 11. Future Enhancements

- Deploy WAR into a Tomcat Docker container

- Convert freestyle job to Jenkinsfile (Declarative Pipeline)

- Add automated testing stage

- Implement Docker image build and push