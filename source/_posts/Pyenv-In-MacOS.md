title: '[Python] 使用 pyenv 管理 python 版本'
author: ''
date: 2022-05-15 23:30:00
tags:
  - Python
  - pyenv
categories:
  - Python
---

pyenv 可以協助管理 python 的版本，讓你在 python2、python3 間隨意切換，此篇會紀錄我是如何安裝 pyenv 的，以下是我的系統配置：

作業系統：MacOS Mojave 10.14.6
終端機：bash terminal

<!--more-->

會分為以下步驟：

<ol>
	<li>homebrew 安裝 pyenv</li>
  	<li>輸入 pyen init</li>
  	<li>修改 .bash_profile</li>
    <li>使 .bash_profile 生效</li>
    <li>用 pyenv 安裝 python 版本</li>
    <li>驗證</li>
</ol>

---

## Step01. homebrew 安裝 pyenv

不知道 homebrew 的可以估狗一下
安裝完 homebrew 後於終端機執行以下指令

{% codeblock lang:bash %}
brew install pyenv
{% endcodeblock %}

## Step02. 輸入 pyen init

應該會跑出一個 guideline，告訴你要怎麼設定環境變數，以我的例子，我先將以下指令複製起來

{% codeblock lang:bash %}
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
{% endcodeblock %}

## Step03. 修改 .bash_profile

將上一步複製的指令，新增於 .bash_profile 檔案最後，.bash_profile 通常會放在 `~/` 路徑下，你可以執行以下指令確認該檔案是否存在

{% codeblock lang:bash %}
cd ~
cat .bash_profile
{% endcodeblock %}

## Step04. 使 .bash_profile 生效

{% codeblock lang:bash %}
source .bash_profile
{% endcodeblock %}

## Step05. 用 pyenv 安裝 python 版本

可以先執行以下指令，看有哪些版本可供安裝

{% codeblock lang:bash %}
pyenv install --list
{% endcodeblock %}

選定好版本後，用 pyenv 安裝 python

{% codeblock lang:bash %}
pyenv install 3.8.0
{% endcodeblock %}

安裝完後查看目前的版本，應有 `system` 以及剛剛安裝的版本 `3.8.0`

{% codeblock lang:bash %}
pyenv versions

# 輸入上面的指令後應該有像下面的結果
* system
  3.8.0 (set by /Users/{當前登入的帳號}/.python-version)
{% endcodeblock %}

接著選擇剛剛安裝的版本。使用 `global` 指令的話，之後你開啟終端機，呼叫 python 時都會使用這個版本，如果你只想要此次使用該版本，則使用指定 `local`

{% codeblock lang:bash %}
pyenv global 3.8.0
{% endcodeblock %}

## Step06. 驗證

{% codeblock lang:bash %}
python -V # 理應為 3.8.0
which python # 路徑理應是 /Users/{當前登入的帳號}/.pyenv/shims/python
{% endcodeblock %}

## 參考

[[用舒服的姿勢開發 Python Project] Day 03 - Pyenv 基本使用](https://ithelp.ithome.com.tw/articles/10237266)
[Python 版本管理的好工具 - pyenv](https://myapollo.com.tw/zh-tw/pyenv/)
[在 Mac 上用 pyenv 輕鬆安裝 Python3 (可直接支援安裝多種版本)](https://stringpiggy.hpd.io/mac-osx-python3-multiple-pyenv-install/)
