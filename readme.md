# 🔧 Ansible Automation for ServiceNow Incident Resolution with Control-M Integration

## 📘 Overview

This project demonstrates an **enterprise-grade setup** to automate incident resolution by integrating:

- **ServiceNow** (with OAuth2 & MFA support)
- **Control-M Web API** (to trigger recovery jobs)
- **Ansible** (as the automation engine)

The setup eliminates manual MFA input by using **OAuth2 Refresh Tokens** and enables secure, hands-free integration for production-grade environments.

---

## 🏗️ Architecture Diagram
![image](https://github.com/user-attachments/assets/2884ffa6-4edd-4019-8c31-7e1550cb0497)



---

## 📦 Use Case

1. Incident is raised in **ServiceNow** due to an application failure.
2. Ansible detects the incident.
3. Ansible triggers a **Control-M job** to remediate the failure.
4. ServiceNow incident is updated with job status and comments.
5. All steps are handled **automatically and securely**.

---

## 🔐 ServiceNow OAuth2 Setup (One-Time)

1. Go to **ServiceNow → Application Registry → New**
2. Choose: **"Create an OAuth API endpoint for external clients"**
3. Configure:
   - **Name**: Ansible Automation
   - **Client ID**: (auto-generated or custom)
   - **Client Secret**: (secure and unique)
   - **Grant Types**: Enable `password` and `refresh_token`
4. Save and note the following for automation:
   - `client_id`
   - `client_secret`

---

## 🔁 Generate Initial Refresh Token (One-Time)

Use a secure tool like **Postman** or **cURL** to get the initial `refresh_token` and `access_token`:

- Authenticate using:
  - Username
  - Password
  - MFA (if required)
- Receive:
  - `access_token` (short-lived)
  - `refresh_token` (long-lived — use in automation)

---

## 🔑 Token-Based Authentication Strategy

| Token Type       | Purpose            | Requires MFA? |
|------------------|--------------------|---------------|
| Access Token     | Used in API calls  | ❌            |
| Refresh Token    | Generates access token | ✅ Only Once |

Once the refresh token is generated, **MFA is no longer needed** for automation.

---

## 🔐 Secret Management

All sensitive values such as:
- `client_id`
- `client_secret`
- `refresh_token`
- `controlm_token`

Should be stored securely using:
- **Ansible Vault**, or
- **CI/CD Secret Managers** (e.g., GitLab CI/CD, GitHub Secrets, AWS Secrets Manager)

---

## 🧰 Playbook Workflow

The playbook does the following:

1. Uses **refresh token** to get a fresh `access_token` from ServiceNow.
2. Fetches incident details using the `access_token`.
3. Triggers a **Control-M job** for remediation.
4. Updates the incident in ServiceNow with the job result.

All steps are automated without needing interactive login or MFA.

---

## 🚀 CI/CD Pipeline Example (GitLab)

A sample `.gitlab-ci.yml` pipeline can:
- Run the playbook on incident detection
- Use secure CI/CD environment variables or Ansible Vault for secrets
- Maintain logs and rollback logic for compliance

---

## 🔒 Best Practices

- Use **long-lived refresh tokens** securely.
- Rotate tokens using automation pipelines.
- Use **`no_log: true`** for sensitive Ansible tasks.
- Enable **logging and monitoring** for every automation action.
- Implement **retries and error handling** in playbook logic.

---

## 📊 Summary Table

| Component         | Role in Automation                        |
|------------------|--------------------------------------------|
| Ansible           | Automation engine                          |
| ServiceNow OAuth2 | Secure API access with refresh token       |
| Control-M API     | Job execution system                       |
| Ansible Vault     | Secrets management                         |
| GitLab CI/CD      | Pipeline automation                        |

---

## 📞 Support & Contributions

For contributions, feature requests, or issues, please create a GitHub issue or pull request.

---

## 📚 References

- [ServiceNow OAuth Docs](https://docs.servicenow.com)
- [Control-M Automation API Guide](https://documents.bmc.com)
- [Ansible URI Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html)
