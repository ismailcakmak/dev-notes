
same-origin-policy ve CORS öğrenmeden önce, browserlarına çalışma mantığıylaa ilgili bilmemiz gereken bazı kritik şeyler var, önce bunları öğrenmeliyiz.

browser ile bir websiteyi yüklediğinizde diyelim ki, www.youtube.com yüklediniz. buna ilk request i attığınızda, browser hemen bir executing context oluşturur. Örneğin başka bir tab da da www.localhost:3001 e istek gönderdiniz. Browser Your browser creates separate execution contexts for each origin so that : 
- each javascript from a site runs in its own isolated sandbox
- This JavaScript cannot directly access DOM elements, cookies, or localStorage from different origins

browserların her tab için farklı execution context yaratıp, bu execution contextlerin isolated olması aslında same-origin-policy'nin bir parçasıdır.


same-origin-policy aslında tamamiyle güncel browserların dayattığı bazı kurallardır. bu kurallar client ın güvenliği için browserlar tarafından uygulanır.

eğer browser kullanmadan curl vs ile istek gönderip alırsanız bu kurallara bağlı kalmak zorunda kalmazsınız.

Peki nedir bu kurallar yani same-origin-policy :

Same origin-policy temel olarak 3 ayaktan oluşur : 

## The Three Main Aspects of Same-Origin Policy

1. **DOM Access Restriction**: JavaScript running on one origin cannot read or modify the DOM (document object model) of documents from another origin. This is the protection I described in the bank example.
2. **Network Request Restriction**: Browsers restrict certain types of cross-origin requests, particularly those that can read responses. This is the aspect you were initially focusing on.
3. **Cookie Access Restriction**: Scripts can only read cookies that belong to their own origin, preventing credential theft.


Diyelim ki bir tabınınzda online bankacılığa girdiniz, diğer tabınızda da bir malicious siteye girdiniz. same-origin-policy olmasa idi, bu malicious sitenin yüklendiğinde size gönderdiği kod, bankacılık tabınızdaki html elementlerine erişebilirdi ve mesela kart numaranız veya cnn kodunuzu html elementlerinden okuyabilirdi ve kendi serverına bu bilgiyi gönderebilirdi. Veya banka uygulamasının kaydettirdiği cookieleri okuyup banka giriş bilgiilerinze erişebilirdi, ve yine aynı şekilde bu bilgileri kendi serverlarına gönderebilirdi.


yukardaki örnekte same-origin-policy'nin 1 ve 3. özelliklerinin neden faydalı oldıuğunu anlıyoruz.  Fakat bir de 2.madde var, network requestler hakkında da bir dayatma getiriyor.

şimdi gelin bunun ne olduğundan önce bir örnek verelim bu bu örnkten sonra ne olduğunu ve neden öönemli olduğunu otomatik olarak anlayacaksınız.

Diyelim ki bir malicious siteye tıkladığınız ve bu site size bir js döndürdü, browserınız bunu çalıştırdı, ve bu js www.newhouse.com a bir istek atıyor diyelim. dolayısıyla bu adrese sizin broswerınızdan yani sisteminizden bir istek gönderildi. Burda bir sorun yokmuş gibi duruyor, fakat sizin gibi 10.000 insan daha bu siteye malicious siteye tıkladı diyelim, tüm bu insanların bilgisayrından da www.newhouse.com a istek gönderildi bu site aslında DDOs saldırısına uğradı.

Veya başka bir örnek daha verelim, diyelim ki hackerların elinde çalıntı bir banka veya kripto hesap bilgisi mevcut, ve bu hesap bilgileirni kullanarak bankaya girip yapıp bu parayı başka bir yere göndermek istiyorlar. bunu kendi bilgisyarından yapabilirler, fakat bir hacker iseniz bunu kendi biigisayarınızdan yapmaktan daha güzel bir şey olarak bunu başkasının bilgisayarında yapmaktır. Bu sebeple hackerlar malicious sitenin size gönderdiği ve sizin browserınınzda yolladığı javasctipt kodunu öyle bir yazabilirler ki siz farkında bile olmadan sadece bu siteye bağlanarak, browserınızın çalıştırdığı js kodu ile sizin sisteminiz bu çalıntı hesap bilgileriyle banka hesabına bağlanır, gerekli gönderme işlermlerini yapar. Ardından bu suçun hesabını siz vermek zorunda kalırsınız, çünkü tüm işlemler sizin sistemiz tarafından yapılmıştır!

İşte same-origin-policy nin network ayağı buna benzer, tarayıcınızın başka kişilerin zombi bilgisayarı olarak kullanılmasını engeller, bunu ise söyle yapar, yazının başında browser her tab için bir excution context oluşuturur demiştik hatırlarsınız ve bu execution context'i bağlandığınız origin e özel olarak oluşturur. Tarayıcı ardından şunu yapar, bir origin e spesifik bir execution context'ten başka bir origin'e istek yollanmasına izin vermez.

Yani diyelim ki siz şu adrese bağlandığınız : www.malicioussite.com, ve bu site size bie js gönderdi be bu js www.bank.com a bir istek gönderen bir kod içeriyor. tarayıcınız bu exevution context'teki origin'i malicioussite.com olduğundan ve bank.com olduğundan, bank.com'a istek gönderilmesini engeller. bu execution context sadece malicioussite.com a istek gönderip alabilir. 

Akıılıca değil mi?

Evet öyle.

Peki aklınıza şu geldi mi, örneğin twitter.com (artik x.com) da gezerken insanların paylaştıkları youtube videosuna veya blog post a nasıl gideceğiz? sonuc olarak twitter.com a bağlandığımızda execution context başka bir origin e istek gönderip almamız izin vermiyordu?

Eminim böyle bir twitter deneyimi çok sıkıcı olurdu. Olmasın diye CORS dediğimiz bir sistem geliştirildi. CORS (Cross-Origin Resource Sharing) aslıdan same-origin-policy nin netwotrk ayağını bazı durumalrda gevşetmeye yarıyor. Yani proper bir CORS ayarı yapıldığında browserınız execution context'ten farklı originlerle de bağlantı kurabiliyor.

Şimdi böyle diyince aklınıza muhtemelen şöyle bir şey geldi, biz gidip browser'a biliyorum sen şu twitter.com originine sahip execution contexte youtube.com a izin vermiyorsun ama ben sana söylüyorum ve artık izin vermelisin. Fakat durum böyle değil, CORS client yani browser ile ilgili değil tamamiyle server ile ilgili bir durumdur. 

Yani olan şey şudur : 
youtube server'ının, twitter origininden bana bağlanabilirsin için izin vermesi gerekir.

