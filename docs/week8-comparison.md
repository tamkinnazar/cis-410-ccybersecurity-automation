| Dimension            | On-Premise Docker (Wks 3–5)                            | Cloud Run (Week 8)                                                 |
| -------------------- | ------------------------------------------------------ | ------------------------------------------------------------------ |
| Infrastructure setup | 3 VMs created manually and Docker installed on each VM | Fully managed by Google Cloud, no VM setup required                |
| Deployment command   | SSH into VM → build image → run container manually     | Terraform deploy + GitHub Actions + automatic Cloud Run deployment |
| TLS / HTTPS          | Not configured by default, manual setup required       | Automatically provided by Google Cloud Run                         |
| Scaling approach     | Manual scaling (add VMs or redeploy containers)        | Automatic scaling based on traffic, including scale-to-zero        |
| Port management      | Manual port assignment (5000, 5001, 5002)              | No manual port management, Cloud Run handles routing               |
| Cost when idle       | VM runs 24/7 even without traffic                      | Scales to zero when idle, no cost when not used                    |
| Rollback             | Manually redeploy previous Docker image                | Rollback by deploying previous commit SHA image                    |
| Secrets management   | SSH keys or manual environment variables               | GitHub Actions OIDC + IAM roles (no long-lived SSH keys)           |


Which approach required more manual steps?
The on-premise Docker approach required more manual steps because I had to SSH into VMs, install Docker, build images, and run containers manually. Cloud Run removed these steps by automatically deploying the container using Terraform and GitHub Actions.

How do you know which version is running in production?
In on-premise Docker, I had to manually check running containers and images on the VM. In Cloud Run, I can track the exact version using the commit SHA tag in Artifact Registry and Cloud Run revision history, making it easier to verify the deployed version.

What is the security advantage of scale-to-zero?
Scale-to-zero reduces the attack surface because there are no running containers when there is no traffic. This means fewer exposed services and reduced risk of exploitation compared to always-running VMs.

What attack surface was removed by OIDC?
OIDC removed the need for SSH keys and long-lived service account keys. This reduces the risk of leaked credentials because authentication is short-lived and automatically managed by GitHub and Google Cloud.
