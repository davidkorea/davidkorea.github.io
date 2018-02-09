---
layout: default
title: "Git push remote repo"
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
