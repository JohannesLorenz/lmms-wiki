Assuming you've already cloned your branch to your computer using:

```bash
$ git clone -b master http://github.com/my_personal_branch/lmms
$ cd lmms
```

Add an upstream remote by doing:

```bash
$ git remote add upstream https://github.com/LMMS/lmms.git
```

To sync a personal `master` based branch with the upstream `master` branch:

```bash
$ git pull --rebase upstream master
```

This simply pulls everything from the upstream master branch and if there
are changes in the currently selected local branch, they get applied on
top of upstream. Bear in mind, if you do that after you've already
pushed to your github repo, you'll have to do a forced push next time
`(push -f)` or git will complain about non-fast-forwards.

```bash
$ git push
```

The script for `stable-x.x` is the same, just replace `master` with
`stable-x.x`. The only thing to be careful about here is to always run the
correct branch name or you'll risk totally messing up your local branch...
but as long as you keep track of where your changes are headed towards,
you should be fine.

*Adapted from a conversation with Vesa (@diizy) :jack_o_lantern:*
