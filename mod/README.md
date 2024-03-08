# MOD ALMA

*mod -> %*,   kalanı ifade eden matematiksel operasyon 

```a mod b = r```

a = bölünen sayı 

b = bölen sayı

r = kalan sayı
___

Belirtildiği gibi DAG üretimi için kullanılan RNG(Rastgele Sayı Üreteci), sayı teorisinden elde edilen bazı sonuçalara dayanmaktadır. Öncelikle *picker* değişkenine temel olan Lehmer RNG'nin geniş bir periyoda sahip olduğunun
güvencesiyle sağlanıyor. İkinci olarak, *pow(x,3,P)* ' nin x'i 1'e veya başlangıç için x E[2, P-2] olması koşuluyla P-1'e eşlemeyeceği gösteriliyor. 

Son olarak *pow(x, 3, P)* 'nin karma işlevi olarak ele alındığında düşük 
çarpışma oranına sahip olduğu gösteriliyor.


Örneğin:
```
x=3, P=11

(3, 3, 11) = 27 mod 11 = 5

3^3 = 27 
```


# Lehmer Random Number Generator 
*produce_dag* işlevi tarafsız rastgele sayılar üretmesi gerekmese de, ```seed^i % P```'nin yalnızda bir avuç değer alması potansiyel bir tehdittir(bu modeli tanıyan madenciler, tanımayanlara göre avantaj sağlayabilir).
 Tohumun(seed) i kuvveti P ile modunu alırken küçük sayılar almak sınırlı bir dizi değeri alabilir. Bu durumda, üretilen rastgele sayılar belirli bir desene veya küçük bir sayı kümesine sıkışabilir.

 Örneğin, seed=2 P=10 olsun:
```
1 seed mod P => 2^1 mod 10 = 2  !!
2 seed mod P => 2^2 mod 10 = 4
3 seed mod P => 2^3 mod 10 = 8
4 seed mod P => 2^4 mod 10 = 6
5 seed mod P => 2^5 mod 10 = 2  !!
```
Burada döngü başlıyor, çünkü 2^5 ile 2^1 aynıdır. Bu durumda ifade 2,4,6,8 değerlerine sıkışmış oluyor.

Bu tür açıklardan kaçınmak için sayı teorisiden elde edilen bir sonuca başvurulur. SafePrime(Güvenli Asal), ```(P-1)/2```'de asal olacak şekilde bir asal P olara tanımlanır. Multiplicative Group(Çarpımsal Grup) Z/nZ üyesinini x üyesinin sırasını minimum m olarak tanımlanır. 

Örnekle:

# Gizli Anahtar Değişimi (Diffle-Hellman Anahtar Değişimi)

* Alice ve Bob güvenli bir şekilde anahtarlarını paylaşmak istiyorlar.
* Alice ve Bob her biri için rastgele bir özel anahtar seçer(a,b).
* ```g^a mod P``` ve ```g^b mod P``` hesaplanır ve bu değerler karşı tarafa paylaşılır. Her iki tarafta ```g^ab mod P``` değerini hesaplayabilir ve bu ortak anahtar olarak kullanılabilir.  (g = çarpan grubu elemanı(generator))
* Elinde sadece ```g^a mod P``` ve ```g^b mod P``` olan bir saldırgan, gerçek anahtarı elde etmesi oldukça zordur, çünkü özel a ve b değerlerini bilmeksizin hesaplanamaz.

___

# RSA Şifrelemesi

RSA şifreleme algoritması, 
örneğin:

* Alice, Bob'un açık anahtarını kullanarak mesajını şifreler.
* Bob mesajı almak için özel anahtarını kullanarak şifreyi çözer.
* Eğer saldırgan, mesajı almışsa ve şifresini çözmeye çalışıyorsa, bu işlem [Safe Prime](https://en.wikipedia.org/wiki/Safe_and_Sophie_Germain_primes) ve [Multiplicative Group](https://en.wikipedia.org/wiki/Multiplicative_group_of_integers_modulo_n) dayandığı için çok zor olacaktır.



___

# Observation 1
Güvenli bir asal P için, Z/PZ çarpımsal grubunun bir üyesi olsun. Eğer, 
```x mod P ≠ 1 mod P``` hem de ```x mod P ≠ P-1 mod P``` ise, o zaman x'in sırası *P-1* ya da *(P-1)/2* olmalıdır.

Örneğin: 

P=11, x=2 olsun,

```2 mod 11 ≠ 1 mod 11``` ve ayrıca ```2 mod 11 ≠ 10 mod 11``` 
Observation 1'e göre x=2 elemanının sırasıyla ya ```11-1=10``` ya da ```(11-1)/2 = 5``` olmalıdır.

=> Gerçekten de ```2^^10 mod 11 = 1``` ve ```2^^5 mod 11 = 10``` olduğu görülür. 


___

# Multiplicative Group

Bir elemanın çarpan grubunda ki sırası, örnekle:

*mod = 7 üzerinden çalışalım ve g = 3 olsun.*

```
g^1 mod 7 => 3^1 mod 7 = 3
g^2 mod 7 => 3^1 mod 7 = 2
g^3 mod 7 => 3^1 mod 7 = 6
g^4 mod 7 => 3^1 mod 7 = 4
g^5 mod 7 => 3^1 mod 7 = 5
g^1 mod 7 => 3^1 mod 7 = 1
```

Burada ```g = 3``` elemanın çarpan grubunda ki sırası(order) (g)=6 oluyor. Çünkü ```g^6 mod 7 ≠ 1 mod 7```    (burada m < 6)

=> Eğer g elemanının sırası küçükse, bu durum "Dönme Saldırısı" gibi zayıf noktaları açığa çıkarabilir.



___

[Fermat's Little Theorem'ine](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem) göre x'in sırası 1 olamaz. 

Dolayısıyla x, benzersiz olan Z/nZ'nun çarpımsal kimliği olmalıdır. Varsayım olarak ```x ≠ 1``` olduğunu varsaydığımız için bu mümkün değildir. 

```x = P-1``` olmadıkça sırası 2 olamaz çünkü bu, P'nin asal olmasını ihlal eder.

  Yukarıda ki önermeden, yinelenen ```(picker*init)%P```nin en az ```(P-1)/2``` döngü uzunluğuna sahip olacağını anlayabiliriz. Bunun nedeni, P'yi yaklaşık ikinin daha büyük kuvvetine eşit güvenli bir asal olarak seçmemiz ve *init*'in ```[2,2^256+1]``` aralığında olmasıdır. P'nin büyüklüğünü göz önüne alırsak, modüler üstellikten asla bir döngü beklememeliyiz.


DAG'de ki ilk hücreyi (*init etiketi değişkeni*) atarken ```pow(sha3(seed)+2,3,P)``` hesabı yapılır. İlk bakışta bu, sonucun ne 1 ne de P-1 
olacağını garanti etmez. Bununla birlikte, P-1 güvenli bir asal olduğundan, Observation 1(Gözlem 1)'in doğal sonucu olan aşağıda ki ek güvenceye sahip;

**Observation 2**(Gözlem 2), Güvenli bir asal *P'nin x, Z/PZ* multiplicative group'un bir üyesi olsun ve *w* bir doğal sayı olsun. 
Eğer ```x mod P ≠ 1 mod P``` ve ```x mod P ≠ P-1 mod P```, ayrıca ```w mod P ≠ P-1 mod P``` ve ```w mod P ≠ 0 mod P``` ise, o zaman ```x^w mod P ≠ 1``` ve ```x^w mod P ≠ P-1 mod P```



___

# Karma İşlevi Olarak Modüler Üstel Alma 
 *P ve w*' nin belirli değerleri için (x, w, P) fonksiyonunun birçok çarpışması olabilir. Örneğin pow(x, 9, 19) yalnızca {1, 18} değerlerini alır. 
  P'nin asal olduğu göz önüne alındığında, modüler bir üstel karma işlevi için uygun bir w, aşağıda ki sonuç kullanılarak seçilebilir:

  **Observation 3**, P bir asal olsun; *w ve P-1* göreceli olarak ancak ve ancak Z/PZ'da ki tüm a ve b için; ```a^w mod P ≡  b^w mod P``` ancak ve ancak 
  ```a mod P ≡ b mod P``` ise 

  Böylece, P'nin asal ve w'nin P-1'e göreli olarak asal olduğu göz önüne alındığında **```|{pow (x, w, P) : x ∈ Z}| = P```**, karma fonksiyonunun mümkün olan minimum çarpışma oranına sahipm olduğunu ima eder.


**Göreceli Asal Nedir?**

Örneğin: 

4'ün bölenleri : 1, 2, 4 
15'in bölenleri : 1, 3, 5, 15 

*(Ortak bölenleri yoktur. Bu nedenle, 4 ve 15 göreceli asal sayılardır.)*
