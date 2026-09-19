# Create a website with GitHub and Zensical

Modern tooling has made it a breeze to create simple websites with modern features and version control.

## Goal

Create a **public** website using GitHub and open source tools.

## Before you start

To follow these instructions, you'll need

1. A GitHub account
2. An internet connection
3. A computer with a commandline and `git` installed

**This whole process should take ~20 minutes**, unless you get distracted with creating content for your new website :wink:.

### Technologies

This guide uses the technologies described below.

!!! quote "Disclaimer"

    I am not sponsored by these techonologies nor their owning interests.
    Use of these platforms and tools is subject to their respective licensing.
    Be sure to review their respective terms and conditions before proceeding.

* **GitHub ([github.com](https://github.com/)) and [GitHub Pages](https://docs.github.com/en/pages)** - a commercial cloud platform for hosting and developing code. 
* **`uv` ([docs.astral.sh/uv](https://docs.astral.sh/uv/))** - an open source tool for installing and managing Python environments. 
* **Zensical ([zensical.org](https://zensical.org))** - an open source, easy-to-use yet powerful website generator, from the developers of "Material for MkDocs".

    !!! warning
        **Zensical** is under active development and will certainly update more frequently than this webpage!
        Their documentation is pretty good, though, so check that out if there any issues.

### Process

The rest of the guide will walk you through these steps:

1. **Software setup** (5 minutes)
2. **Initial website setup** (10 minutes)
3. **View the website** (5 minutes)
4. **Update your website** (indefinite)

!!! warning
    This will create a ***public*** website at a URL that contains **your GitHub username** and **the name of the GitHub repository**!

## Software setup

This is a one-time setup for the computer you are using.

!!! tip
    I recommend that you use a personal Linux or macOS computer.
    If you are on Windows, then I recommend that you setup [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/) to obtain a Linux enviroment.

1. Open a terminal on your computer.
2. Run the `uv` [installation script](https://docs.astral.sh/uv/#installation).

    * *If on Linux/macOS*:
        
        ```bash
        curl -LsSf https://astral.sh/uv/install.sh | sh
        ```

    * *If on Windows*:
        
        ```powershell
        powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
        ```

That's it!

You should now have the `uv` command in your terminal. 
You can confirm this by running

```
uv help
```

## Initial website setup

You only need to do these steps the first time you setup the website.

!!! tip "Website URL"
    You should be deliberate in the names that you choose.

    For example, with 

    * GitHub username `user123` 
    * repository name `my-first-website`

    then the website will be publically available at

    ```
    https://user123.github.io/my-first-website
    ```

### Create a public GitHub repository

1. Login to [github.com](https://github.com).
2. Click the "+" icon in the menu bar and select "Create Repository".
3. Fill in the details on the page, but skip adding the optional files.
4. Click create.

Keep this page open for the time being. 
It includes instructions for how to upload code to the repository, which you'll do in a later step.

### Enable GitHub Actions for the repository

1. From the menu bar for the new repository on GitHub, open the "Settings" page in a new tab.
2. Select the "Pages" page from the left-hand navigation menu.
3. Under the "Source" dropdown, change the selection to "GitHub Actions".

The page will reload and present some starting templates.
Ignore these, because Zensical will automatically create what is needed!

### Setup Zensical environment using uv

1. Create a directory on your computer with the same name as the GitHub repository.
2. Navigate into this directory with your terminal.
3. Initialize `uv` for this directory with

    ```
    uv init
    ```

4. Install Zensical with

    ```
    uv add zensical
    ```

You now have Zensical installed in this local environment.
You can confirm this by running

```
uv run zensical --help
```

### Create Zensical project

1. While still in the terminal in the same directory, run

    ```
    uv run zensical new
    ```

    No message will print, but it will add a `zensical.toml` file and a `docs` directory where you ran it.

2. Open the `zensical.toml` file in your favorite editor.
3. Edit the `site_url` and `site_name` lines with the desired details.

    * `site_url` - set this to the expected GitHub Page URL, i.e., `https://GITHUB_USERNAME.github.io/REPOSITORY_NAME`.
    * `site_name` - the name you want to show up on the website

4. The next couple of lines are optional. 
    You will need to remove the leading `#` and space in order for the line to activate.

    * `site_description` - a short, one line description
    * `site_author` - your name or desired nom de plume
    * `copyright` - if so desired

5. The next couple of lines are strongly recommended.
    Remove the leading `#` and space to activate the lines.

    * `repo_url` - set this value to the URL for the GitHub repository you created earlier, i.e., `https://github.com/GITHUB_USERNAME/REPOSITORY_NAME` 
    * `repo_name` - set this value to the repository name, i.e., `GITHUB_USERNAME/REPOSITORY_NAME`
    * `edit_uri` - uncomment this value but do not otherwise alter it

    !!! warning
        The `site_url` is **NOT** the same as the `repo_url`.

        * The `site_url` corresponds to the nicely rendered website that you are creating.
        * The `repo_url` corresponds to the GitHub repository that hosts the codebase.

6. *The activation of `repo_url` and `edit_uri` will only work if you follow this step as well.*

    Look for the lines containing `content.action.edit` and `content.action.view`; somewhere around line 28 of the file.

    Remove the leading `#` and **only a single space** from the start of those lines.
    There should be exactly two spaces at the start of each line.
    
7. Save and close the `zensical.toml` file.

### Upload project to GitHub repository

At this point, all of the work is local to your computer.
To upload the files to GitHub, we'll follow a modified version of the commands that were shown in the webpage after initially creating the GitHub repository.

1. "Add" your changes to the Git repository.

    Still in the same folder, run this command:

    ```
    git add .
    ```

2. "Commit" your changes to the Git repository by running

    ```
    git commit -m 'Initial commit'
    ```

    !!! failure "Author identity unknown"
        If you have not used `git` on this computer before, you may receive this error message along with the message `*** Please tell me who you are.`
        
        Run the suggested commands to configure `git` on the computer with the necessary information.

3. Set the "branch" name to `main` with

    ```
    git branch -M main
    ```

    This name **must** be `main` for this example; this can be changed, but the defaults we're using assume it is `main`.

4. Link your local repository to the GitHub repository by running

    ```
    git remote add origin https://github.com/GITHUB_USERNAME/REPOSITORY_NAME.git
    ```

5. Upload the local changes to GitHub for the first time with

    ```
    git push -u origin main
    ```

### Look at the website

Now, we wait.

If everything has been done correctly, then GitHub will automatically create and publish the website using the Zensical software.
This should take less than 5 minutes, typically.

After a few minutes, go to your website URL at `https://GITHUB_USERNAME.github.io/REPOSITORY_NAME`.

You should see the website! *Anyone* with the link should also be able to see the website!

## Updating your website

Now the real work begins.

### Editing pages in the web browser

This is best for making small edits, one page at a time.

It only requires a web browser and your GitHub account.

1. Navigate to the page in your website that you want to edit.
2. In the top right of the page, below the search bar and to the left of the "On this page" navigation, should be a pair of paper icons. 
3. Click the icon with of the paper with the pencil; hovering on it should say "Edit this page".
4. On the GitHub page that loads, you can edit the corresponding Markdown file!
5. Once you are done, click the "Commit changes" button in the top right corner.
6. Confirm the commit details.

The website should update with the changes in a couple of minutes.

### Editing the website in codespace

For more in-depth, multi-file changes, while still using the web browser, consider using GitHub Codespace.

1. Go to the GitHub repository page at `https://github.com/GITHUB_USERNAME/REPOSITORY_NAME`.
2. Click on the green "Code" button.
3. Select the "Codespaces" tab, then "Create codespace on main". 
4. Within a couple of minutes, an editing environment should load in your browser.
5. Create and edit files, then commit the changes when you are done.

For more information on this feature, see [the Codespaces documentation](https://docs.github.com/en/codespaces). 

### Editing the website locally

Like with any GitHub repository, you can edit the changes locally.

1. Go to the directory in your command line where you initially set up the website.
2. Download the latest version of the code **before** making any changes, with

    ```
    git pull
    ```

3. Create and edit files as you see fit.
4. "Add" changed and new files with

    ```
    git add FILENAME_1 FILENAME_2
    ```

5. "Commit" the changes with

    ```
    git commit -m 'YOUR COMMIT MESSAGE HERE'
    ```

6. Upload the changes to GitHub with

    ```
    git push
    ```

This is a fairly unsophisticated approach to working with `git` and GitHub, but as long as you are the only one making edits and only ever work from the same computer, there should be no issues.

If you want to be able to make edits from multiple computers, and without publishing the changes until you are ready, you should learn more about "branches" and "pull requests".

### Previewing the website locally

If you've been editing the website on your computer and want to preview the changes before you upload them to GitHub, you can preview the website and its changes in your browser.

1. In the website code directory (where the `zensical.toml` file is located), run

    ```
    uv run zensical serve
    ```

2. In your web browser, go to `http://localhost:8000` (or whatever link is on the first line printed by the `serve` command).

    !!! failure
        
        If there's a problem with how Zensical is setup, the command may not work. The error message should indicate the cause of the problem, but typically it's an error in the `zensical.toml` file.

3. This local preview should automatically update with any saved changes you make to the underlying files!

    !!! tip

        It's a good idea to preview changes before uploading them to GitHub. That way you can catch "obvious" visual problems that can arise from subtle Markdown syntax mistakes!

4. Cancel the command or close it's terminal when you are done with the preview.

## Tips

### Add website link to GitHub repository

While the website has been published, it's not directly linked in your GitHub repository, yet.

1. Go to the GitHub repository main page at `https://github.com/GITHUB_USERNAME/REPOSITORY_NAME`.
2. On the right-hand side, look for the "About" section. Click the settings gear icon.
3. Check the box for "Use your GitHub Pages website".
4. Save changes.

Now the website link appears in the "About" section!

### Setup a main GitHub pages website

You may be wondering how to setup the main `https://GITHUB_USERNAME.github.io` website, with no repository name.
You can use the instructions in this guide to do so!

The only change is that when you create the GitHub repository, give it the name `GITHUB_USERNAME.github.io`.
If your username is `user123`, then the name of the repository should be `user123.github.io`. 

This is a special name that GitHub automatically maps to the shorter `https://GITHUB_USERNAME.github.io`.

### Website navigation

By default, in the `zensical.toml` file is a `nav` section that has the default files populated.

**Manual navigation**

You can change the `nav` mapping of pages to files manually as you like. 
See the documentation for how you can setup nested navigation and other advanced features.

**Automatic navigation**

Alternatively, just remove this section from the `zensical.toml` file altogther. 
Now, Zensical will automatically build the website navigation from the directory structure of the `docs` directory: subdirectories become their own sections of the website, with `.md` files as the corresponding pages.

### Zensical features

Zensical has a lot of cool features, some of which are introduced in the default webpages created when following this guide.
The documentation has a lot more, and most of these features are turned on by default!

It's worth crawling through the documentation to see what strikes your fancy. 

