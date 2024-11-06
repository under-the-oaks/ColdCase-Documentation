# Best Practices

## Story duration - Actual 

After compleating each issue it is mandatory to note your time spend on the issue. Use the "Actual" field of the issue itself. The time you spend working on the issue is rounded up to the next full hour -> 1h20min is going to be recorded as 2h

![example image actual field in issue](image.png)


## Working on an issue

### New branch
If the issue requires changes to the repository you create a new branch for the issue. Such a branch is called a feature or bug branch, depending on the type of issue.

This can be done from the issue (see image below), which links the new branch to the issue automatically. Alternatively, this can be done manually by clicking on the cog icon next to the "Development" Section of the issue.

**ALWAYS LINK YOUR BRANCH TO THE CORRESPONDING ISSUE**

![alt text](image-2.png)


#### Naming convention for feature branch

Feature branch name as follows "NR-issue-name-in-kebap-case".  
Bsp.: 9-update-feature-branch-naming-convention



---
### Coding best practices

- if you work with InteliJ Idea regularly press `Strg + Alt + L` to reformat your file

---
### Commit Messages

Commit message name as follows "NR short summary of your changes".  
Bsp.: "9 changed naming conventio to kabab case for issue branches"


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

