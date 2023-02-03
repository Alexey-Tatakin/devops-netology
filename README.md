В репозиторий добавлен файл исключений для проектов Terraform <br/>
Который находится в terraform/.gitignore <br/>
В нем исключаются: <br/>
+ Скрытая папка .terraform и все что в ней находится <br/>
+ Служебные файлы tfstate состояния <br/>
+ Файлы crash логов <br/>
+ Файлы переменных содержащие пароли, ключи и т.д. tfvars <br/>
+ Файлы временно содержащие локальные переопределения override.tf а так же файлы конфигурации .terraform.rc и terraform.rc

## ДЗ для Ветвления в Git
**Промежуточный этап**
```
~/Netology.ru/devops-netology on  main ⌚ 17:34:25
$ git log --oneline --graph --all
* f15ad0d (origin/git-rebase, git-rebase) git-rebase 2
* a53b973 git-rebase 1
| * 9bc3f8c (HEAD -> main, origin/main, origin/HEAD) Change rebase.sh in main
|/  
| * ef6af1a (origin/git-merge, git-merge) merge: use shift
| * c6a9674 merge: @ instead *
|/  
* ebfefc1 prepare for merge and rebase
* 2ec95a1 (tag: v0.1, tag: v0.0) git rm file
* 2147374 Edit README.md for MD standart
* c49fd04 Moved and deleted
* 6de53fb Prepare to delete and move
* 8fabc17 Added gitignore
* da218e1 First commit
* c92ae73 Initial commit
```
**Merge**
```
~/Netology.ru/devops-netology on  main ⌚ 17:34:33
$ git merge git-merge
Merge made by the 'recursive' strategy.
 branching/merge.sh | 5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)

 ~/Netology.ru/devops-netology on  main ⌚ 17:39:19
$ git push
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 368 bytes | 368.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/Alexey-Tatakin/devops-netology.git
   9bc3f8c..310debe  main -> main
```
**Результат**
```
~/Netology.ru/devops-netology on  main ⌚ 17:40:10
$ git log --oneline --graph --all
*   310debe (HEAD -> main, origin/main, origin/HEAD) Merge branch 'git-merge'
|\  
| * ef6af1a (origin/git-merge, git-merge) merge: use shift
| * c6a9674 merge: @ instead *
* | 9bc3f8c Change rebase.sh in main
|/  
| * f15ad0d (origin/git-rebase, git-rebase) git-rebase 2
| * a53b973 git-rebase 1
|/  
* ebfefc1 prepare for merge and rebase
* 2ec95a1 (tag: v0.1, tag: v0.0) git rm file
* 2147374 Edit README.md for MD standart
* c49fd04 Moved and deleted
* 6de53fb Prepare to delete and move
* 8fabc17 Added gitignore
* da218e1 First commit
* c92ae73 Initial commit
```
**Rebase**
```
~/Netology.ru/devops-netology on  git-rebase ⌚ 17:44:35
$ git rebase -i main
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh
error: could not apply a53b973... git-rebase 1
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply a53b973... git-rebase 1

~/Netology.ru/devops-netology/branching on  310debe! ⌚ 17:50:29
$ git rebase --continue
[detached HEAD 54cea2f] git-rebase 1
 1 file changed, 1 insertion(+), 1 deletion(-)
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh
error: could not apply f15ad0d... git-rebase 2
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply f15ad0d... git-rebase 2

~/Netology.ru/devops-netology/branching on  54cea2f! ⌚ 17:52:50
$ git rebase --continue
[detached HEAD 2b83eb5] git-rebase 1
 Date: Fri Feb 3 17:28:11 2023 +0300
 1 file changed, 2 insertions(+), 2 deletions(-)
Successfully rebased and updated refs/heads/git-rebase.
```
**Push - error**
```
~/Netology.ru/devops-netology/branching on  git-rebase ⌚ 17:53:17
$ git push -u origin git-rebase
To https://github.com/Alexey-Tatakin/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'https://github.com/Alexey-Tatakin/devops-netology.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

~/Netology.ru/devops-netology/branching on  git-rebase ⌚ 17:55:36
$ git push -u origin git-rebase -f
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 389 bytes | 389.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/Alexey-Tatakin/devops-netology.git
 + f15ad0d...2b83eb5 git-rebase -> git-rebase (forced update)
Branch 'git-rebase' set up to track remote branch 'git-rebase' from 'origin'.
```
**Final merge git-rebase**
```
~/Netology.ru/devops-netology/branching on  main ⌚ 17:59:05
$ git merge git-rebase
Updating 310debe..2b83eb5
Fast-forward
 branching/rebase.sh | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

~/Netology.ru/devops-netology/branching on  main ⌚ 17:59:56
$ git push
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Alexey-Tatakin/devops-netology.git
   310debe..2b83eb5  main -> main
```
**Status and log**
```
~/Netology.ru/devops-netology/branching on  main ⌚ 18:00:16
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

~/Netology.ru/devops-netology/branching on  main ⌚ 18:02:32
$ git log --oneline --graph --all
* 2b83eb5 (HEAD -> main, origin/main, origin/git-rebase, origin/HEAD, git-rebase) git-rebase 1
*   310debe Merge branch 'git-merge'
|\  
| * ef6af1a (origin/git-merge, git-merge) merge: use shift
| * c6a9674 merge: @ instead *
* | 9bc3f8c Change rebase.sh in main
|/  
* ebfefc1 prepare for merge and rebase
* 2ec95a1 (tag: v0.1, tag: v0.0) git rm file
* 2147374 Edit README.md for MD standart
* c49fd04 Moved and deleted
* 6de53fb Prepare to delete and move
* 8fabc17 Added gitignore
* da218e1 First commit
* c92ae73 Initial commit
```