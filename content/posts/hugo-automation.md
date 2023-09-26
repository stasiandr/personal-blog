---
title: "How to host personal blog for developers"
description: "Hugo → GitHub Actions → Firebase"
date: 2020-09-15T11:30:03+00:00
tags: ["Tutorial", "Hugo", "Git", "GitHub Actions" , "Firebase"]
author: "Me"
draft: false
hidemeta: false
comments: false
disableHLJS: false
hideSummary: false
searchHidden: false

---

In today's blog post, we'll walk through the process of setting up a static website using the Hugo framework, deploying it to Firebase Hosting, and automating the deployment process with GitHub Actions. While this may not be the most exhilarating topic, it can be usefull for bloggers and developers looking to manage their websites efficiently.

## Hugo

### Installing Hugo

To start, we'll need to [install Hugo]((https://gohugo.io/installation/)). 
If you're using MacOS or Linux and have Homebrew installed, you can run the following command:

```bash
brew install hugo
```

Once Hugo is installed, you can create a new Hugo project and navigate to the project directory using these commands:

```bash
hugo new site personal-blog
cd personal-blog
```

### Adding a Theme

Themes are an essential part of any website, and Hugo provides a wide range of options. For this tutorial, we'll use the 
[PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, which you can find on the [Hugo themes]((https://themes.gohugo.io)). 
To add the theme to your project, run these commands:

Next you need theme. For this you may want to go to [Hugo themes](https://themes.gohugo.io). As for this tutorial (and this blog) we will be using [PaperMod](https://github.com/adityatelange/hugo-PaperMod). 

> Don't forget to add hugo `.gitignore` to your repo, it can be found [here](https://github.com/github/gitignore/blob/main/community/Golang/Hugo.gitignore)

```bash
git init
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

# needed when you reclone your repo (submodules may not get cloned automatically)
git submodule update --init --recursive
echo "theme = 'PaperMod'" >> hugo.toml
``` 

### Test Your Hugo Site

With your theme in place, you can now serve your Hugo site locally to start writing and designing your blog posts:


```bash
hugo serve
```

While the website may seem barebones right now, it's operational. For in-depth information on how Hugo functions and to explore the PaperMod theme further, I encourage you to visit [Hugo's official website](https://gohugo.io)  and the [PaperMod GitHub repository](https://github.com/adityatelange/hugo-PaperMod). .

> One last non-obvious thing: you can replace `hugo.toml` with `config.yaml`.

## Firebase

### Settings up Firebase

> Before we proceed, create a new [GitHub](https://github.com) repository for your project. 

Now, let's set up Firebase. If you haven't already, install the Firebase CLI globally:

```bash 
npm install -g firebase-tools
```

After installation, log in to your Firebase account:

```bash
firebase login
```

Next, initialize your Firebase project. Choose the "Hosting" option when asked about Firebase features:

```bash
firebase init
```

You can stick with the default options for most of the questions, as we'll configure GitHub Actions for deployment later.

```text
? What do you want to use as your public directory? public
? Configure as a single-page app (rewrite all urls to /index.html)? No
? Set up automatic builds and deploys with GitHub? No
```

### Manual Deployment to Firebase (Optional)

If you'd like to deploy your website manually, you can do so with the following commands:

```bash 
hugo
firebase deploy
```

> Don't forget to update the baseUrl in your Hugo configuration to ensure your links work correctly.

## GitHub Actions

### Github Actions Workflow

Now, let's automate the deployment process with GitHub Actions. In your Hugo project, create a `.github/workflows` folder and add a `deploy.yaml` file with the following content:

```yaml
name: Deploy Hugo to Firebase

on:
  push:
    branches:
      - main  # Change this to the branch you want to trigger deployment from

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2
      with:
        submodules: recursive # This clones our theme 

    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: 18  # You can change the Node.js version if needed

    - name: Install Hugo
      run: npm install -g hugo-cli

    - name: Build Hugo site
      run: hugo --minify

    - name: Deploy to Firebase
      run: |
        npm install -g firebase-tools
        firebase deploy --token "$FIREBASE_TOKEN" --only hosting

    env:
      FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```

### GitHub Secrets

To securely store your Firebase token, go to your GitHub repository, navigate to "Settings" > "Secrets" > "Actions," and add a new repository secret named FIREBASE_TOKEN. You can obtain this token by running following:

```bash
firebase login:ci
```

### Push to GitHub

Finally, push your repository to GitHub:

```bash
git remote add origin <your repository>
git branch -M main
git push -u origin main
```

Now, you can monitor the "Actions" tab in your GitHub repository to see the automated build and deployment process in action. 
Your website will be up and running on Firebase Hosting, ready for you to publish your content.

