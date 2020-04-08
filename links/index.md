---
title: Links
date: 2020-3-25 16:00:00
---

不分先后，欢迎联系我交换友链，联系方式在NavBar的About中

下面的链接卡片为2020.04.08加入的实验性功能，兼容性有待测试。

<style>
    .avatar {
        height: 50px;
        width: 50px;
    }
    
    .name {
        font-size: 1.2em;
        font-weight: bold;
    }

    .right {
        margin: 0px 10px;
    }

    .link-card {
        display: flex;
        flex-flow: row;
        min-width: max-content;
        width: 200px;
        padding: 10px;
        border: solid 2px black;
        border-radius: 10px;
        margin: 20px;
    }
</style>

<div id="link-container">
    <div class="link-card" style="display: none;">
        <img class="avatar">
        <div class="right">
            <div class="name">name</div>
            <div class="intro">intro</div>
        </div>
    </div>
</div>

<script>
    let req = new XMLHttpRequest();
    req.onreadystatechange = () => {
        if (req.readyState == 4 && req.status == 200) {
            let links = JSON.parse(req.responseText);
            links.forEach(link => {
                let linkNode = document.getElementsByClassName('link-card')[0].cloneNode(true);
                linkNode.setAttribute('style', '');
                linkNode.children[1].children[0].innerText = link.name;
                linkNode.children[1].children[1].innerText = link.intro;
                linkNode.getElementsByClassName('avatar')[0].setAttribute('src', link.avatar != undefined ? link.avatar : '');
                linkNode.onclick = () => {
                    window.open(link.link);
                }

                document.getElementById('link-container').appendChild(linkNode);
            })
        }
    }
    req.open("GET", "./links-info.json", true);
    req.send();
</script>

---

- [鸾喈尘の喷火梧桐树](https://darc.pro)
- [XYenon's Blog](https://blog.xyenon.bid)
- [Avimitin](https://avimitin.com)
