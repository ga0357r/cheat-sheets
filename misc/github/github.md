# GitHub

GitHub is an Internet hosting service for software development and version control using Git ([[git]]), plus access control, bug tracking, software feature requests, task management, continuous integration, and wikis for every project.

---
## GitHub Actions

Automate, customize, and execute your software development workflows right in your repository with GitHub Actions ([[repos/cheat-sheets/misc/github-actions]]). You can discover, create, and share actions to perform any job you'd like, including CI/CD, and combine actions in a completely customized workflow.

---
## Add New SSH Key to Github Account
For more information, see [link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

Create the key in local terminal(powershell) using ``` ssh-keygen -t ed25519 -C example@email.com ```.

Add the Key to your service agent in local terminal using 
``` 
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
ssh-add c:/Users/exampleUser/.ssh/id_ed25519
```

add the key to your github account ``` cat ~/.ssh/id_ed25519.pub | clip ```. And go to your [GitHub Key Settings Page](https://github.com/settings/keys) and add your SSH key
