# Sphinxのインストール

詳しくは[公式のインストールガイド](https://www.sphinx-doc.org/ja/master/usage/installation.html)を参照のこと.

## MacOS (brewを使う方法)
```sh
$ brew install sphinx-doc

```
インストール後、パスを通す
```sh
echo 'export PATH="/usr/local/opt/sphinx-doc/bin:$PATH"' >> ~/.zshrc
```

## Linux (Ubuntu20.04)
```sh
$ sudo apt install python3-sphinx
```

## PyPIからパッケージをインストールする
```sh
$ pip3 install sphinx
```

---

## 参考文献
1. [SPHINX Documentation: Sphinxのインストール](https://www.sphinx-doc.org/ja/master/usage/installation.html)