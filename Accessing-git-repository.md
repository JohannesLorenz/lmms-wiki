### Anonymous Git

Assuming you have git installed, you first have to get a copy of the Git
repository:

	git clone https://github.com/LMMS/lmms.git

This will clone the master branch, which can be unstable and have bugs.

If you wish to switch to the current stable branch (for the purpose of compiling LMMS by yourself):

	cd lmms
	git checkout stable-1.1

For instructions on how to compile LMMS, visit [[Compiling LMMS]].

If you want to update your copy simply type

	git remote update
	git pull

inside lmms-directory. The first step usually can be omitted.

### Browse Git code

For browsing the Git repository easily you can also use the [Git Web
interface](https://github.com/LMMS/lmms).