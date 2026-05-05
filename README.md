# Git Cheat Sheet

[Türkçe](#turkce) | [English](#english)

---

## Turkce

Bu dosya temel Git komutlarini, push/history uyusmazligi senaryolarini ve belirli dosyalari haric tutma orneklerini icerir.

### 1) Temel Git Komutlari

#### `git init`
Yeni bir Git deposu baslatir.

```bash
mkdir project && cd project
git init
```

#### `git clone`
Uzak depoyu bilgisayarina kopyalar.

```bash
git clone https://github.com/username/example-repo.git
cd example-repo
```

#### `git status`
Calisma alanindaki degisikliklerin durumunu gosterir.

```bash
git status
```

#### `git add`
Degisiklikleri commit oncesi hazirlik alanina alir.

```bash
git add .
# veya
git add src/app.js
```

#### `git commit`
Staged degisiklikleri kayda alir.

```bash
git commit -m "Updated login button text"
```

#### `git log`
Commit gecmisini listeler.

```bash
git log --oneline --graph --decorate
```

#### `git diff`
Degisikliklerin satir farklarini gosterir.

```bash
git diff
git diff --staged
```

#### `git branch`
Branch listeler veya yeni branch olusturur.

```bash
git branch
git branch feature/login
```

#### `git switch`
Branch degistirir veya yeni branch olusturup gecis yapar.

```bash
git switch feature/login
git switch -c feature/profile
```

#### `git merge`
Baska bir branch'i mevcut branch'e birlestirir.

```bash
git switch main
git merge feature/login
```

#### `git pull`
Remote degisiklikleri alir ve yerelde birlestirir.

```bash
git pull origin main
```

#### `git push`
Yerel commit'leri remote depoya gonderir.

```bash
git push origin main
```

#### `git remote -v`
Bagli remote adreslerini gosterir.

```bash
git remote -v
```

#### `git stash`
Commit edilmemis degisiklikleri gecici olarak saklar.

```bash
git stash
git switch main
git stash pop
```

### 2) Push ve History Uyusmazligi

#### Ilk kez branch push etmek (`-u`)
Branch'i remote'a gonderir ve upstream baglantisi kurar.

```bash
git push -u origin feature/login
```

#### `non-fast-forward` hatasi
Remote branch ondeyse once degisiklikleri al, sonra push et.

```bash
git pull --rebase origin main
git push origin main
```

#### `unrelated histories` hatasi
Local ve remote farkli gecmislerden geliyorsa kullanilir.

```bash
git pull origin main --allow-unrelated-histories
git push origin main
```

#### Local history ile remote'u ezme (dikkat)
Remote gecmisini yerel gecmis ile zorla gunceller.

```bash
git push --force-with-lease origin main
```

### 3) x, y, z Haric Gonderme

#### Yontem A (onerilen)
Once hepsini stage et, istemediklerini stageden cikar.

```bash
git add .
git restore --staged x.js y.txt z/config.json
git commit -m "Changes except x, y, z"
git push
```

#### Yontem B (tek komut)
`git add` sirasinda dosyalari haric tutar.

```bash
git add . ':!x.js' ':!y.txt' ':!z/config.json'
git commit -m "Changes except x, y, z"
git push
```

#### Kontrol komutlari
Commit oncesi staged icerigi dogrular.

```bash
git status
git diff --staged
```

### 4) Pull'da Otomatik Entegrasyonu Engelleme

#### Guvenli yaklasim: `pull` yerine `fetch`
Remote degisiklikleri indirir, local branch'e otomatik birlestirme yapmaz.

```bash
git fetch origin
```

#### Entegrasyon oncesi inceleme
Gelen commit ve dosya farklarini gosterir.

```bash
git log --oneline --graph HEAD..origin/main
git diff --name-status HEAD..origin/main
```

#### Manuel ve secici entegrasyon
Tumunu, tek commit'i veya tek dosyayi secerek alabilirsin.

```bash
# Tumunu al
git merge origin/main
# veya
git rebase origin/main

# Tek commit
git cherry-pick <commit_hash>

# Tek dosya
git restore --source=origin/main -- path/to/file
```

#### Guvenli `git pull` ayari
Sadece fast-forward mumkunse `pull` calisir.

```bash
git config --global pull.ff only
```

[Basa don](#git-cheat-sheet)

---

## English

This document includes basic Git commands, push/history mismatch scenarios, and examples of excluding specific files.

### 1) Basic Git Commands

#### `git init`
Initializes a new Git repository.

```bash
mkdir project && cd project
git init
```

#### `git clone`
Copies a remote repository to your local machine.

```bash
git clone https://github.com/username/example-repo.git
cd example-repo
```

#### `git status`
Shows the current state of your working tree.

```bash
git status
```

#### `git add`
Stages changes before committing.

```bash
git add .
# or
git add src/app.js
```

#### `git commit`
Saves staged changes to history.

```bash
git commit -m "Updated login button text"
```

#### `git log`
Lists commit history.

```bash
git log --oneline --graph --decorate
```

#### `git diff`
Shows line-by-line differences.

```bash
git diff
git diff --staged
```

#### `git branch`
Lists branches or creates a new branch.

```bash
git branch
git branch feature/login
```

#### `git switch`
Switches branches or creates one and switches to it.

```bash
git switch feature/login
git switch -c feature/profile
```

#### `git merge`
Merges another branch into your current branch.

```bash
git switch main
git merge feature/login
```

#### `git pull`
Fetches and merges remote changes into local branch.

```bash
git pull origin main
```

#### `git push`
Sends local commits to the remote repository.

```bash
git push origin main
```

#### `git remote -v`
Shows connected remote URLs.

```bash
git remote -v
```

#### `git stash`
Temporarily stores uncommitted changes.

```bash
git stash
git switch main
git stash pop
```

### 2) Push and History Mismatch

#### First push of a branch (`-u`)
Pushes a branch and sets its upstream.

```bash
git push -u origin feature/login
```

#### `non-fast-forward` error
If remote is ahead, pull first, then push.

```bash
git pull --rebase origin main
git push origin main
```

#### `unrelated histories` error
Used when local and remote have separate histories.

```bash
git pull origin main --allow-unrelated-histories
git push origin main
```

#### Overwrite remote history (careful)
Force updates remote with local history.

```bash
git push --force-with-lease origin main
```

### 3) Push All Except x, y, z

#### Method A (recommended)
Stage everything, then unstage excluded files.

```bash
git add .
git restore --staged x.js y.txt z/config.json
git commit -m "Changes except x, y, z"
git push
```

#### Method B (one command)
Excludes selected files directly during `git add`.

```bash
git add . ':!x.js' ':!y.txt' ':!z/config.json'
git commit -m "Changes except x, y, z"
git push
```

#### Verification commands
Checks staged content before commit.

```bash
git status
git diff --staged
```

### 4) Avoid Direct Integration on Pull

#### Safe approach: use `fetch` instead of `pull`
Downloads remote changes without merging into your local branch.

```bash
git fetch origin
```

#### Review before integration
Shows incoming commits and changed files.

```bash
git log --oneline --graph HEAD..origin/main
git diff --name-status HEAD..origin/main
```

#### Manual and selective integration
You can integrate all, a specific commit, or a specific file.

```bash
# Integrate everything
git merge origin/main
# or
git rebase origin/main

# Specific commit
git cherry-pick <commit_hash>

# Specific file
git restore --source=origin/main -- path/to/file
```

#### Safer `git pull` setting
Allows pull only when fast-forward is possible.

```bash
git config --global pull.ff only
```

[Back to top](#git-cheat-sheet)
