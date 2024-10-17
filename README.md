# Twitch images in chat
0) ОБЯЗАТЕЛЬНО ДОЛЖЕН БЫТЬ УСТАНОВЛЕННО РАСШИРЕНИЕ [7TV](https://chromewebstore.google.com/detail/7tv/ammjkodgmmoknidbanneddgankgfejfh?hl=ru&utm_source=ext_sidebar)
1) устанавливаете [Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
2) Нажимаете создать новый скрипт
3) Вставляете код ниже
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
    }, 2000);

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
    // todo check if dis loading
    var isDiscordAvailable = await loadImageByUrl('https://cdn.discordapp.com/attachments/702903037827743869/1296326660936699945/image.png?ex=6711e1c8&is=67109048&hm=43393827d3711e5c7d90e17fff372f73abfe07686de18a2c3d838789e6c79100&');
    console.log('isDiscordAvailable ' + isDiscordAvailable);

    const regex = /https?:\/\/[a-zA-Z0-9\.\/\-\_]+(?:\.jpg|\.jpeg|\.png|\.gif|\.bmp|\.tif|\.tiff|\.webp)/i;
    setInterval(() => {
        const messagesList = document.querySelectorAll('.seventv-chat-message-body');

        for (const messageBody of messagesList) {
            for (const messagePart of messageBody.children) {
                if (regex.test(messagePart.textContent.trim())) {
                    if (messagePart.textContent.trim().includes('discord')){
                        if (isDiscordAvailable){
                             messagePart.innerHTML = '<img src="' + messagePart.textContent.trim() + '">'
                        }
                    } else
                    {
                        messagePart.innerHTML = '<img src="' + messagePart.textContent.trim() + '">'
                    }
            }
            }
        }
    }, 1000);
})()
```
4) Включаете скрипт и обновляете страницу
