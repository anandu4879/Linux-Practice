# KodeKloud Git Tasks - Future Reference

## 1. Install Git and Create a Non-Bare Repository

### Question

The Nautilus development team shared with the DevOps team requirements for new application development, setting up a Git repository for that project.

Create a Git repository on Storage server in Stratos DC as per details given below:

* Install git package using yum on Storage server.
* Create/init a git repository named `/opt/blog.git`.
* Make sure not to create a bare repository.

### Solution

```bash
sudo su -

yum install -y git

mkdir -p /opt/blog.git

git init /opt/blog.git
```

### Verification

```bash
git -C /opt/blog.git status
```

---

## 2. Create a New Branch from Master

### Question

Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/ecommerce`.

Create a new branch `xfusioncorp_ecommerce` from the `master` branch.

Do not make any code changes.

### Solution

```bash
sudo su -

cd /usr/src/kodekloudrepos/ecommerce

git checkout master

git checkout -b xfusioncorp_ecommerce
```

### Verification

```bash
git branch
```

---

## 3. Create Branch, Commit File, Merge and Push

### Question

The Nautilus application development team has been working on project repository `/opt/ecommerce.git`.

This repository is cloned at:

```text
/usr/src/kodekloudrepos/ecommerce
```

Requirements:

* Create a branch named `datacenter` from master.
* Copy `/tmp/index.html` into the repository.
* Add and commit the file.
* Merge branch back into master.
* Push changes to origin for both branches.

### Solution

```bash
sudo su -

cd /usr/src/kodekloudrepos/ecommerce

git checkout master

git checkout -b datacenter

cp /tmp/index.html .

git add index.html

git commit -m "Add index.html"

git push origin datacenter

git checkout master

git merge datacenter

git push origin master
```

### Verification

```bash
git branch

git log --oneline --decorate -5
```

---

## 4. Add New Remote and Push Changes

### Question

The xFusionCorp development team added updates to the project maintained under:

```text
/opt/cluster.git
```

The repository is cloned at:

```text
/usr/src/kodekloudrepos/cluster
```

Requirements:

* Add a remote named `dev_cluster`.
* Point it to `/opt/xfusioncorp_cluster.git`.
* Copy `/tmp/index.html` into the repository.
* Add and commit the file on master.
* Push master branch to the new remote.

### Solution

```bash
sudo su -

cd /usr/src/kodekloudrepos/cluster

git remote add dev_cluster /opt/xfusioncorp_cluster.git

git checkout master

cp /tmp/index.html .

git add index.html

git commit -m "Add index.html"

git push dev_cluster master
```

### Verification

```bash
git remote -v

git log --oneline -1
```

---

# Git Remote Notes

## What is a Remote?

A remote is a nickname for another Git repository.

Example:

```bash
git remote add origin https://github.com/user/repo.git
```

Here:

* `origin` = remote name
* URL = actual repository location

### View Remotes

```bash
git remote -v
```

### Push

```bash
git push origin master
```

### Pull

```bash
git pull origin master
```

### Multiple Remotes Example

```text
Local Repository
     |
     |---- origin ------> Main Repository
     |
     |---- dev_cluster -> Backup Repository
```

---

## 5. Revert Latest Commit

### Question

The Nautilus application development team reported an issue with the latest commit in:

```text
/usr/src/kodekloudrepos/news
```

Requirements:

* Revert the latest commit (HEAD).
* Previous commit is the initial commit.
* Use commit message:

```text
revert news
```

### Important

Use:

```bash
git revert
```

Do NOT use:

```bash
git reset
```

### Solution

```bash
cd /usr/src/kodekloudrepos/news

git revert HEAD --no-edit

git commit --amend -m "revert news"
```

### Verification

```bash
git log --oneline -3
```

### Before Revert

```text
B (HEAD)
A (initial commit)
```

### After Revert

```text
C (revert news)
B
A (initial commit)
```

Where:

* B = bad commit
* C = commit that undoes B

### Difference Between Revert and Reset

#### git revert

Creates a new commit that undoes changes.

```text
A --- B --- C
```

Safe for shared repositories.

#### git reset --hard

Moves HEAD backward and deletes commit history.

```text
A
```

Dangerous on shared repositories.

```
```
