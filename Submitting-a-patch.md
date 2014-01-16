The common way to contribute code to a project on GitHub is by forking it and creating a pull request after the changes are done. This is the workflow we recommend. 

If you don't have a GitHub account and don't want to register, you may just [create a patch file](#Patch) and send it to us.

## Using GitHub
### Fork the official LMMS repository
For instructions on how to fork LMMS, see the GitHub help article [Fork A Repo](https://help.github.com/articles/fork-a-repo). Having forked LMMS and cloned it to your hard drive, you can start working on the code.

### Pushing your changes to GitHub
After you made your changes and implemented your ideas, it's about time to push them to your git repository.  
For doing this, you may use the command line, as it's explained below, or use `git gui` to open a graphical user interface, which can make things a lot easier if you're uncomfortable with the command line.

#### Reviewing your changes
You can list all changed files by running
```
$ git status
```
inside the source root directory. A more detailed view on what exactly has been changed is provided by
```
$ git diff
```

#### Staging and committing
The next step is to stage the changes you've made. To stage a file, run
```
$ git add FILENAME
```

Then, you have to commit your staged changes. To do so, run
```
$ git commit
```
You'll be asked to enter a commit message to summarize what you did. The staging area can also be skipped by just typing
```
$ git commit -a
```
This way all changes you made will automatically be staged and committed.

### Contributing your changes
In order for your changes to be merged into the official repository, you should create a pull request.
You can find instructions on this topic in GitHubs article [Using Pull Requests](https://help.github.com/articles/using-pull-requests).

## Using only a patch file <a name="Patch"></a>
To have your changes applied without using GitHub, create a patch file containing your changes by running
```
$ git diff > my-patch-for-lmms.diff
```
in the source root directory. Now you have a patch file we can easily apply. Just [contact us](https://github.com/LMMS/lmms/wiki#contact-us) about it.