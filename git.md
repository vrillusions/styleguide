# Git

- [Basic work flow](#basic-work-flow)
- [Initial commit](#initial-commit)
- [Initial repo layout](#initial-repo-layout)
- [Logically sized commits](#logically-sized-commits)
- [Separate commit for format changes](#separate-commit-for-format-changes)
- [Commit message format](#commit-message-format)
- [Rewriting history](#rewriting-history)
- [Pull requests](#pull-requests)

## Basic work flow

- Development usually happens on a feature branch. Typically this will only ever live as a local branch but sometimes is pushed to remote.
- Feature branches are assumed to be volatile, meaning commit history within that branch may be altered and force push back to server. Exceptions include long life branches like a stable or release-1.0 branch.
- Rebase from remote/master to local master, rebase master to feature branch, merge from feature branch to master. This reduces all those "merge head of master" messages.

### Merging a feature branch process

```bash
git checkout master
git pull --rebase
git checkout feature1
git rebase master
git checkout master
git merge feature1 --ff-only
git push
```

This process updates local master branch via rebase (since any local commits that need to rebase shouldn't have been pushed to server). Then rebase master in to feature1 (this means feature1's commit history will be changed, 99% of the time these are local branches though). And then merge feature1 in to master. At this point the merge should be a fast forward so no merge commit will be needed.

## Initial commit

The initial commit to a project should be the bare minimum. Reason for this is git considers the initial commit different.  This makes it very difficult to make any changes to it. For example you have a project and the initial commit is the entire project. You then realize that you commit some files with passwords in them you'd like to remove.  Removing those from the initial commit is very difficult.  This is because the way git rewrites a commit is checkout that commit's parent commit and then apply a new commit with the specified changes.  The initial commit doesn't have a parent commit. As you can guess I had first hand experience with an issue like this. It may have been updated now but it doesn't hurt anything to import everything in the second commit.

An example of a good initialization of a git project:

```bash
git init .
echo "# Blank gitignore" >.gitignore
git add .gitignore
git commit -m "Initial commit."
```

You could also just add a readme file but again it should be blank. So `touch README.mkd`. The reason the single line is added above is for clarity in case nothing is ever added to it. It's assumed the readme will be changed in the very next commit to something useful.

## Initial repo layout

See my [template repo](https://github.com/vrillusions/template-repo) for what the initial repo looks like.

## Logically sized commits

Each commit should contain a logical amount of work.  Work shouldn't be piled up for days and then all added in a single commit.  Commands like cherry-pick work at the commit level.  Trying to cherry-pick a large commit would be quite annoying.  Typical fix for such an issue is to just manually copy over the changes you need and just reference the commit you took it from.

## Separate commit for format changes

When reformatting code such as moving functions around, adjusting indentation or new lines, and renaming files create a separate format only commit. As an example say you want change indent style from tabs to spaces.  That's going to change practically every line of code.  Mixing changes in with that would make it hard to diagnose bugs introduced with that commit.  Even when doing the indents you notice a piece of code is wrong. Don't change it in that commit. Instead make a note of it and do one commit with the reformatted changes and another commit with the code changes. Generally reformatting should be avoided as it does still add this commit with a lot of changes that may have changes intermixed with it.

Going along with this, don't ever rebase a formatting change commit with a code change commit.  This defeats the whole purpose the two were separate to begin with.  If several formatting commits were made you can merge all those into one formatting commit but if there's a code change commit in the middle then you can't.

## Commit message format

Overall formatting follows the [50/72](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) format. This means the initial line, commonly called the subject, should be 50 characters or less.  This isn't a hard number to me so if it's 55 or 60 characters long, that's fine.  While it should be capitalized, don't end with punctuation.  Below that is a blank line and then follows a longer description of the commit. At the bottom can contain keywords that are of use to various ticket managers and such.  I'll deviate from this in that I use an asterisk for lists and don't have a blank line between each one.  The body should also have hard wraps at around 73 characters. Several git commands don't wrap text around so to see a message you have to scroll sideways.

Format commit messages per [conventional commits](https://www.conventionalcommits.org) spec.  This is a summary:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

The spec places emphasis on use of `fix:` and `feat:` and `BREAKING CHANGE` types.  My implementation is more loose.  The `type` can be anything that defines what the commit covers.  An example from my [dotfiles](https://github.com/vrillusions/dotfiles) repo:

```
vim: Add bracketed-paste plugin
```

The correct format is `feat(vim): Add bracketed-paste plugin` as per spec.  As long as it's short and too the point it's good.

Commit messages should be in the present tense. So "Fix contact email" and not "Fixed contact email" A way to imagine this is the commit should say what will happen when update to that commit. Reason for this is all of git's commands like merge and revert use present tense.

Messages should focus more on the why and not the how. How you did something can be understood from the commit itself. The commit itself may not give a reason to why that change was made. The above example of "Fix contact email" may still be enough information.  For example if the commit itself changes the email from `jdoe@example.com` to `jdoe@NOSPAMexample.com` then it can be inferred that poor jdoe was getting a lot of spam messages and attempted to obfuscate the email.

References can be placed at the bottom of the commit message.  For example if you want to have this close a ticket in github you can add `Closes: gh-3` to tell github add a link to ticket id 3 and then close it. Alternatively `References: gh-3` would just add a link to that commit to ticket id 3 but doesn't close it.

## Rewriting history

If the revision has been pushed to a central repo or shared with someone else, don't touch it. Sometimes it may be acceptable to rebase a feature branch in case there are several minor changes that can be squashed into a single commit.  To do a rebase on only the commits that haven't been pushed to origin can be accomplished with `git rebase -i origin/master`.

Tags that have been published should NEVER be modified.  While moving the commit a tag points to is a valid operation it's generally assumed that a tag identifies a specific commit and doesn't.  If you see a use case for a tag that changes what commit it points to, you should be using branches.  The old tag may be removed but keep in mind that it may have been cached or pulled from someone in the interim. They will see that tag even after it's deleted on origin.

## Pull requests

For contributing to a project will usually be in the form of a pull request. Basic workflow for this:

```bash
# fork project on github
git clone git@github.com:MyUser/ProjectName
cd ProjectName
git remote add upstream https://github.com/OriginalUser/ProjectName
git fetch upstream
git checkout -b fix_whatever
# make changes and commit it
git push origin fix_whatever
# submit pull request
```

If it takes a while for the author to get to it you may be asked to rebase/merge your changes with current master:

```bash
git fetch upstream
# if you use merge or rebase here go by that developer's polices.
git checkout feature1
git rebase upstream/master
git push origin fix_whatever
# At this point you can leave a comment that it's been done
git checkout master
git merge upstream/master
git push
```

The key thing here is that fix branch should be as specific as possible. Pull requests are all or nothing and it's annoying if like 90% of the changes fix whatever and then the other 10% is completely unrelated. Once you confirmed that the pull request is in you can delete the `fix_whatever` branch from your repo and probably even remove the fork if you don't plan to make any more changes.
