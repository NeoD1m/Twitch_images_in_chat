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
const initialization = () => {
    const observer = new MutationObserver((mutations) => {
        const container = mutations[0].target;
        const message = mutations[0]?.addedNodes[0]?.querySelector('.seventv-chat-message-body');
        const link = message?.querySelector('.link-part');

        if (link && link.href.match(/\.(png|jpg|jpeg|gif|webp)/ig)) {
            const img = new Image();
            img.src = link.href;
            img.style.height = "auto";
            img.style.maxHeight = "250px";
            img.onload = () => {
                container.scrollTop = (container.scrollHeight + img.height);
            }

            const anchor = document.createElement('a');
            anchor.href = link.href;
            anchor.target = '_blank';
            anchor.rel = 'noopener noreferrer';
            anchor.appendChild(img);

            message.innerHTML = '';
            message.appendChild(anchor);
        }
    });

    observer.observe(document.querySelector('main.seventv-chat-list'), {
        subtree: false,
        childList: true
    });
}

setTimeout(initialization, 5000);
```
5) Включаете скрипт и перезапускаете браузер
