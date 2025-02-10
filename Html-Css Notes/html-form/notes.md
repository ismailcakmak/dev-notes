
# Client Side Validation
Not : required verilen her şey doldurulmak zorundadır ve valid olmalıdır. aksi takdirde invalid kabul edilir ve css de :invalid uygulanır. valid olduğunda ise :valid uygulanır.

Text inputs : text, email, textarea   -> uses <input> and <textarea>

Validation :
minlenght, maxlenght, pattern
bazı text inputlar, mesela email kendisi de text validation yapıyor.


Numeric inputs : number, range and date -> uses <input>

Validation : max, min, step

date için farklı farklı bir sürü zaman ve tarih seçme türü var : date, month, time etc

Radio buttons : aralarından biri seçilir sadece -> uses <input>

Validation : mesela bir grup radio button olsun, 5 tane. herhangi birine required eklendiği anda 
hiçbiri seçili değilse invalid kabul edilir, eğer biri seçili ise valid olur.

checkbox : birkaç tanesi birlikte de seçilebilir
Validation : eğer checkbox grubundan birine required verilirse, o verilen sadece required olur, ona tıklanması zorunludur formu göndermek için, diğerlerine tıklanmasa da olur.

