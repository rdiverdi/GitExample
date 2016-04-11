---
---
# Making a Github Pages Website

### Contents
- [making a project page - for a single project](#making-a-project-page)
- [making a personal page - for something like a portfolio website](#making-a-personal-page)
- [hosting on a different domain](#hosting-webpage-on-custom-domain)

### Making a Project Page
##### What Github Does
Github will host a webpage for any of your projects.  It looks for a branch named gh_pages to find files for a website.  If you know how to make a website and just want to put the files somewhere for github to host them, make a branch for your project called `gh_pages` and push your website files to that branch.  Then go to `username.github.io/ProjectName` and you will get your webpage.
##### Using Github to Make a Webpage
To automatically generate a webpage:
- Go to the github website, and open the page for the repository you want to make a website for.
- Click on `Settings` (in the top menu)
- Scroll down to the section titled `GitHub Pages` and click `Launch automatic page generator`
- The form that comes is the content of your webpage
  - `Project Name` will be the page's title
  - `Tagline` will appear as a subtitle
  - The `Body` section has some helpful guidelines at the top, everything in the block of writing will be the content of your page.
    - This content is written in markdown - which will allow you to do most of the things you want to do on your website.
    - You can click `Load README.md` to load in your README as the content of your website (which you can then edit)
- Continue to Layout brings you to the options for the website layout.  
  - Choose a layout and click publish page
- Your page should now be posted to `username.github.io/ProjectName`
You can always go back and edit your page by going back to settings, launch page generator: the content will be the same as you left it.

##### Linking to a markdown file
If you have a markdown file for, say, one of the project writeups and you want to link to it, you can put a copy of it in the gh_pages branch of your repository (from terminal on your computer) and push it to github.  You can then link to that file from your main page using markdown syntax `[words that appear](filename)` to make a link to that file.

###### NOTE:
In order for the markdown file to appear formatted correctly, you need to put this:
```
---
---
```
at the top of the markdown file

### Making a Personal Page
Github will host a webpage for you independet of specific projects on `username.github.io`.
##### Setup Webpage:
- Make a github repository called `<username>.github.io`
- follow the same steps as [above](#using-github-to-make-a-webpage) (settigs, launch automatic page generator)
- Alternately, like before, you can push your own website files to the `username.github.io` repository and it will host your webpage for you.

### Hosting Webpage on Custom Domain
github has [instructions here](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) for hosting your github webpage on a different domain 