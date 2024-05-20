# gh-pages

[gh-pages - npm](https://www.npmjs.com/package/gh-pages)

Automate the deployment of your project's static files to the `gh-pages` branch of your GitHub repository, which "GitHub Pages" then uses to build and host your website.

```bash
npm install gh-pages --save-dev
```

```json
{
  "scripts": {
    "build": "",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

```bash
npm run deploy
```

Repo Settings -> Code and automation : Pages -> Build and deployment : Branch -> Choose `gh-pages` branch
