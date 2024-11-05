---
title: "How to Delete Local and Remote Branches in Git: A Complete Guide"
seoTitle: "How to Delete a Git Branch: Local and Remote"
seoDescription: "Learn how to efficiently delete local and remote branches in Git. Discover the difference between them and commands for safe and force deletions."
datePublished: Mon Nov 04 2024 04:00:38 GMT+0000 (Coordinated Universal Time)
cuid: cm32htwit000008jme7y3apag
slug: how-to-delete-local-and-remote-branches-in-git-a-complete-guide
canonical: https://keploy.io/blog/community/how-to-delete-local-and-remote-branches-in-git-a-complete-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728887728866/b5089871-5788-43a0-bbb5-a53216f915be.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1728887774586/c9e302bc-0b55-4c6e-b5b4-740021d6cebf.png
tags: github, git, git-branch, git-branching

---

### TLDR;

> **Deleting a Local Branch in Git**
> 
> To delete a local branch, use one of these commands:
> 
> * **Safe deletion**: `git branch -d <branchName>`
>     
> * **Force deletion**: `git branch -D <branchName>`
>     
> 
> **Deleting a Remote Branch in Git**
> 
> To delete a branch from a remote repository (like GitHub or GitLab):
> 
> ```bash
> git push origin --delete <branchName>
> ```

## Local Branch vs. Remote Branch in Git

Understanding the difference between local and remote branches is crucial for working with Git effectively.

### Local Branch

* **Location**: Exists only on your machine.
    
* **Purpose**: For individual development; changes here won’t affect other developers. It’s where you do your day-to-day development and changes.
    
* You can create, delete, and modify it without affecting other developers.
    
* **Command to List Local Branches**: `git branch`
    

### Remote Branch

* **Location**: Exists on a remote server (e.g., GitHub, GitLab).
    
* **Purpose**: For collaboration; changes here are accessible to the team, allowing multiple people to work on the same branch.
    
* You need to fetch or pull changes from the remote to sync it with your local branch.
    
* **Command to List Remote Branches**: `git branch -r`
    

![How to set Upstream Branch on Git?](https://s3.ap-south-1.amazonaws.com/s3.studytonight.com/tutorials/uploads/pictures/1623294975-103268.png align="left")

## Key Differences Between Local and Remote Branches

| **Feature** | **Local Branch** | **Remote Branch** |
| --- | --- | --- |
| **Location** | Exists on your machine only | Exists on a remote server |
| **Collaboration** | Used for individual work | Used for collaboration with the team |
| **Creation/Deletion** | Only affects your local repository | Affects everyone who accesses the remote |
| **Syncing** | You can work offline | Requires fetching or pulling updates |

## Understanding with an example

Let’s walk through a practical example.

### Step 1: Cloning the Repository

Let us clone a basic repo from GitHub

```bash
 git clone git@github.com:TvisharajiK/sample.git

#git shows the below 
Cloning into 'sample'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (3/3), done.
```

Navigate to the cloned directory and check the available branches:

```bash
cd sample
git branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

Currently, we have only the `main` branch in the `sample` repository.

### Step 2: Working with Local Branches

Let's create a local branch named `try`

```bash
$ git checkout -b try
Switched to a new branch 'try'
```

#### Deleting the Feature Branch

Now, try to delete the `try` branch using the `-d` option:

```bash
$ git branch -d try    
error: Cannot delete branch 'try' checked out at '/Users/tvisharaji/sample'
```

You’ll encounter an error since you cannot delete the currently checked-out branch.

#### **Switching to the Main Branch**

Switch to the `main` branch to delete `try`:

```bash
$ git checkout main
Switched to branch 'main'
```

Now, delete the `try` branch again:

```bash
$ git branch -d try
Deleted branch feature (was 3aac499)
```

### Deleting a Local Branch With the `-D` Option

Let’s recreate the `try` branch and make some changes:

```bash
$ git checkout -b try
$ echo "trying this" >> README.md
$ git add README.md
$ git commit -m 'Add to README'
```

Attempting to delete the branch with `-d` again, it will show an error because it contains unmerged changes:

```bash
$ git branch -d try
error: The branch 'try' is not fully merged.
```

#### Force Deletion

To force delete the branch, use:

```bash
$ git branch -D try
Deleted branch try (was 4a87db9)
```

### Understanding Local Branch Deletion

It’s important to note that using `-d` or `-D` will only remove the **local branch**; any corresponding **remote branch** will remain unaffected.

## Deleting a Remote Branch

To delete a remote branch, use one of the following commands:

For Git versions 1.7.0 and later:

```bash
git push origin --delete <branchName>
```

### Creating and Pushing a Remote Branch

Let’s create a `tryrem` branch, make some changes, and push it to the remote repository:

```bash
$ git checkout -b tryrem
$ echo "trying for the Remote branch" > remote.txt
$ git add remote.txt
$ git commit -m 'Add remote.txt'
$ git push origin tryrem
```

### Deleting the Remote Branch

To remove the remote `tryrem` branch:

```bash
$ git push origin --delete tryrem
```

After executing this command, the remote branch is deleted, but your local branch will still exist.

## Conclusion

In this article, we explored how to delete local and remote branches in Git. Here’s a quick summary:

* **Delete a local branch**: `git branch -d <branchName>` or `git branch -D <branchName>` for force deletion.
    
* **Delete a remote branch**: `git push origin --delete <branchName>`
    

## FAQ

### What command do I use to delete a local branch in Git?

* To delete a local branch safely, use:
    
    ```bash
    bashCopy codegit branch -d <branchName>
    ```
    
    To force delete a local branch, use:
    
    ```bash
    bashCopy codegit branch -D <branchName>
    ```
    

### How do I delete a remote branch in Git?

* To delete a remote branch, use one of the following commands:
    
    ```bash
    bashCopy codegit push origin --delete <branchName>
    ```
    
    or
    
    ```bash
    bashCopy codegit push origin :<branchName>
    ```
    

### What is the difference between a local branch and a remote branch?

* **Local Branch**: Exists on your machine only; used for individual development without affecting others. Commands to manage local branches include `git branch` to list them.
    
* **Remote Branch**: Exists on a remote server (e.g., GitHub or GitLab); used for collaboration. Changes to remote branches affect all collaborators. Use `git branch -r` to list remote branches.
    

### Can I delete a branch that I’m currently on?

* No, you cannot delete the branch you are currently checked out on. You must switch to another branch (e.g., `main`) before deleting the desired branch.
    

### What happens if I try to delete a branch that has unmerged changes?

* If you attempt to delete a local branch with unmerged changes using the `-d` option, you will receive an error. To forcefully delete the branch, you can use the `-D` option.
    

### What are the consequences of deleting a local branch?

* Deleting a local branch with `-d` or `-D` only affects your local repository. The corresponding remote branch will remain intact unless explicitly deleted.
    

### How can I verify if a branch has been deleted successfully?

* To check if a branch has been deleted:
    
    * For local branches, use:
        
        ```bash
        bashCopy codegit branch
        ```
        
    * For remote branches, use:
        
        ```bash
        bashCopy codegit branch -r
        ```
        

### What is the process for creating and pushing a new branch to a remote repository?

* To create and push a new branch to a remote repository:
    
    ```bash
    bashCopy codegit checkout -b <newBranchName>
    git push origin <newBranchName>
    ```