---
draft: false
featured: none
authors:
  - Miksin
title: "Backlog 清了一半：踏板、觸控、還有幾個小事"
description: "第一次讓 agent 自己跑 backlog。sustain pedal、手機觸控支援、文章標題整理，一個下午處理四個 issue。"
pubDate: 2026-04-25T16:00:00.000Z
license: mit
tags:
  - piano
  - dev-log
  - workflow
image:
  src: /thumbnails/bugfix-session.svg
  alt: Nightly session illustration
---

今天試了一個新流程：早上 agent 回報 backlog 觀察，我挑幾個移到 Todo，然後叫 agent 直接開始處理。

效果還不錯。

## 處理了什麼

**MIK-13 — About 頁面 404**：這個其實我自己順手修了，agent pull 下來發現已經好了，直接標 Done。有時候最快的修法就是人直接動手。

**MIK-14 — 文章標題前綴**：幾篇文章標題還帶著 `[dev-session]`、`[piano]` 這種前綴，是剛開始沒想清楚留下來的習慣。一行 patch 改掉，改用 tags 分類，標題回歸乾淨的自然語句。

**MIK-12 — Sustain Pedal**：空白鍵模擬鋼琴踏板。按住 Space 時，放開琴鍵不會立刻消音；放開 Space 才觸發所有 noteOff。實作上用一個 `sustainedNotes` Set 暫存，等踏板釋放再批次 noteOff。這個功能讓彈奏起來明顯更有「鋼琴感」。

**MIK-6 — 行動裝置觸控**：在手機上 piano 以前不能用——click 有延遲，AudioContext 也不一定能在 touch 事件中啟動。現在改成 `onTouchStart` 裡直接 `await Tone.start()` 再 noteOn，`onTouchEnd` 觸發 noteOff，多指觸控各自獨立。

## 新的開發節奏

我們現在有兩個 cron job：

- **JST 09:00**：agent 自動觀察專案、研究業界動向，把新的改進點加到 Linear backlog
- **JST 23:00**：agent 自動處理所有 Todo 狀態的 issue

我的角色就是早上看報告，把想做的拖到 Todo，晚上收結果。

還在觀察這個流程跑起來的感覺。今晚是第一次手動觸發「處理一半」試水溫——四個 issue，都成功了。
