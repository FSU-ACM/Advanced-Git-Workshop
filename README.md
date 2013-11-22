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
###PURGING
The most useful tool when purging data from a repository is: `git filter-branch`
<br />options:
- `--prune-empty` removes commits that become empty (i.e., do not change the tree) as a result of the filter operation. In the typical case, this option produces a cleaner history.
- `-d` names a temporary directory that does not yet exist to use for building the filtered history.
- `--index-filter` is the main event and runs against the index at each step in the history. You want to remove oops.iso wherever it is found, but is isn’t present in all commits. The command git `rm --cached -f --ignore-unmatch filename` deletes the file when it is present and does not fail otherwise.
- `--tag-name-filter` describes how to rewrite tag names. A filter of cat is the identity operation. Your repository, like the sample above, may not have any tags, but I included this option for full generality.
- `--` specifies the end of options to git filter-branch
- `--all` following `--` is shorthand for all refs. Your repository, like the sample above, may have only one ref (master), but we included this option for full generality.

Although it is possible to use the interactive rebase tool for this as well. To completely remove a commit from history we simply delete it in the interactive rebase. Any files and modifications added there will be lost, mods get absorbed into the next change.

You will want to change the previous commit to edit which will drop you to that change when you exit rebase.<br />
Here we want to remove are files from the repo with `git rm --cached filename`<br />
and then amend the commit with `git commit --amend -C HEAD`.<br />
finally continue completes the rebase `git rebase --continue`

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

#####Many ways to do this
1. create a new branch, pull down contributor's code, merge back into master
<br />or
2. `git am` The git am command allows yo to apply a diff to your working directory.
```
curl http://github.com/other-user-name/forked-repo/pull/1.patch | git am
```
3. curl the patch into a temp directory and then use git apply


-----------
###TAGGING
Git has the ability to tag specific points in history as being important. Generally, people use this functionality to mark release points (v1.0, and so on).

To list tags: `git tag`<br />
or for a specific tag: `git tag -l 'v1.1.*'` 

3 types:
 1. Annotated	`git tag -a v.1.0 -m 'version 1; SHIP IT'`
 2. Signed		`git tag -s v1.1 -m 'signed version: BUG FIXES'`
 3. Lightweight	`git tag v1.2-lw` **do not use** `-a`, `-m`, or `-s`


-----------
###SUB-MODULES
So simple! `git submodule add ...` will automatically create a file: `.gitmodules`

Many ways to do this:
 1. from remote repo `git submodule add git://github.com/user/repo.git dir-name`
	clones and adds submodule to dir name specified
	`cat .gitmodules`
	If you have multiple submodules, you’ll have multiple entries in this file.

 2. clone the repository into your repositories directory
 	add/modify the `.gitmodules` file yourself
 	 1. `git submodule init`
 	 2. `git submodule update` 

 ```
 [submodule "name"]
      path = path/to/module/
      url = git://github.com/user/name.git
 ```