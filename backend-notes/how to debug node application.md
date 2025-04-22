
iki yol var : 

- normal vs code dan debugger ile kullanmak
- çalıştıracağınız kodu --inspect ile çalıştırmak : node --inspect index.js bu sayede chrome debugger kullanılabilir


ikinci yöntem için, bu kodu çalıştırdığınızda eğer bu tarayıcıda açılan bir frontend kodu döndüyorsa, tarayıcıda o taba gidip inspect diyip sol köşeye gelen yeşil node simgesine tıklayabilirsiniz.

eğer bu kod normal bir js ise, chrome://inspect sayfasına gidip orda remote target kısmına gelen remote a tıklayıp debugger vs yi açabilirsiniz.  ref video : https://www.youtube.com/watch?v=P9u4RTmso_g