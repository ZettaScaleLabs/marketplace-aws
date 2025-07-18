---
## Prerequisite

Ensure that ports 22 is accessible.

## 🔧 Connect to EC2

> ℹ️  **Info**  
> Your environment is chrooted to the Zenoh directory
> Command available: cp, cd, ls, vim, cat, jq
> 
> The dedicated user for ssh is "zenohd"

```bash
ssh -i ~/.ssh/your_id_rsa zenohd@{$IP}
```
