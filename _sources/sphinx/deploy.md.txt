# Sphinxで作成したページをデプロイする

- [Sphinx-Users.jp: Sphinxで作ったドキュメントのホスティング](https://sphinx-users.jp/cookbook/hosting/index.html#heroku-basic)

- [Sphinx-Users.jp: GitHub Actionsを用いてGitHub Pagesへのデプロイを自動化する](https://sphinx-users.jp/cookbook/githubaction/index.html)
- [Qiita: sphinxでドキュメント作成からWeb公開までをやってみた](https://qiita.com/kinpira/items/505bccacb2fba89c0ff0)
- [Sphinx-lesson: Deployment with Github Actions](https://coderefinery.github.io/sphinx-lesson/gh-action/)

## Deploy keys & Secretsを登録する

まず公開鍵と秘密鍵のペアを生成する
```sh
$ ssh-keygen -t ed25519 -C "$(git config use.email)" -f github-deploy-key-ed25519 -N ""
```

次に公開鍵と、秘密鍵を登録する。
- 公開鍵は、プロジェクトページの`Settings` -> `Deploy Keys`の`Add deploy key`で登録する。
- 秘密鍵は、プロジェクトページの`Settings` -> `Secrets`の`Add a new secret`で登録する。Secretは`MNY_ACTIONS_DEPLOY_KEY`という名前で登録する。


## GitHub Pagesの設定

`Settings` -> `GitHub Pages`でGitHub Pagesの設定をする.

**source**の項目を`gh-pages`の/root以下に設定すると、GitHub Pagesの成果物が`gh-pages`ブランチのルートディレクトリ以下に作成されるようになる。これらを変更することも可能である。
- [詳しくはこちらのページ](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)


## workflowsの作成
`.github/workflows/`に`document.yml`を作成する.

```
name: document

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: Ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Set up Python 3.8
        uses: actions/setup-python@v1
        with:
          python-version: '3.8'

      - name: Display python version
        run: python3 -c "import sys; print(sys.version)"

      - name: Install Sphinx dependencies
        run: sudo apt-get install python-dev build-essential

      # Note: it is better using requirements.txt to install required packages
      - name: Install Sphinx + requirements via pip
        run: |
          pip install sphinx
          pip install sphinx-rtd-theme
          pip install myst_parser

      - name: Build documentation
        run: make html

      # Posting to GitHub Pages
      - name: Deploy GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.MY_ACTIONS_DEPLOY_KEY }}
          publish_dir: ./build/html
          force_orphan: true
          commit_message: ${{ github.event.head_commit.message }}
```