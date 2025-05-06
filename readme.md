# Automating CI CD with Jenkins on Google Cloud

This is a sample demo showcasing how to perform CI/CD integration using Jenkins. It covers the comprehensive process of setting up DevOps tools on Google Cloud, integrating Jenkins with Tomcat for automated deployments, and performing Git workflow operations.

I'm excited to share these steps with the community to help others understand how to build a complete CI/CD pipeline in the cloud.

---

##  Infrastructure Overview

To accomplish this task, I created **4 Virtual Machines** (VMs) in Google Cloud:

| VM        | Purpose                                                        |
|-----------|----------------------------------------------------------------|
| platform  | Acts like a developer machine for handling production code     |
| jenkins   | Hosts Jenkins for CI/CD automation                             |
| tomcat-1  | Tomcat server for internal deployment and testing              |
| tomcat-2  | Another Tomcat server for internal sharing and redundancy      |

<img width="1507" alt="Screenshot 2025-05-05 at 17 44 23" src="https://github.com/user-attachments/assets/7ad54d7d-e807-44ee-a475-f6f8d27e9397" />

---

## CI/CD Pipeline Steps

### ✅ Step 1: Clone GitHub Repository to Platform VM
I cloned my project repository to the platform VM, simulating a developer pushing code changes.

### ✅ Step 2: Configure Jenkins VM
- Installed Jenkins
- Set up Java, Git, and required dependencies
- Installed necessary Jenkins plugins (e.g., Git, Deploy to Container, etc.)

### ✅ Step 3: Configure Tomcat VMs
- Installed and configured Apache Tomcat
- Enabled `manager-script` role for remote deployments
- Verified accessibility of `/manager/text` endpoint

---

## 📚 What I Learned

- **Jenkins Setup on GCP**: Learned how to install and configure Jenkins in a VM environment with necessary tools and variables.
- **CI/CD Pipeline Creation**: Built pipelines to automate deployment from Jenkins to Tomcat.
- **Git Mastery**: Practiced advanced Git commands for branching, merging, and managing development workflows.
- **Understanding SDLC**: Gained insights into managing and automating the software development lifecycle, from code push to production deployment.

---

## 🙌 Contribution

If you found this useful, feel free to fork, suggest improvements, or reach out!

---

## 📬 Contact

If you have any questions, feel free to open an issue or connect via [LinkedIn](#) *(update with your link)*.

