# Best Practices

## Story duration - Actual 

After compleating each issue it is mandatory to note your time spend on the issue. Use the "Actual" field of the issue itself. The time you spend working on the issue is rounded up to the next full hour -> 1h20min is going to be recorded as 2h

![example image actual field in issue](image.png)


## Working on an issue

### Feature branch
If the issue requires changes to the repository you create a new branch for the issue. Such a branch is called a feature branch.

This can be done from the issue:   

![alt text](image-2.png)

#### Naming convention for feature branch

Feature branch name as follows "[Repository#issue-nr] issue title".  
Bsp.: [D#9] update feature branch naming convention

The Repository is abbreviated with:
- 'S' for ColdCase-Server
- 'C' for ColdCase-Client
- 'D' for ColdCase-Documentation

This done so branches can be deterministically assigned one issue across all repositorys.

---
### Coding best practices

- if you work with InteliJ Idea regularly press `Strg + Alt + L` to reformat your file

---
### Commit Messages

Commit message as follows "[Repository#issue-nr] Description of your changes".  
Bsp.: S[#7] Added Tile Content to Tile

The Repository is abbreviated with:
- 'S' for ColdCase-Server
- 'C' for ColdCase-Client
- 'D' for ColdCase-Documentation

This done so commits can be deterministically assigned one issue across all repositorys.


---
### Pushing to development

After compleating the issue:

1. create and resolve a pull-request from from development branch to your feature branch (this is to prevent merge conflicts on development)
2. create a pull-request from from development branch to main branch
3. 2 or more coworkers have to approve the pull-request 
4. merge the pull-request


## Pushing to main

1. create a pull-request from from development branch to main branch
2. 2 or more coworkers have to approve the pull-request 
3. merge the pull-request

