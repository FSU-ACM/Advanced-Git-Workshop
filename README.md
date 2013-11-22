Advanced-Git-Workshop
=====================

###GLOBAL CONFIGURATIONS

####set global gitignore
```
$ vim ~/.gitignore_global
$ git config --global core.excludesfile '~/.gitignore_global'
```

####set global configurations
```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

####set editor and diff tool
```
$ git config --global core.editor emacs/vim/nano/etc
$ git config --global merge.tool vimdiff
```

####custom commands
```
$ git config --global alias.today '!git log --since=midnight --author="$(git config user.name)" --oneline'
```

####check settings
```
$ git config --list
```


-----------
###ADVANCED LISTING
advanced logging

	⁃ alias.le=log --oneline --decorate
	⁃ alias.ll=log --pretty=format:%C(yellow)%h%Cred%d\ %Creset%s%Cblue\ [%cn] --decorate --numstat
	⁃	alias.ls=log --pretty=format:%C(yellow)%h%Cred%d\ %Creset%s%Cblue\ [%cn] --decorate
	⁃	alias.lds=log --pretty=format:%C(yellow)%h\ %ad%Cred%d\ %Creset%s%Cblue\ [%cn] --decorate --date=short
	⁃	alias.ld=log --pretty=format:%C(yellow)%h\ %ad%Cred%d\ %Creset%s%Cblue\ [%cn] --decorate --date=relative

**a better log** - [pimping your git log](http://www.jukie.net/bart/blog/pimping-out-git-log)
`$ git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"`



-----------
###REBASING AND ROLLING BACK
####Removing sensitive information(purging a file)
**Once any sensitive information has been committed you should consider it compromised and change it immediately.**

Once it is changed you want to remove the file from your repo entirely!

command:
```
$ git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch path/to/filename’ \
  --prune-empty --tag-name-filter cat -- --all
 ```

NOTE: `git rm --cached filename` only removes a file from the repo history
	  where `git rm filename` will remove and delete it

then add that file to your .gitignore so that you never accidentally commit it again
```
$ echo “path/to/filename” >> .gitignore
```

-----------
###STASHING


-----------
###PULL REQUESTS
A pull request is often made on a repository when a contributor has forked it, made some modifications and is now ready to hove those changes reviewed and merged back in to the original repo.

#####Two common schools of thought:
**Fork & Pull Model** lets anyone fork an existing repository and push changes to their personal fork without requiring access be granted to the source repository. The changes must then be pulled into the source repository by the project maintainer. This model reduces the amount of friction for new contributors and is popular with open source projects because it allows people to work independently without upfront coordination.<br />
**Shared Repository Model** is more prevalent with small teams and organizations collaborating on private projects. Everyone is granted push access to a single shared repository and topic branches are used to isolate changes.
Pull requests are especially useful in the Fork & Pull Model because they provide a way to notify project maintainers about changes in your fork. However, they're also useful in the Shared Repository Model where they're used to initiate code review and general discussion about a set of changes before being merged into a mainline branch.
(github)x


You can handle pull requests from either the web interface or the command line.

#####WEB INTERFACE
1. “Pull Requests”
2. Follow the steps to apply the changes to a specific branch.

#####COMMAND LINE
Every pull request comes with a corresponding patch URL. You create this URL by adding “.patch” to the address.
http://github.com/user/repo-name/pull/pull-id.patch

This also works with regular commits:
http://github.com/user/repo-name/commit/commit-id.patch

#####2 ways to do this
1. create a new branch, pull down contributor's code, merge back into master
<br />or
2. `git am` The git am command allows you apply a diff to your working directory.


