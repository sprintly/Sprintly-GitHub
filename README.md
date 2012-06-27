#Sprintly-GitHub

[Sprint.ly](http://sprint.ly/ 'Sprint.ly') is a great tool for managing work items; [GitHub](http://github.com 'GitHub') is a great tool for managing your code. The tools in this repository will help you take advantage of the integration with GitHub that Sprint.ly offers without drawing your attention away from the terminal. Use the `sprintly` command line tool to get a list of all items assigned to you. Install the `commit-msg` hook to facilitate Sprint.ly's GitHub integration.

#How to use `sprintly`

	Usage: sprintly [options]

	By default, your sprintly items will be shown.

	Options:
	  -h, --help        show this help message and exit
	  --install         install this tool
	  --update          update this tool
	  --install-hook    install commit-msg hook in current directory (must be a
						git repository)
	  --uninstall-hook  uninstall commit-msg hook in current directory (must be a
						git repository)
	  --update-config   edit configuration

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
	Creating config...
	Enter sprint.ly username (email): user@company.com
	Enter sprint.ly API Key: 3536bae19bacd16831fb5100b13e34d2
	Enter default sprint.ly product id (117 - Company, 129 - Secret): 117
	Configuration successfully created.

*Note: (1) your Sprint.ly API Key can be found [here](https://sprint.ly/account/profile/). (2) you will only be asked to enter a sprint.ly product id if you have more than one product.*

To update `sprintly`, type `sudo sprintly --update`. When updating, you will see a warning:

	$ sudo sprintly --update
	Downloading latest version of sprintly tool from GitHub...
	A file already exists at /usr/local/bin/sprintly. Overwrite file?

Entering `y` will overwrite the old installation with the latest version. *Note: if you have another tool installed at `/usr/local/bin/sprintly`, enter `n` and manually install it under a different name.*

#GitHub Integration: Installing the `commit-msg` hook.

The `sprintly` tool can install the hook for you. Navigate to a git repository and run:

	$ sprintly --install-hook
	Creating symlink...
	Hook was installed at <repository>/.git/hooks/commit-msg

**Important: you MUST install this manually in every git repository. This is a limitation of the way git implements hooks. Don't blame me!**

Tip: make sure that your git user.email matches your Sprint.ly username, or the hook won't work. If this happens, you will see the following message:

	$ sprintly --install-hook
	Creating symlink...
	Hook was installed at <repository>/.git/hooks/commit-msg
	WARNING: Your git email (user@site.org) does not match your sprint.ly username (user@company.com)
	WARNING: Don't worry - there is an easy fix. Simply run one of the following:
		'git config --global user.email user@company.com' (all repos)
		'git config user.email user@company.com' (this repo only)

*Note: the hook installed is actually a symbolic link to a shared copy of the hook found at /usr/local/share/sprintly/commit-msg. By doing this, the hook can be easily updated for all users and all repositories by calling `sprintly --update`.*

###Uninstalling the `commit-msg` hook.

The `sprintly` tool can uninstall the hook for you as well. Navigate to the git repository in question and run:

	$ sprintly --uninstall-hook
	Hook has been uninstalled.

#Changing the Configuration

If, at some point, you wish to change your configuration (you get a new username, API key, or wish to change the default product), type `sprintly --update-config`. Pro tip: you don't have to re-type everything just to change your API Key. Simply press enter to keep the old version of any individual item:

	$ sprintly --update-config
	Updating config... Press enter to accept default value shown in brackets.
	Enter sprint.ly username (email) [user@company.com]:
	Enter sprint.ly API Key [3536bae19bacd16831fb5100b13e34d2]: 5c11931f26b4c5f6a435983d1a734839
	Enter default sprint.ly product id (117 - Company, 129 - Secret):
	Configuration successfully updated.

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

To save time, include an item number at the beginning of your commit to automatically reference that item. You won't be prompted to select an item number if you go this route:

	$ git commit -m "#42 Adding README"
	[master 555a912] References #42 Adding README
	 0 files changed, 0 insertions(+), 0 deletions(-)
	 create mode 100644 README

Or include Sprint.ly keywords followed by an item number anywhere in your message. Again, you won't be prompted to select an item number if you go this route:

	$ git commit -m "Adding some samples. Closes #42. Refs #54."
	[master bfb7a8b] Adding some samples. Closes #42. Refs #54.
	 0 files changed, 0 insertions(+), 0 deletions(-)
	 create mode 100644 sample