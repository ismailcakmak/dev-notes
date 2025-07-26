Öncelikle node kurulduğu için 
`npm init -y` diyerek default ayarlarda node projesi kurarız ve package.json gelir.

ardından main.js, index.js, script.js nasıl isimlendirirseniz bir js dosyası oluşturuyoruz ve bu dosya ana scriptimizi içerecek.

sonrasında terminal scriptleri belirleyelim, genel olarak şu 3 script'i yazmak iyidir :
- start
- dev
- test

start normal :  `node index.js`

dev için nodemon kurmak iyidir, çünkü otomatik olarak her seferinde yeni bir değişiklik yaptığında kendiliğinden server'ı yeniden çalıştırır.

nodemon şöyle indirilir : 

`npm install --save-dev nodemon`

ardından dev script'i de dahil scriptlerimiz şu şekilde olur : 

```js
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": ""
  }
}
```

şuanlık test için bir şey yapmayalım
