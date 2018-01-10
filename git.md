# Git #
You add external projects as submodules with 

	git submodule add git://github.com/... [SUBMODULENAME]


### Single branch pull 
	git clone -b mybranch --single-branch git://	sub.domain.com/repo.git
	
In general:

git clone -b <branch> <remote_repo>

Example:

git clone -b develop git@github.com:user/myproject.git	
				
------	
# Submodules
### Issues with Submodules
Using submodules isn’t without hiccups, however. First, you must be relatively careful when working in the submodule directory. When you run **git submodule update**, it checks out the specific version of the project, but not within a branch. This is called having a detached HEAD — it means the HEAD file points directly to a commit, not to a symbolic reference. The issue is that you generally don’t want to work in a detached HEAD environment, because it’s easy to lose changes. If you do an initial **submodule update**, commit in that submodule directory without creating a branch to work in, and then run **git submodule update** again from the superproject without committing in the meantime, Git will overwrite your changes without telling you. Technically you won’t lose the work, but you won’t have a branch pointing to it, so it will be somewhat difficult to retrieve.
	
# Removing Submodules

What happens if we need to remove a submodule? Maybe I made a mistake. It could also be that the design of the project has changed, and the submodules need to change with it. No problem, I’ll simply run “git submodule rm [submodule path]” and everything will be great, right?

	[user@office SampleTheme]$ git submodule rm lib/billboard
	error: pathspec 'rm' did not match any file(s) known to git.
	Did you forget to 'git add'?
 	b8ff8f68eb56938b9b4bf993619218fa848c5848 lib/billboard (1.2.25)
 
Unfortunately, this is wrong. Git does not have a built in way to remove submodules. Hopefully this will be resolved in the future, because we now have to do submodule removal manually.

Sticking with the example, we’ll remove the lib/billboard module from SampleTheme. All the instructions will be run from the working directory of the SampleTheme repository. In order, we need to do the following:

1. Remove the submodule’s entry in the .gitmodules file.Since lib/billboard is the only submodule in the SampleTheme repository, we can simply remove the file entirely by running “git rm lib/billboard”.If lib/billboard isn’t the only submodule, the .gitmodules file will have to be modified by hand. Open it up in vi, or your favorite text editory, and remove the three lines relevant to the submodule being removed. In this case, these lines will be removed:

		[submodule "lib/billboard"]
		path = lib/billboard
		url = git@mygithost:billboard

2. Remove the submodule’s entry in the .git/config. This step isn’t strictly necessary, but it does keep your config file tidy and will help prevent problems in the future.The submodule’s entry in .git/config will only be present if you’ve run “git submodule init” on the repository. If you haven’t, you can skip this step.In this example, the following lines will be removed:

		[submodule "billboard"]
		url = git@mygithost:billboard

3. Remove the path created for the submodule. This one is easy. Simply run **“git rm –cached [plugin path]“**. In this example, I will run **“git rm –cached lib/billboard“**.As I’ve seen noted elsewhere, do not put a trailing slash as the command will fail. For example, if I run **“git rm –cached lib/billboard/“**, I get an error: **“fatal: pathspec ‘lib/billboard/’ did not match any files“**.