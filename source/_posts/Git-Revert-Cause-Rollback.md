title: '[Git] Revert 造成別人的程式被 rollback'
author: ''
date: 2022-05-01 23:30:00
tags:
  - Git
  - Revert
categories:
  - Git
---

有天工作時在 feature branch 開發完，準備發 pull request 回 develop branch，在 review PR 時發現有不是我開發的程式碼，而且在比較 branch diffrence 時，發現我的 pull request 會把別人的程式 rollback，原本想說只要從 develop branch 先發 PR merge 回 feature branch，再把 feature branch merge 回 develop 即可，但情況還是一樣，以為又是 Bitbucket 的 bug，~~畢竟之前還會在 X PR 看到 Y PR 的內容~~。

<!--more-->

就在苦惱到底為何會這樣時，同事的一句：「你是不是有 revert 啊？」讓我恍然大悟，仔細看我的 commit，發現我當時確實有做過把 **「develop branch merge 回 feature branch」**，但後來覺得這樣 PR 會包含不止我的程式碼，review 起來會很亂，所以又做 **「revert develop branch merge 進 feature branch」**，就是這個動作導致後續將 feature branch 要 merge 回 develop branch 時會將別人的 code 給 rollback，commit 的排序如下圖所示：

<img src="https://i.imgur.com/rZBm9tk.png">

解法很簡單，在 feature branch 將 **「revert develop branch merge 進 feature branch」** 的這個 commit 再 revert 一次，然後再從 feature branch 發 PR 回 develop branch 即可。
