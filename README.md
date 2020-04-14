## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [x] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Выполнить инструкцию учебного материала
- [x] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя> #initialize value GITHUB_USERNAME
$ export GITHUB_EMAIL=<адрес_почтового_ящика> #initialize value GITHUB_EMAIL
$ export GITHUB_TOKEN=<сгенирированный_токен> #initialize value GITHUB_TOKEN
$ alias edit=<nano|vi|vim|subl> #make an alias for the standard editor
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace #change the directory
$ source scripts/activate #read and execute commands from the file `scripts/activate`
```

```ShellSession
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https #change global configuration communication protocol for https 
```

```ShellSession
$ mkdir projects/lab02 && cd projects/lab02
$ git init #creates an empty Git repository by creating .git directory
Initialized empty Git repository in /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab02/.git/
$ git config --global user.name ${GITHUB_USERNAME}  #change global configuration user.name
$ git config --global user.email ${GITHUB_EMAIL}  #change global configuration user.email
# check your git global settings
$ git config -e --global #check global configurations
[user]
        name = puchkovki
        email = puchkov.k@phystech.edu
[core]
        editor = vim
[hub]
        protocol = https
~                
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git # add a new remote Git repository
$ git pull origin master #automatically fetch and then merge remote branch into our current branch
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/puchkovki/lab02
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
$ touch README.md #update the dates associated with the file README.md
$ git status  #check the commit status in the current branch
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md

nothing added to commit but untracked files present (use "git add" to track)
$ git add README.md #add changes
$ git commit -m "added README.md" #commit changes
[master 955665e] added README.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
$ git push origin master #push changes
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 281 bytes | 28.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/puchkovki/lab02.git
   7cbf7b5..955665e  master -> master
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
.idea/
```

```ShellSession
$ git pull origin master #download all changes from the master
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/puchkovki/lab02
 * branch            master     -> FETCH_HEAD
   955665e..eb76ccc  master     -> origin/master
Updating 955665e..eb76ccc
Fast-forward
 .gitignore | 4 ++++
 1 file changed, 4 insertions(+)
 create mode 100644 .gitignore
$ git log #show the commit logs
commit eb76ccc46e15f1ac2c74b1ef3df59ee0ed3aa5e9 (HEAD -> master, origin/master)
Author: Киря <44577888+puchkovki@users.noreply.github.com>
Date:   Thu Mar 12 15:42:11 2020 +0300

    Create  .gitignore

commit 955665ee0848fd0e28f7e6f35ffd2125c6cc0471
Author: puchkovki <puchkov.k@phystech.edu>
Date:   Thu Mar 12 15:38:50 2020 +0300

    added README.md

commit 7cbf7b5ed9a211cbe59af9b4fc630cf570b6a0b4
Author: Киря <44577888+puchkovki@users.noreply.github.com>
Date:   Thu Mar 12 15:36:59 2020 +0300

    Initial commit
```

```ShellSession
$ mkdir sources
$ mkdir include
$ mkdir examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```ShellSession
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```ShellSession
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```ShellSession
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```ShellSession
$ edit README.md
```

```ShellSession
$ git status #check the commit status in the current branch
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        examples/
        include/
        sources/

no changes added to commit (use "git add" and/or "git commit -a")
$ git add . #add all changes
$ git commit -m"added sources"
[master 69b5490] added sources
 5 files changed, 34 insertions(+)
 create mode 100644 examples/example1.cpp
 create mode 100644 examples/example2.cpp
 create mode 100644 include/print.hpp
 create mode 100644 sources/print.cpp
$ git push origin master
Username for 'https://github.com': puchkovki
Password for 'https://puchkovki@github.com':
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (10/10), 1.04 KiB | 39.00 KiB/s, done.
Total 10 (delta 0), reused 0 (delta 0)
To https://github.com/puchkovki/lab02.git
   eb76ccc..69b5490  master -> master
```

## Report

```ShellSession
$ cd ~/workspace/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2020 The ISC Authors
```
