=================================
Linking local repo to Github repo
=================================

GitHub allows you to host git repositories online. This allows you to:

- Work collaboratively on a shared repository
- Fork someone else's repository to create your own copy that you can use and change as you want
- Suggest changes to other people's repositories through pull requests

To do any of this, you will need a GitHub account. You can sign up at https://github.com. A free account is fine as long as you don't mind all of your repositories being "Public" (viewable by anyone).

The basic unit for working in GitHub is the repository. A repository is a directory of files with some supplemental files saving some additional information about the directory. While R Projects have this additional information saved as an ".RProj" file, git repositories have this information in a directory called ".git".

Because this pathname of the .git directory starts with a dot, it won't show up in many of the ways you list files in a directory. From a bash shell, you can see files that start with ``.`` by running ``ls -a`` from within that directory.

If you have a local directory that you would like to push to GitHub, these are the steps to do it. First, you need to make sure that the directory is under git version control. See the previous notes on initializing a repository. Next, you need to create an empty repository on GitHub to sync with your local repository. To do that:

1. In GitHub, click on the "+" in the upper right corner ("Create new").
2. Choose "Create new repository".
3. Give your repository the same name as the local directory you'd like to connect it to. For example, if you want to connect it to a directory called "example_analysis" on your computer, name the repository "example_analysis". (It is not required for your GitHub repository name to be identical to your local repository name, but it will make things easier.)
4. Leave everything else as-is (unless you'd like to add a short description in the "Description" box). Click on "Create repository" at the bottom of the page.

Now you are ready to connect the two repositories. First, you should change some settings in RStudio so GitHub will recognize that your computer can be trusted, rather than asking for you password every time. Do this by adding an SSH key from RStudio to your GitHub account with the following steps:

1. In RStudio, go to "RStudio" -> "Preferences" -> "Git / svn". Choose to "Create RSA key".
2. Click on "View public key". Copy everything that shows up.
3. Go to your GitHub account and navigate to "Settings". Click on "SSH and GPG keys".
4. Click on "New SSH key". Name the key something like "mylaptop". Paste in your public key in the "Key box".



Syncing RStudio and GitHub
**************************

Now you're ready to push your local repository to the empty GitHub repository you created.

- Open a shell and navigate to the directory you want to push. (You can open a shell from RStudio using the gear button in the Git window.)
- Add the GitHub repository as a remote branch with the following command (this gives an example for adding a GitHub repository named "ex_repo" in my GitHub account, "geanders"):

.. code-block:: bash
    :linenos:
    
    git remote add origin git@github.com:geanders/ex_repo.git
    
  
As a note, when you create a repository in GitHub, GitHub will provide suggested git code for adding the GitHub repository as the "origin" remote branch to a repository. That code is similar to the code shown above, but it uses "https://github.com" rather than "git@github.com"; the latter tends to work better with RStudio.

Push the contents of the local repository to the GitHub repository.

.. code-block:: bash
    :linenos:
    
    git push -u origin master
    

To pull a repository that already exists on GitHub and to which you have access (or that you've forked and so have access to the forked branch), first use cd from a bash shell on your personal computer to move into the directory where you want to put the repository. Then, use the git clone function to clone the repository locally. For example, to clone a GitHub repository called "ex_repo" posted in a GitHub account with the user name janedoe, you could run:


.. code-block:: bash
    :linenos:
    
    git clone git@github.com:janedoe/ex_repo.git
    
Once you have linked a local R project with a GitHub repository, you can push and pull commits using the blue down arrow (pull from GitHub) and green up arrow (push to GitHub) in the Git window in RStudio.

GitHub helps you work with others on code. There are two main ways you can do this:


- **Collaborating**: Different people have the ability to push and pull directly to and from the same repository. When one person pushes a change to the repository, other collaborators can immediately get the changes by pulling the latest GitHub commits to their local repository.
- **Forking**: Different people have their own GitHub repositories, with each linked to their own local repository. When a person pushes changes to GitHub, it only makes changes to his own repository. The person must issue a pull request to another person's fork of the repository to share the changes.


Issues
******

Each original GitHub repository (i.e., not a fork of another repository) has a tab for "Issues". This page works like a Discussion Forum. You can create new "Issue" threads to describe and discuss things that you want to change about the repository.

Issues can be closed once the problem has been resolved. You can close issues on the "Issue" page with the "Close issue" button. If a commit you make in RStudio closes an issue, you can automatically close the issue on GitHub by including "Close #[issue number]" in your commit message and then pushing to GitHub. For example, if issue #5 is "Fix typo in section 3", and you make a change to fix that typo, you could make and save the change locally, commit that change with the commit message "Close #5", and **then push to GitHub, and issue #5 in "Issues" for that GitHub repository will automatically be closed, with a link to the commit that fixed the issue**.

Pull Requests
*************

You can use a pull request to suggest changes to a repository that you do not own or otherwise have the permission to directly change. Take the following steps to suggest changes to someone else's repository:

- Fork the repository
- Make changes (locally or on GitHub)
- Save your changes and commit them
- Submit a pull request to the original repository
- If there are **not any conflicts and the owner of the original repository likes your changes**, he or she can merge them directly into the original repository. If there are conflicts, these need to be resolved before the pull request can be merged.

You can also use pull requests within your own repositories. Some people will create a pull request every time they have a big issue they want to fix in one of their repositories.

In GitHub, each repository has a "Pull requests" tab where you can manage pull requests (submit a pull request to another fork or merge in someone else's pull request for your fork).


Merge Conflicts
***************

At some point, if you are using GitHub to collaborate on code, you will get merge conflicts. These happen when **two people have changed the same piece of code in two different ways at the same time**.

For example, say two people are both working on local versions of the same repository, and the first person changes a line to mtcars[1, ] while the second person changes the same line to head(mtcars, 1). The second person pushes his commits to the GitHub version of the repository before the first person does. Now, when the first person pulls the latest commits to the GitHub repository, he will have a merge conflict for this line. To be able to commit a final version, the first person will need to decide which version of the code to use and commit a version of the file with that code.

If there are merge conflicts, they'll show up like this in the file:

.. code-block:: none
    :linenos:
    
    <<<<<<< HEAD
    mtcars[1, ]
    =======
    head(mtcars, 1)
    >>>>>>> remote-branch
    
To fix them, search for all these spots in files with conflicts (Ctrl-F can be useful for this), pick the code you want to use, and delete everything else. For the example conflict, it could be resolved by changing the file from this:

.. code-block:: none
    :linenos:
    
    <<<<<<< HEAD
    mtcars[1, ]
    =======
    head(mtcars, 1)
    >>>>>>> remote-branch
    
To this:

.. code-block:: none
    :linenos:
    
    head(mtcars, 1)
    
That merge conflict is now resolved. Once you resolve all merge conflicts in all files in the repository, you can save and commit the files.

These merge conflicts can come up in a few situations:

- You pull in commits from the GitHub branch of a repository you've been working on locally.
- Someone sends a pull request for one of your repositories, and you have updated some of the code between when the person forked the repository and submitted the pull request.


      