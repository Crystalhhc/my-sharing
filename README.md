# MKdocs for Multi-language

## Project Layout

```
myDocs/
├── .venv/                 # Virtual environment directory
├── mydocs-en/             # English documentation project
│   ├── docs/
│   │   ├── index.md       # The documentation homepage (English)
│   │   ├── blog/
│   │   │   ├── posts/
│   │   │   └── index.md
│   │   └── ...            # Other markdown pages, images and other files
│   └── mkdocs.yml         # The configuration file for English docs
├── mydocs-zh-TW/          # Traditional Chinese documentation project
│   ├── docs/
│   │   ├── index.md       # The documentation homepage (Traditional Chinese)
│   │   ├── blog/
│   │   │   ├── posts/
│   │   │   └── index.md
│   │   └── ...            # Other markdown pages, images and other files
│   └── mkdocs.yml         # The configuration file for Traditional Chinese docs
└── README.md              # This file, containing project overview and instructions
```
## MKDocs Installaiton
1. Create a new Directory for root of  `myDocs`
```bash
mkdir myDocs
cd myDocs
```
2. Install Virtual Environment
```bash
python3  -m venv .venv
source .venv/bin/activate
```
3. Install mkdocs-material
```bash
pip install mkdocs-material
```
## Create Projects for English and Traditional Chinese Version each
1. Create a new Project for English Version
```bash
mkdocs new mydocs-en
```
That will generate:
```bash
(.venv) chenhsihu@MacBook-Pro-Crystal myDocs % mkdocs new mydocs-en
INFO    -  Creating project directory: mydocs-en
INFO    -  Writing config file: mydocs-en/mkdocs.yml
INFO    -  Writing initial docs: mydocs-en/docs/index.md
```
2. Create a New Project for Traditional Chinese Version
```bash
(.venv) chenhsihu@MacBook-Pro-Crystal myDocs % mkdocs new mydocs-zh-TW
```
Response:
```bash
INFO    -  Creating project directory: mydocs-zh-TW
INFO    -  Writing config file: mydocs-zh-TW/mkdocs.yml
INFO    -  Writing initial docs: mydocs-zh-TW/docs/index.md
```
## Developing Multiple Projects simultaneously
1. For your English documentation project:
```bash
cd ~/my-shaing
mkdocs serve -f ./mydocs-en/mkdocs.yml -a localhost:8000
```
2. For your Traditional Chinese documentation project:
```bash
cd ~/my-shaing
mkdocs serve -f ./mydocs-zh-TW/mkdocs.yml -a localhost:8001
```
By using different port numbers (8000 and 8001 in this example), you can run both projects simultaneously without conflicts.

You can also use different IP addresses if needed:
```bash
mkdocs serve -f /Users/chenhsihu/myDocs/mydocs-en/mkdocs.yml -a 127.0.0.1:8000
mkdocs serve -f /Users/chenhsihu/myDocs/mydocs-zh-TW/mkdocs.yml -a 127.0.0.1:8001
```

## MkDocs Commands

* `mkdocs new <dir-name>` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Setup Blog
1. add `blog` to plugins list in `mkdocs.yml` file
```yml title="mydocs-en/mkdocs.yml"
plugings:
    - search
    - tags
        tags_file: tags.md
    - blog
```
That will generate blog directory structure as below
```bash
myDocs-en/
├── docs/
│   ├── blog/
│   │   ├── posts/
│   │   └── index.md
│   └── index.md
├── mkdocs.yml
└── README.md
```
Note: Please do not add `blog` and `post` directories by yourself, that will not work properly.

## Multi-languages Setup
!!!Note: According to author's [documentation](https://github.com/squidfunk/mkdocs-material/blob/master/docs/setup/changing-the-language.md#site-language-selector), MKdocs is design for one projec with one language. It is limited by the nature of HTML language. Therefore, he suggested that to create a project for each languag, and using language selector to switch language.

Material for MkDocs supports internationalization (i18n) and provides translations for template variables and labels in 60+ languages.
1. Default language setting

`mydocs-en`:
```yml title="mydocs-en/mkdosc.yml"
site_url: https://mydomain.org/en/ 
  language: en
```
`mydocs-zh-TW`:
```yml title="mydocs-zh-TW/mkdosc.yml"
site_url: https://mydomain.org/zh-TW/ 
  language: zh-TW
```
2. Language selection settings 

`mydocs-en`:
```yml title="mydocs-en/mkdosc.yml"
extra:
  alternate:
    - name: English
      link: https://mydomain.org/en/  # absolute link
      lang: en
    - name: 正體中文
      link: https://mydomain.org/zh-TW/ # absolute link
      lang: zh-TW
```
`mydocs-zh-TW`:
```yml title="mydocs-zh-TW/mkdosc.yml"
extra:
  alternate:
    - name: English
      link: https://mydomain.org/en/  # absolute link
      lang: en
    - name: 正體中文
      link: https://mydomain.org/zh-TW/ # absolute link
      lang: zh-TW
```

### Experiment 1 - plugin: `mkdocs-static-i18n` (Not compatible with blog feature)
1. Install plugin: `mkdocs-static-i18n`
```bash
pip install "mkdocs-static-i18n[material]"

2. `mkdocs.yml`
```yml
plugins:

  - i18n:
      docs_structure: suffix
      languages:
        - locale: en
          default: true
          name: English
          build: true
        - locale: zh-TW
          name: 正體中文
          build: true 
```
Note: After installation, Option 1 conflicts with the blog functionality.
### Experiment 2  -  plugin: `mkdocs[i18n]` - No need. Material theme accepts build-in i18n
1. Install plugin: `mkkdocs[i18n]`
```bash
pip install 'mkdocs[i18n]'
```
2. `mkdocs.yml`
```yml
theme:
  name: mkdocs
  locale: zh-TW
```

## Sync with Github
We want to sync both version of languages in Github. Hence we need to change directory to the root 'myDocs`
1. cd to the root:
```bash 
cd ~/myDocs
```

2. inintialize Git
```bash
git init
```
3. add README.md(ignore it if it exists.) 
```bash
touch README.md
```
3. Create a new repository, i.e. `myDocs` in Github 
4. Push to Github

```bash
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/Crystalhhc/myDocs.git
git push -u origin main
```
## Publish in Github Pages
1. Ensure that the project is already pushed to GitHub
2. Create a new branch named `gh-pages` in your repository. This branch will be used to host your built documentation.
3. Configure your `mkdocs.yml` files for both `English` and `Traditional Chinese` versions to use the gh-deploy feature. Add the following to both `mkdocs.yml` files:
```yml title="mkdocs-en/mkdocs.yml"
site_url: https://crystalhhc.github.io/myDocs/en/
```

```yml title="mkdocs-zh-TW/mkdocs.yml"
site_url: https://crystalhhc.github.io/myDocs/zh-TW/
```
4. Build and deploy your English documentation:
```bash
cd ~/myDocs/mydocs-en
mkdocs gh-deploy --force
```
5. Build and deploy your Traditional Chinese documentation:
```bash
cd ~/myDocs/mydocs-zh-TW
mkdocs gh-deploy --force
```
6. Go to your GitHub repository settings, navigate to the "Pages" section, and ensure that the source is set to the gh-pages branch.

7. Your documentation should now be available at:
English: 
`https://crystalhhc.github.io/myDocs/en/`
Traditional Chinese: 
`https://crystalhhc.github.io/myDocs/zh-TW/`
8. To automate this process, you can set up a GitHub Actions workflow. Create a file named `.github/workflows/deploy-docs`.yml in your repository with the following content:
```yml title=".github/workflows/deploy-docs.yml"
name: Deploy MkDocs
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: 3.x
      - run: pip install mkdocs-material
      - run: mkdocs build --config-file mydocs-en/mkdocs.yml --site-dir site/en
      - run: mkdocs build --config-file mydocs-zh-TW/mkdocs.yml --site-dir site/zh-TW
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```
This workflow will automatically build and deploy both language versions of your documentation whenever you push changes to the main branch.
9. After pushing these changes, go to your GitHub repository settings:

- Navigate to "Settings" > "Pages"
- Under "Source", select the gh-pages branch
- For the folder, select / (root)

These changes will ensure that your 
- English version: https://crystalhhc.github.io/myDocs/en/
- Chinese version: https://crystalhhc.github.io/myDocs/zh-TW/
- Root URL (https://crystalhhc.github.io/myDocs/) will redirect to the English version

!!!Note
If have problems with deployment, please check:
1. Check Repository Permissions:
Go to your GitHub repository's settings, then to "Actions" under "Code and automation" in the left sidebar. Make sure "Read and write permissions" is selected under "Workflow permissions".
2. Verify GitHub Pages Settings:
In your repository settings, go to "Pages" and ensure that the source is set to "Deploy from a branch" and the branch is set to "gh-pages".

3. Update your workflow file:
Modify your .github/workflows/deploy-docs.yml file to use a Personal Access Token (PAT) instead of the default GITHUB_TOKEN. Here's how to do it:
a. Create a Personal Access Token:
Go to your GitHub account settings
Click on "Developer settings" in the left sidebar
Click on "Personal access tokens" and then "Tokens (classic)"
Generate a new token with the "repo" scope
