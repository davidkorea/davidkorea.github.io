---
layout: post
title: "Git Push Remote Repository"
date: 2018-02-09
tags: git
---


> My own blog has been created in the past few days, so i'm planning to update my blog regularly from now on.
Also, i need to use github to store and manage my project that i'm doing these days and learn how to use git by the way. Even i started github last year, but, you know ...

# 1. Before push remote

> All the codes run in default path.

# 1.1 SSH

- check whether you have ssh key in you mac already or not.

  $ ```more ~/.ssh/id_rsa.pub```

- if not

  $ ```ssh-keygen```, press enter, enter, enter

- copy contents

  $ ```more ~/.ssh/id_rsa.pub```

- go to github settings - SSH and GPG keys

# 1.2 global config

$ ```git config --global user.name "davidkorea"```

$ ```git config --global user.email "*****@naver.com"```

# 2. Create remote repo on github

- Create a repo that has the same name of the one in local
- Use the command given on github to push

  $ ```git remote add origin git@github.com:davidkorea/180205.git```

  $ ```git push -u origin master```

> When create a repo on github, don't check 'Initialize this repository with a README'. Or you couldn't push to remote cuz your remote repo isn't blank. Your local repo doesn't have this README file.

# 3. Conflict Error

```Python
PS C:\Users\xerox\Desktop\blogdata> git push origin

To https://github.com/davidkorea/blogdata.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/davidkorea/blogdata.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing

hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
ANSWER

```Python
PS C:\Users\xerox\Desktop\blogdata> git pull

remote: Counting objects: 4, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From https://github.com/davidkorea/blogdata
   529a2ff..3e1790e  master     -> origin/master

Merge made by the 'recursive' strategy.
 20180211/2018-02-11-request-get.md | 6 +++++-
 1 file changed, 5 insertions(+), 1 deletion(-)
 
PS C:\Users\xerox\Desktop\blogdata> git push origin
```

