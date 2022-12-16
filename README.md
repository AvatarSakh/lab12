<h2 align="center">  МИНИСТЕРСТВО НАУКИ И ВЫСШЕГО ОБРАЗОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ ОБРАЗОВАТЕЛЬНОЕ УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ «САХАЛИНСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ» </h2>
<div align="center">
<h3>Институт естественных наук и техносферной безопасности
<br>
Кафедра информатики
<br>
Половников Владислав Олегович</h3>

<br>
<h3>Лабораторная работа №6
<br>
“Введение в вэб-разработку”
<br>
01.03.02 Прикладная математика и информатика</h3>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<h3 align="right">Научный руководитель
<br>
Соболев Евгений Игоревич
</h3>

<h3 align="center">Южно-Сахалинск
<br>
2022г.
</h3>
<hr>
</div>
<p>
HTML (HyperText Markup Language) - язык разметки гипертекста - предназначен для создания Web-страниц.
Под гипертекстом в этом случае понимается текст, связанный с другими текстами указателями-ссылками.
HTML представляет собой достаточно простой набор кодов, которые описывают структуру документа. HTML позволяет выделить в тексте отдельные логические части (заголовки, абзацы, списки и т.д.), поместить на Web-страницу подготовленную фотографию или картинку, организовать на странице ссылки для связи с другими документами.
HTML не задает конкретные и точные атрибуты форматирования документа. Конкретный вид документа окончательно определяет только программа-браузер на компьютере пользователя Интернета. 
HTML также не является языком программирования, но web-страницы могут включать в себя встроенные программы-скрипты на языкахJavascript и Visual Basic Script и программы-апплеты на языке Java.
</p>

<h3 align="center">Задание</h3>

Задача (https://jsonplaceholder.typicode.com/):
Реализовать галерею на Node JS. Ваше приложение должно делать: 
-запрос к серверу, получать данные в формате json;
-обработка json и вывод пользователю информацию в виде веб-страницы;
-выводить список альбомов;
-выводить список фотографий;
-переход между альбомом и списком фотографий.


<h2 align="center">Решение</h2>

![img1](/images/1.png)

![img2](/images/2.png)

![img3](/images/3.png)

<h3>Файл index.html</h3>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
    <link href="./assets/css/style.css" rel="stylesheet">
    <script src="./assets/js/jquery-3.6.1.min.js"></script>
    <script src="./assets/js/script.js"></script>
    <title>Лабораторная 12</title>
    <script>
          
    </script>
</head>
<body>
  <h2 class="text-center" id="Header">Альбомы</h2>
  <div class="d-flex flex-wrap" id="albums">
    <button id="album" value="1" onclick="GetImage(this)">1</button>
  </div>

    <button type="button" class="btn btn-primary" id="buttonback" onclick="GetAlbums()">Назад</button>
  <div class="d-flex flex-wrap" id="images">
  </div>
</body>
</html>

```

<h3>Файл server.js</h3>

```javascript
const { json } = require('express');
const express = require('express')

const app = express()
const port = 3000;
const fs = require("fs");


app.use(express.static("."));

app.get((req, res) => {
    res.sendFile(".\\index.html")
 })
 
 app.get("/albums", function(req, res){
    var content = fs.readFileSync("./assets/json/albums.json","utf8");
    var listOfAlbms = JSON.parse(content);
    res.send(listOfAlbms);
});

app.get("/images/:id", function(req, res){
    var id = req.params.id; 
    var content = fs.readFileSync("./assets/json/photos.json","utf8");
    var ListOfPhotos = JSON.parse(content);
    var photos = [];
    for(var i=0; i<ListOfPhotos.length; i++){
        if(ListOfPhotos[i].albumId==id){
            photos.push(ListOfPhotos[i]);
        }
    }
    res.send(photos);
});


 app.listen(port, () => console.info(`Server running on ${port}`));
```

<h3>Файл assets/css/style.css</h3>

```css
h2{
    margin-top: 15px;
    font-size: 50px;
    
}
body{
    background-image: url("../image/bg.png");
    background-size: 100%;    
    background-repeat: repeat-y;
}
#album{
    background: url("../image/Album.png") rgba(0,0,0,0);
    background-size: 100% 100%;
    color: black;
    padding: 35px;
    margin: 10px;
    width: auto;
    height: auto;
    border: 0px;
}
#album:hover{
    border: 1px solid black;
}
#image{
    margin: 20px;
    height: 150px;
    width: 150px;
}
#buttonback{
    position: absolute;
    top: 2%;
    left: 2%;
    width: 100px;
    height: 50px;
    font-size: 20px;
}
```

<h3>Файл assets/js/script.js</h3>

```javascript
async function GetAlbums() {
    const response = await fetch("/albums", {
        method: "GET",
        headers: { "Accept": "application/json" }
    });
    if (response.ok === true) {
        const albums = await response.json();
        var maindiv = document.getElementById("albums"); 
        images.innerHTML = "";
        document.getElementById("Header").innerText = "Альбомы";
        document.getElementById("buttonback").style.display = "none";
        albums.forEach(album => {
            maindiv.innerHTML += `<button id="album" value="`+album.id+`"onclick="GetImage(this)">`+album.id+"</button>";
        });
    }
}
GetAlbums();

async function GetImage(button) {
    var id = button.value;
    document.getElementById("Header").innerText = "Альбом: " + id;
    document.querySelectorAll("#album").forEach(a=>a.style.display = "none");
    document.getElementById("buttonback").style.display = "block";
    const response = await fetch("/images/" + id, {
        method: "GET",
        headers: { "Accept": "application/json" }
    });
    if (response.ok === true) {
        var photos = await response.json();
        var images = document.getElementById("images"); 
        console.log(photos);
        photos.forEach(photo => {
            images.innerHTML += `<img src="`+photo.url+`" id="image">`;
            console.log(photo.url);
        });
    }
}
```



<h4>Вывод: Укрепил знания в Node js и больше узнал о json.</h4>

