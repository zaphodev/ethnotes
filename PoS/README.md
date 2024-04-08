<h1>Hisse İspatı (PoS)</h1>
 Hisse İspatı (PoS) doğrulayıcıların ağda dürüst olmayan bir hareket yaptıkları zaman yok edilebilecek değerli bir şey koydukları kanıtlamanın bir yoludur. Ethereum hisse ispatında, doğrulayıcıların Ethereum'da ki bir akıllı sözleşmeye ETH şeklinde açıkça sermaye yatırdığı hisse ispatı kullanılır. Doğrulayıcı, daha sonra ağ üzerinden yayılan yeni blokların geçerli olup olmadığını kontrol etmekten ve zaman zaman yeni blokları kendileri oluşturup yaymaktan sorumludur. Ağı dolandırmaya çalışırlarsa, hisseledikleri ETH'nin bir kısmı ya da tamamı yok edilebilir. 


 Bir doğrulayıcı olarak katılmak için, kullanıcının mevcut sözleşmesinde 32 ETH yatırması ve üç ayrı yazılım parçası çalıştırması gerekir. 
  

```Blok zamanlaması temp sabittir. => 32 yuva, dönem(12 saniye)```


___
# Tasdik Nedir?
Her dönemde bir doğrulayıcı ağa bir tasdik önerir. Bu tasdik, dönemdeki spesifik bir yuva içindir. Tasdikin amacı doğrulayıcının zincir görüşü yani spesifik olarak
en haklı görülen blok ile mevcut dönemdeki ilk blok için oy vermektedir. Bu bilgi tüm katılım sağlayan doğrulayıcılar için birleştiricidir ve ağın blok zincirin mevcut durumu üzerinde mutabakata varmasını sağlar. 


**Tasdik Bileşenleri:**

```aggregation_bits:``` Komitelerindeki doğrulayıcı endeks ile eşleşen pozisyonun bulunduğu bir doğrulayıcı bit listesi; değer (0/1) doğrulayıcının 'data'
imzalayıp imzalamadığını gösterir. (yani aktif olup olmadıklarını ve blok önericisi ile anlaşıp anlaşmadığını).


```data:``` Aşağıda tanımlandığı gibi, tasdik ile alakalı ayrıntılar.


```signature:``` Tekil doğrulayıcının imzalarını toplayan bir BLS imzası.
___


***Tasdikleyici bir doğrulayıcı için ilk görev 'data' inşaasıdır.***



**data** bilgileri (ayrıntıları):

```slot:``` Tasdikin değindiği yuva numarası.

```index:``` Verilen bir yuvadaki doğrulayıcının hangi kurula ait olduğunu belirten bir sayı.

```beacon_bloc_root:``` Doğrulayıcının zincirin başında gördüğü bloğun kök şifresi (çatal seçimi algoritmasının sonucu).

```source:``` Doğrulayıcıların neyi en güncel kabul edilebilir blok olarak gördüğünü belirten kesinlik oyununun bir kısmı.

```target:``` Doğrulayıcıların neyi mevcut dönemin ilk bloğu olarak gördüğünü belirten kesinlik oyununun bir kısmı.

**'data' inşa edildiğinde, doğrulayıcı 'aggregation_bits' içinde kendi doğrulayıcı endeksine delen biti 0'dan 1'e çevirerek katılım sağlandığı gösterilebilir.
Son olarak, doğrulayıcı tasdiki imzalar ve ağa yayınlar.**

Tasdik yaşam döngüsü aşağıdaki şemada belirtilmiştir:

https://ethereum.org/content/translations/tr/developers/docs/consensus-mechanisms/pos/attestations/attestation_schematic.png 


___ 



# Ödüller
 Doğrulayıcılar tasdikleri bildikleri için ödül alırlar. Tasdik ödülü iki değişkene bağlıdır; ```base reward (ana ödül)``` ve ```inclusion delay (dahil etme gecikmesi)```. *inclusion delay* için en iyi durum 1'e eşit olmasıdır. 


 ```(tasdik ödülü)attestation reward = 7/8 base reward x (1 / inclusion delay)```

 ``` base reward = validator effective balance x 2^6 / SORT ```

 *SORT*: Tüm aktif doğrulayıcıların etkili dengesi



___



# Kısaca Bir Tasdik Saldırısı (Olası)

- Saldırgan, bir bloğu(B) belirli bir zaman diliminde oluşturur ve bu blok için ispat(proof) sağlar.
- Ancak bir bloğu ağdaki diğer doğrulayıcılara açıklamaz ve saklar.
- Bir sonraki zaman diliminde (n+1), saldırgan yine bir blok oluşturur ve bu sefer bir bloğu ağa yayınlar.
- Daha sonra, saldırgan bir sonraki zaman diliminde (n+2) geçtiğinde, dürüst bir doğrulayıcı başka bir blok önerir(C).
- Aynı anda saldırgan önceki zaman diliminde oluşturduğu ve sakladığı bloğu(B) ve o blok için yaptığı ispatları yayınlar.
- Saldırgan blok B'nin varlığını inkar etme gücü kazanır, çünkü aynı anda iki farklı bloğu (B, C) yayınlar.
- Dürüst doğrulayıcı olan D, hangi bloğun daha ağır olduğunu seçer. Saldırgan, bu seçimde blok D'nin B üzerine inşa edilmesinin C üzerine inşa edilmesinden daha ağır olduğunu göstererek, D'nin C'yi seçmesini engeller.


*(Bu tür saldırıları önlemek için çeşitli dengeleme yöntemleri ve güvenlik önlemleri araştırılıyor).*

https://ethereum.org/content/translations/tr/developers/docs/consensus-mechanisms/pos/attack-and-defense/reorg-schematic.png



___




<h2>Doğrulayıcı Anahtarları</h2>
https://ethereum.org/content/translations/tr/developers/docs/consensus-mechanisms/pos/keys/validator-key-schematic.png

https://ethereum.org/content/translations/tr/developers/docs/consensus-mechanisms/pos/keys/multiple-keys.png
