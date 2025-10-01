# fetchリクエストを中断する

作成日 2025/09/04

## 1. [AbortController](https://developer.mozilla.org/ja/docs/Web/API/AbortController)

AbortControllerオブジェクトは、WEBリクエストをいつでも中断することが可能

fetchなどの非同期操作の通信は、AbortSignalオブジェクトを使って行われる

## 2. [AbortSignal](https://developer.mozilla.org/ja/docs/Web/API/AbortSignal)

AbortControllerオブジェクトを介して中止することを可能にするシグナルオブジェクト

## 3. サンプルコード

```javascript
let controller;
const url = "video.mp4";

const downloadBtn = document.querySelector(".download");
const abortBtn = document.querySelector(".abort");

downloadBtn.addEventListener("click", fetchVideo);

abortBtn.addEventListener("click", () => {
  if (controller) {
    controller.abort();
    console.log("Download aborted");
  }
});

async function fetchVideo() {
  controller = new AbortController();
  const signal = controller.signal;

  try {
    const response = await fetch(url, { signal });
    console.log("Download complete", response);
    // process response further
  } catch (err) {
    console.error(`Download error: ${err.message}`);
  }
}
```
