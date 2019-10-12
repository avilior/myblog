---
title: "Hosting Hugo Site on GitHub Pages"
date: 2019-10-11T20:24:03-04:00
draft: false
---

I am hosting my [Hugo](https://github.com/gohugoio) generate static [site](https://avilior.github.io/) on GitHub Pages.

My setup is as follows:

* My site repo is on [GitHub](https://github.com/avilior/myblog)
* Locally i have clone on my mac. As per the instructions [here]((https://gohugo.io/hosting-and-deployment/hosting-on-github/)), I embed my avilior.github.io site as the public folder where hugo generates the site.  I have a deploy.sh script that I use to deploy the site.

# Site Generator Choice

[Jekyll](ttps://jekyllrb.com) the grandfather of static site generator is tightly integrated with GitHub Pages but I started out with Hugo and choose to stay with it.

# WorkFlow

* I use [typora](https://typora.io) as my MarkDown editor to write the blogs.

* I save them to `MyBlog/content/posts`directory.

* To generate the site and test it:

  ```
  cd ~/MyBlog/
  hugo -t ananke
  hugo server
  ```

* To deploy it - which builds the site commits the public directory to github and also commits the site to github

  ```
  cd ~/MyBlog/
  ./deploy.sh
  ```


# Deploy Script

The following is the deploy script i got from the web with slight modifications:

* The shell is zsh and not bash
* I added lines to commit my site to the GitHub repo.

```bash
#!/bin/zsh

# If a command fails then the deploy stops
set -e

printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"

# Build the project.
hugo -t ananke

# Go To Public folder
cd public

# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site $(date)"
if [ -n "$*" ]; then
	msg="$*"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

cd ..

git add .
git commit -m "$msg"
git push origin master

```

# References

- [Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [Working with submodules - The GitHub Blog](https://github.blog/2016-02-01-working-with-submodules/)
- [Working with GitHub Pages - GitHub Help](https://help.github.com/en/categories/working-with-github-pages)
- [About GitHub Pages and Jekyll - GitHub Help](https://help.github.com/en/articles/about-github-pages-and-jekyll)

