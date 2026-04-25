---
draft: false
featured: none
authors:
  - Miksin
title: "三個小 Bug，一個下午"
description: "音源靜音、版權人錯誤、黑鍵跑位——看起來各自獨立，修起來卻都讓人恍然大悟。"
pubDate: 2026-04-25T14:00:00.000Z
license: mit
tags:
  - piano
  - bugfix
  - dev-log
image:
  src: /thumbnails/bugfix-session.svg
  alt: Bugfix session — bug fixed illustration
---

今天沒有新功能，全是 bug 修復。但有時候這種日子反而讓我覺得踏實——發現問題、搞清楚原因、一行改掉，世界就對了。

## 黑鍵沒有聲音

這個 bug 有點偽裝得很好。Piano 畫面一切正常，白鍵按下去有聲音，但黑鍵靜音。打開 Network 面板才看到端倪——所有升音記號（`D#`、`F#`、`A#`…）的音源 URL 全是 404。

罪魁禍首藏在 [gleitz/midi-js-soundfonts](https://github.com/gleitz/midi-js-soundfonts) 的命名習慣。這個 CDN 用的是**降音（flat）命名**：`Eb1.mp3`、`Gb1.mp3`、`Bb1.mp3`，而不是我們習慣的升音（`D#1`、`F#1`、`A#1`）。

音樂理論上 D# 和 Eb 是同一個音，但檔案系統可不管這個。修法很直接，在 `samplerEngine.ts` 加一張對照表：

```ts
const sampleMap: Record<string, string> = {
  'D#1': 'Eb1', 'F#1': 'Gb1', 'A#1': 'Bb1',
  // ...以此類推
}
```

Tone.js 內部繼續用升音名稱操作音符，只有實際去抓檔案的時候才換成降音。這種「介面與實作分離」的感覺，還蠻乾淨的。

## 黑鍵跑去白鍵旁邊

修好聲音之後又發現另一個視覺 bug：黑鍵位置不對，全部貼在白鍵左側，沒有交錯在兩個白鍵之間。

追到 `PianoKeyboard.tsx` 的定位計算：

```ts
// 錯的
blackKeyPositions.push({ key: k, whiteIndex: whiteIdx - 0.5 })

// 對的
blackKeyPositions.push({ key: k, whiteIndex: whiteIdx })
```

多了一個 `- 0.5` 導致黑鍵中心往左偏移了半個白鍵寬度。拿掉之後配合 CSS 的 `translateX(-50%)`，黑鍵自然就居中在兩個白鍵之間。一個數字的事。

## LICENSE 上的陌生名字

比較無聊但也必須修的：dev-log 的 `LICENSE` 檔案還寫著範本作者 "Daniel Adrian"。這個 blog 用了 [truedaniyyel/basic-blog](https://github.com/truedaniyyel/basic-blog) 作為起點，當時忘了換掉版權人。

改成 `Copyright (c) 2026 Miksin`，一行搞定。

---

三個 bug，性質截然不同——CDN 命名慣例、定位計算、範本殘留。但每個修完都有那種「對，就是這樣」的感覺。明天繼續。
