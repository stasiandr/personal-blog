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

## Introduction

Why do you want hugo

## Hugo

First of all let's create our blog website with Hugo. To install hugo you can go to [Hugo installation](https://gohugo.io/installation/) section. 
On MacOS and Linux you can run this (of course, if you are earlier installed brew):

```bash
brew install hugo
```

Then we can create out webstite. To create the directory structure for your project in the `./personal-blog` directory.

```bash
hugo new site personal-blog
cd personal-blog
```

Next you need theme. For this you may want to go to [Hugo themes](https://themes.gohugo.io). As for this tutorial (and this blog) we will be using [PaperMod](https://github.com/adityatelange/hugo-PaperMod). Themes are installed throug git. Don't forget to add hugo `.gitignore` to your repo, it can be found [here](https://github.com/github/gitignore/blob/main/community/Golang/Hugo.gitignore). To install it you run:

```bash
git init
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

# needed when you reclone your repo (submodules may not get cloned automatically)
git submodule update --init --recursive
echo "theme = 'PaperMod'" >> hugo.toml
``` 

And at last you want to serve you blog to write post as like I'm literraly doing rigth now

```bash
hugo serve
```

It's empty but it's working. I dont want to go much into details of how hugo works, so I highly incorage you to go to [Hugo website](https://gohugo.io) and [PaperMod](https://github.com/adityatelange/hugo-PaperMod). 

One last non-obvious thing: you can replace `hugo.toml` with `config.yaml`.

## Firebase

Before next steps you want to create new GitHub repo.

So first of all you want to create you project at [Firebase Console](https://console.firebase.google.com/). Next you want to install **Firebase CLI** by running following:

```bash 
npm install -g firebase-tools
```

Next you login into your firebase accout.

```bash
firebase login
```

And initializes project. In firebase features we want to select `❯◉ Hosting: Configure files for Firebase Hosting and (optionally) set up GitHub Action deploys`.

```bash
firebase init
```

Next there will be a lot of questions about yout repo. We can answer every question with default option, because we want to set up our github actions later.

```text
? What do you want to use as your public directory? public
? Configure as a single-page app (rewrite all urls to /index.html)? No
? Set up automatic builds and deploys with GitHub? No
```

For now if you want to deploy your website manually, run following: 

```bash 
hugo
firebase deploy
```

Do not forget to change your baseUrl, because otherwise all your links will be broken!

## GitHub Actions

Create `.github/workflows` folder insinde Hugo project. We will build and deploy using npm. There you place `deploy.yaml` file:

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
        submodules: recursive

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

Next in your GitHub repository, go to "Settings" > "Secrets" > "Actions". You want to click on "new repository secret". Use "FIREBASE_TOKEN" as key.

To obtain this token run folowing:

```bash
firebase login:ci
```

And now, you want to push our repo to GitHub:

```bash
git remote add origin <your repository>
git branch -M main
git push -u origin main
```

## Last things


