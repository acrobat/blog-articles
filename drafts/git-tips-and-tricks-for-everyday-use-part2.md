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

force push with lease

## Partial reverts

partial reverts (http://stackoverflow.com/questions/4795600/reverting-part-of-a-commit-with-git)

## Easily switch to previous branch

Just like on the commandline you can switch to the previous directory by using `cd -`, you can use this same trick to
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

```bash
git rebase --onto
```

## Git standup alias

A lot of teams use standups to recap the work each team member did the last day or so. When you work on a lot of
different issues it's hard to remember what you exactly did. Therefor this alias makes it easy to retreive all the work
done in the past day in all branches.

```bash
git config --global --add alias.standup '!git log --branches --remotes --tags --no-merges --author=\"`git config user.name`\" --since="$(if [[ "Mon" == "$(date +%a)" ]]; then echo \"last friday\"; else echo \"yesterday\"; fi)" --format=%s'

# Now you can run the following command

git standup
```

## Bonus: lazy standup

This is a fun little extension to the `git standup` alias. We can use the built-in text-to-speech tool so you
don't even have to speak during the standup :)

```bash
git config --global --add alias.lazy-standup '!git standup-simple | spd-say -e'
```
