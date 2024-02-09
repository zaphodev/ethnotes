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

3**3 = 27 
```
