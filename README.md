# DevOps Practical 1: Version Control with Git

**LYCS DevOps Sem 7, Q1: Version Control with Git (30 marks)**

Repository: https://github.com/show-star/aa1

## Aim

For a project named `student-management`: initialize a Git repository and set the username and email, create `index.html`, `style.css` and `app.js` and commit them, create a branch `feature-login` with a login form, switch back to `main` and change `app.js`, merge `feature-login` into `main`, then create a merge conflict on purpose, resolve it and show `git log --graph --oneline`.

## What this practical is about

- **Git** is a version control system: it records every change to your files, so you can see the history, go back, and work with others.
- A **repository (repo)** is a folder whose history Git tracks. The history lives in a hidden `.git` folder.
- A **commit** is a saved snapshot of the project, with a message saying what changed.
- The **staging area**: `git add` chooses which changes go into the next commit.
- A **branch** is a separate line of work. You build a feature on its own branch without touching `main`.
- A **merge** brings the work from one branch into another.
- A **merge conflict** happens when two branches change the **same line**. Git can't choose, so you decide by hand.

**Everything happens on your own PC.** This practical doesn't use GitHub, and `git config user.name` / `user.email` are just labels on your commits, not a login.

---

## Before you start

1. **Install Git for Windows** from https://git-scm.com (keep the default options).
2. **Install VS Code** from https://code.visualstudio.com.
3. **Open Command Prompt:** press the Windows key, type *cmd* and press Enter. Check Git works:
   ```
   git --version
   ```

**Where to do this:**

| What | Where |
| --- | --- |
| Every `git` command and `type` | **Command Prompt**. Keep this **one window** open for the whole practical |
| Creating and editing files | **VS Code** |
| Saving | Press **Ctrl + S** after every edit in VS Code, **before** `git add` or `git commit`. Git doesn't see unsaved changes |

---

## Step 1: Create the repo and set name and email (4 marks)

In **Command Prompt**:

```
cd %USERPROFILE%\Desktop
mkdir student-management
cd student-management
git init -b main
git config user.name "Your Name"
git config user.email "you@example.com"
git config --local --list
```

| Command | What it does |
| --- | --- |
| `git init -b main` | Turns the folder into a Git repo, with `main` as the first branch |
| `git config user.name` / `user.email` | Sets the name and email written on every commit in this repo |
| `git config --local --list` | Shows this repo's settings, including your name and email |

📸 **Screenshot:** the output of `git config --local --list`.

Open the folder in VS Code:

```
code .
```

If `code` isn't recognized, open VS Code yourself and click **File → Open Folder → Desktop → student-management**. If it asks whether you trust the authors, click **Yes**.

---

## Step 2: Create three files and make the first commit (5 marks)

In **VS Code**, hover over the folder name in the left panel and click the **New File** icon. Create each file, paste its content and press **Ctrl + S**.

**index.html**

```html
<h1>Student Management System</h1>
```

**style.css**

```css
body { font-family: Arial; background: #f4f4f4; }
```

**app.js**

```javascript
console.log("Student Management App");
```

Back in **Command Prompt**:

```
git add .
git status
git commit -m "Initial commit"
```

`git status` lists the three files in green under *Changes to be committed*. The commit output says `3 files changed`.

📸 **Screenshot:** `git status` and the commit output.

---

## Step 3: Create feature-login and add a login form (5 marks)

In **Command Prompt**:

```
git checkout -b feature-login
```

This creates the branch `feature-login` and switches to it. The bottom left corner of VS Code now shows **feature-login**.

In **VS Code**, open **index.html**, replace everything with the code below and press **Ctrl + S**:

```html
<h1>Student Management System</h1>
<h2>Login</h2>
<form>
  <input type="text" placeholder="Username">
  <input type="password" placeholder="Password">
  <button type="submit">Login</button>
</form>
```

In **Command Prompt**:

```
git add index.html
git commit -m "Add login form"
```

---

## Step 4: Switch back to main, change app.js and commit (4 marks)

In **Command Prompt**:

```
git checkout main
```

VS Code now shows `index.html` **without** the form. That's expected, because the form only exists on `feature-login` so far.

In **VS Code**, open **app.js**, add this as a **new second line** and press **Ctrl + S**:

```javascript
console.log("Main branch update");
```

In **Command Prompt**:

```
git add app.js
git commit -m "Update app.js on main"
```

---

## Step 5: Merge feature-login into main (4 marks)

In **Command Prompt**:

```
git merge feature-login -m "Merge feature-login into main"
type index.html
```

There's **no conflict**, because the two branches changed **different files** (`index.html` on `feature-login`, `app.js` on `main`). `type index.html` now shows the login form on `main`.

📸 **Screenshot:** the merge output and `type index.html`.

---

## Step 6: Create a merge conflict and resolve it (8 marks)

### a) Change style.css on a new branch

In **Command Prompt**:

```
git checkout -b feature-theme
```

In **VS Code**, change **style.css** to the line below and press **Ctrl + S**:

```css
body { font-family: Arial; background: lightblue; }
```

In **Command Prompt**:

```
git commit -am "Blue background"
```

`-am` stages every changed file that Git already tracks, and commits, in one command.

### b) Change the same line differently on main

In **Command Prompt**:

```
git checkout main
```

In **VS Code**, change **style.css** to the line below and press **Ctrl + S**:

```css
body { font-family: Arial; background: lightgreen; }
```

In **Command Prompt**:

```
git commit -am "Green background"
git merge feature-theme
```

Git reports:

```
Auto-merging style.css
CONFLICT (content): Merge conflict in style.css
Automatic merge failed; fix conflicts and then commit the result.
```

Both branches changed the **same line**, so Git can't decide which one to keep.

📸 **Screenshot:** the CONFLICT message.

### c) Look at the conflict markers

In **Command Prompt**:

```
type style.css
git status
```

```
<<<<<<< HEAD
body { font-family: Arial; background: lightgreen; }
=======
body { font-family: Arial; background: lightblue; }
>>>>>>> feature-theme
```

| Marker | Meaning |
| --- | --- |
| `<<<<<<< HEAD` | Start of the version on the current branch (`main`) |
| `=======` | Divider between the two versions |
| `>>>>>>> feature-theme` | End of the version from the branch being merged |

`git status` says *both modified: style.css*. In VS Code, the same file shows both versions in colour, with **Accept Current Change / Accept Incoming Change / Accept Both Changes** buttons.

📸 **Screenshot:** `type style.css` with the markers, and/or VS Code's conflict view.

### d) Resolve it

In **VS Code**, open **style.css**, delete **everything** (the markers and both versions), type only the line below and press **Ctrl + S**:

```css
body { font-family: Arial; background: lightyellow; }
```

(You could instead click **Accept Current Change** or **Accept Incoming Change** in VS Code and save. Any version without the markers resolves the conflict.)

In **Command Prompt**:

```
git add style.css
git commit -m "Resolve merge conflict in style.css"
git log --graph --oneline --all
```

The graph shows the branches splitting off and joining back into `main`, something like:

```
*   d4e5f6a (HEAD -> main) Resolve merge conflict in style.css
|\
| * c3d4e5f (feature-theme) Blue background
* | b2c3d4e Green background
|/
*   a1b2c3d Merge feature-login into main
|\
| * 9f8e7d6 (feature-login) Add login form
* | 8e7d6c5 Update app.js on main
|/
* 7d6c5b4 Initial commit
```

(Your commit IDs will be different.)

📸 **Screenshots:** the final commit and `git log --graph --oneline --all`.

---

## Screenshots checklist

1. `git config --local --list`
2. `git status` and the first commit
3. The merge of feature-login into main, and `type index.html`
4. The CONFLICT message
5. `type style.css` with the markers, or VS Code's conflict view
6. The final merge commit
7. `git log --graph --oneline --all`

---

## Errors and fixes

| Problem | Why | Fix |
| --- | --- | --- |
| `'git' is not recognized` | Git isn't installed, or Command Prompt was opened before installing it | Install Git for Windows, then open a **new** Command Prompt |
| `nothing to commit, working tree clean` | The file wasn't saved in VS Code | Press **Ctrl + S** in VS Code, then run the command again |
| `'code' is not recognized` | VS Code isn't on the PATH | Open the folder with **File → Open Folder** in VS Code |
| `warning: LF will be replaced by CRLF` | Harmless line-ending warning on Windows | Ignore it, or run `git config core.autocrlf true` |
| `'cat' is not recognized` / `'grep' is not recognized` | Those are Linux commands; you're in Command Prompt | Use `type` instead of `cat`, and `findstr` instead of `grep` |
| `error: unknown switch 'b'` on `git init` | Old version of Git | Run `git init`, then `git checkout -b main` |
| `Please tell me who you are` | Name and email weren't set in this repo | Run the two `git config` commands from Step 1 again |
| `fatal: not a git repository` | You're not inside the `student-management` folder | `cd %USERPROFILE%\Desktop\student-management` |
| A text editor (Vim) opens in the window during a commit or merge | A commit message is being asked for | Press **Esc**, type `:wq` and press **Enter** |
| `git log` fills the screen and won't return to the prompt | The output is shown one page at a time | Press **q** |
| `error: you need to resolve your current index first` | The conflict isn't finished yet | Fix `style.css`, save, then `git add style.css` and `git commit` |
| `mkdir` says *A subdirectory or file student-management already exists* | You've done this before | Use another name, or delete the old folder first |

**No Git Bash?** That's fine: this README uses **Command Prompt**, which every Windows PC has. If you prefer **Git Bash**, all `git` commands are identical. Just use `cat` instead of `type`, and `cd ~/Desktop` instead of `cd %USERPROFILE%\Desktop`.

---

## Viva points

| Question | Answer |
| --- | --- |
| What is Git? | A distributed version control system that tracks changes to files over time |
| Git vs GitHub? | Git is the tool on your PC. GitHub is a website that hosts Git repositories online |
| What is a repository? | A folder whose history Git tracks, stored in the hidden `.git` folder |
| What is a commit? | A saved snapshot of the project, with a message and an ID |
| What does `git add` do? | Moves changes into the staging area, ready for the next commit |
| What does `git status` show? | Changed, staged and untracked files, and the current branch |
| What is a branch? | An independent line of work. `main` is the default branch |
| `git checkout -b` vs `git checkout`? | `-b` creates a new branch and switches to it. Without `-b`, it just switches |
| What is a merge? | Combining the changes from one branch into another |
| What is a merge conflict? | Two branches changed the same line, so a person must choose what to keep |
| How do you resolve a conflict? | Edit the file to the final version, remove the markers, then `git add` and `git commit` |
| What does `git log --graph --oneline --all` show? | The full history as a tree: one line per commit, with all branches |

## Conclusion

We created a Git repository and set the username and email, committed three files, created a `feature-login` branch with a login form, updated `app.js` on `main`, and merged the branch without conflict. We then made the two branches change the same line of `style.css`, which caused a merge conflict, resolved it by hand and completed the merge. `git log --graph --oneline --all` showed the branches splitting and joining. This demonstrates how Git supports version control, parallel work on branches, and teamwork.
