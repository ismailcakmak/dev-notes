
pseudo class, bir elementi seçen class gidibir, seçtiği elementine tamamına apply lanır.

pseudo element ise bir elemente yeni bir element eklemek gibidir.

bunların ne demek olduğunu anlamak için aşağıdaki örneklerre göz atın, ardından tekrar okuyun daha iyi anlayacaksınız : 


örneğin bir article yazacağız, fakat bu article in her rzaman ilk paragrafının bold olmasını istiyoruz. her seferinde article değiştiğinde gidip yeni paragrafa class eklemek yerine :first-child kullanabiliriz.

html örneğine bakabilirsiniz.



diyelim ki bu article in sadece ilk paragrafını bold lamak yerine her paragrafın ilk satırını renklendirmek istiyoruz. o zaman pseudo element kullanabiliriz : 

html örneğine bakabilirsiniz.


fakat bazı pseudo elementlerin farklı bir özelliği var, ::before veya ::after ile elementlere ekleme yapabiliriz.
