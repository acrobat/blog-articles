[//]: # (TITLE: Git tips & tricks for everyday use - part 2)
[//]: # (DATE: 2000-00-00T00:00:00+01:00)
[//]: # (TAGS: git)

The [first part of my git tips & tricks blogpost](https://jeroenthora.be/post/git-tips-and-tricks-for-everyday-use) got 
some really good responses. So I think the time is right to compose a second list of tips & tricks.

## (Force) push the current branch 

While force pushing you will always have to check you push the right branch. Otherwise it is possible you accidentally 
force push another branch, for example the master branch. If that branch was behind on the origin master you will override
all changes done by others. To avoid this you can just use `HEAD` as the branch indicator:

```bash
git push origin HEAD -f 
```

## Force push with lease

To avoid overriding changes on the remote branch because you force push, git has a built-in safety check. 
You can use the `--force-with-lease` flag, this way git will check that the remote branch has not been 
updated since you last fetched it.

```bash
$ git push origin HEAD --force-with-lease
To /tmp/test-repo
 ! [rejected]        dev -> dev (stale info)
error: failed to push some refs to '/tmp/test-repo'
```

## Partial reverts

partial reverts (http://stackoverflow.com/questions/4795600/reverting-part-of-a-commit-with-git)

## Easily switch to previous branch

Just like on the command line you can switch to the previous directory by using `cd -`, you can use this same trick to
switch easily to the previous branch.

```bash
git checkout feature-branch1

git status
    On branch feature-branch1
    
git checkout master

git status
    On branch master
    
git checkout -

git status
    On branch feature-branch1
```

## Gitignore templates

Github has provided a repository with a whole bunch of gitignore templates to get you started on a new project.
For example when you start a symfony project from scratch, just run the following command and you will have a
correct .gitignore to get started.

```bash
wget https://raw.githubusercontent.com/github/gitignore/master/Symfony.gitignore -O .gitignore
```

To find all available gitignore templates visit the [github/gitignore](https://github.com/github/gitignore) repository.

## Change the base branch

When you accidentally create a feature branch from the incorrect branch. For example you started a branch from a feature 
branch instead of the master branch. By using `rebase --onto` you can rebase the feature branch from the correct branch.
The syntax is like `rebase *original starting branch* --onto *correct branch*`.

```bash
git rebase feature-1 --onto master
```

## Git standup alias

A lot of teams use standups to recap the work each team member did the last day or so. When you work on a lot of
different issues it's hard to remember what you exactly did in that timespan. Therefor this alias makes it easy to retreive all the work
done in the past day in all branches.

```bash
git config --global --add alias.standup '!git log --branches --remotes --tags --no-merges --author=\"`git config user.name`\" --since="$(if [[ "Mon" == "$(date +%a)" ]]; then echo \"last friday\"; else echo \"yesterday\"; fi)" --format=%s'

# Now you can run the following command and get all commits you did yesterday

git standup
```

## Bonus: lazy standup

This is a fun little extension to the `git standup` alias. We can use the built-in text-to-speech tool so you
don't even have to speak during the standup :)

```bash
git config --global --add alias.lazy-standup '!git standup-simple | spd-say -e'

# Just run git lazy-standup and just sit back and relax :)
```

If you have other cool git tips and tricks or you like any of these, let it know in the comments below!
