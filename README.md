# Twitch images in chat
1) устанавливаете [Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo) ЕСЛИ ХОТИТЕ УСТАНОВИТЬ НА FIREFOX ИЩИТЕ В СВОЕМ МАГАЗИНЕ РАСШИРЕНИЙ
2) Включите режим разработчика во вкладке расширений, справа сверху
3) Нажимаете создать новый скрипт
4) Вставляете код ниже
```js
// ==UserScript==
// @name         TwitchImages
// @namespace    http://tampermonkey.net/
// @version      2024-10-17
// @description  try to take over the world!
// @author       NeoDim
// @match        https://www.twitch.tv/*
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        none
// ==/UserScript==

function loadImageByUrl(url) {
    return new Promise((resolve, reject) => {
        const img = new Image();
        img.src = url;
        let timeoutId = setTimeout(() => {
            reject(false);
        }, 500);

        img.onload = () => {
            clearTimeout(timeoutId);
            resolve(true);
        };

        img.onerror = () => {
            clearTimeout(timeoutId);
            reject(false);
        };
    });
}

;(async function() {
    const regex = /https?:\/\/[^\s]+(?:\.jpg|\.jpeg|\.png|\.gif|\.bmp|\.tif|\.tiff|\.webp|\.jfif)/i;


    setInterval(() => {
        const messagesList = document.querySelectorAll('span[data-a-target="chat-line-message-body"], span.seventv-chat-message-body');
        for (const messageBody of messagesList) {
            for (const messagePart of messageBody.children) {
                const message = messagePart.textContent.trim();
                if (regex.test(message)) {
                    loadImageByUrl(message).then((isLoad) => {
                        if (isLoad) {
                            messagePart.innerHTML = `<img src="${message}" style="max-height: 250px; height: auto;">`;
                        }
                    });
                }
            }
        }
    }, 1000);
})();
```
5) Включаете скрипт и перезапускаете браузер
