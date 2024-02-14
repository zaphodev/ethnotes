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
