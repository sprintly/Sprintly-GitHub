#Sprintly-GitHub

[Sprint.ly](http://sprint.ly/ 'Sprint.ly') is a great tool for managing work items; [GitHub](http://github.com 'GitHub') is a great tool for managing your code. The tools in this repository will help you take advantage of the integration with GitHub that Sprint.ly offers without drawing your attention away from the terminal. Use the `sprintly` command line tool to get a list of all items assigned to you. Install the `commit-msg` hook to facilitate Sprint.ly's GitHub integration.

#How to use `sprintly`

	Usage: sprintly [options]

	By default, your sprintly items will be shown.
	
	Options:
	  -h, --help      show this help message and exit
	  --install       install or update this tool
	  --install-hook  install commit-msg hook in current directory (must be a git
					  repository)
	  --update-hook   update commit-msg hook in all repositories
	  
#Installing `sprintly`

The `sprintly` tool can now install itself. Follow the instructions below to get started:

	// download the latest version of the tool
	curl -O https://raw.github.com/nextbigsoundinc/Sprintly-GitHub/master/sprintly
	
	// install
	sudo python sprintly --install
	
	// clean up
	rm sprintly
	
Once the installation is complete, run `sprintly` from any directory to get started. If you have never used the tool before, it will walk you through adding your Sprint.ly credentials:

	$ sprintly
	Config not found. Creating config...
	Enter sprint.ly account (email): user@company.com
	Enter sprint.ly API Key: 3536bae19bacd16831fb5100b13e34d2
	Configuration successfully created.

To update `sprintly`, simply type `sudo sprintly --install`. Note: if you are updating `sprintly`, you may see a warning:

	$ sudo sprintly --install
	Downloading latest version of sprintly tool from GitHub...
	A file already exists at /usr/bin/sprintly. Overwrite file? 
	
Entering `y` will overwrite the old installation with the latest version. Note: if you have another tool installed at `/usr/bin/sprintly`, enter `n` and manually install it under a different name.

#Installing the `commit-msg` hook.

The `sprintly` tool can install the hook for you. Simply navigate to a git repository and run:

	$ sprintly --install-hook
	Downloading latest version of commit-msg hook from GitHub...
	Hook was updated at <home directory>/.sprintly/commit-msg
	Creating symlink...
	Hook was installed at <repository>/.git/hooks/commit-msg
	
You can update the hook periodically by running:

	$ sprintly --update-hook
	Downloading latest version of commit-msg hook from GitHub...
	Hook was updated at /Users/Walter/.sprintly/commit-msg
	
This will automatically update the hook for all repositories in which you have the hook installed.
	
#Sample Output

Let's run through a few examples of how to take advantage of this tool.

Type `sprintly` at a command prompt to see a list of your current Sprint.ly items:

	Product: Example Company (https://sprint.ly/product/#/)
		#1: As a developer, I want to open source our Sprintly-Github tools so that...  
			#5: Publish it.
			#4: Write README
		#2: As a developer, I want a better set of unit tests so that changes to our...
			#3: Add tests to widget creation page

With the `commit-msg` hook installed, whenever a commit is made and pushed, a comment will be automatically published on the corresponding Sprint.ly item. Head to Sprint.ly for more [details](http://support.sprint.ly/kb/integration/available-scmvcs-commands 'Sprint.ly SCM/VCS Commands').
	
	$ git commit -m "Normal commit message here."
	Product: Example Company (https://sprint.ly/product/#/)
			#1: As a developer, I want to open source our Sprintly-Github tools so that...  
				#5: Publish it.
				#4: Write README
			#2: As a developer, I want a better set of unit tests so that changes to our...
				#3: Add tests to widget creation page
	#0 - Proceed without Sprint.ly item number.
	Enter 1 or more item numbers separated by a space: 4
	[master 1e71283] References #4. Normal commit message here.
	 1 files changed, 1 insertions(+), 1 deletions(-)