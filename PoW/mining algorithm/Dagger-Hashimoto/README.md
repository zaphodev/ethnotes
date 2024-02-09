# DAG Generation 
İlk olarak belirtilen hassasiyette ki işaretsiz girişleri dizelere sıralamak için encode_int'i verilir.

```
NUM_BITS = 512
def encode_int(x):
    "Encode an integer x as a string of 64 characters using a big-endian scheme"
    o = ''
    for _ in range(NUM_BITS / 8):
        o = chr(x % 256) + o            //x'in en düşük 8 biti alınarak ASCII karakterine dönüştürülüp dizgenin başına eklenir.
        x //= 256
    return o
```

Tam tersi de: 
```
def decode_int(s):
    "Unencode an integer x from a string using a big-endian scheme"
    x = 0
    for c in s:
        x *= 256
        x += ord(c)
    return x
```

Büyük uçlu düzen kullanılarak bir tamsayıyı 64 karakterlik diz dizgeye kodlar ve aynı düzenden bu dizgeyi çözer. 
'encode_int' fonksiyonu 8 bitlik bloklara böler -> ASCII -> birleştirir.

___

# PARAMETRELER 

SAFE_PRIME_512 = 2**512 - 38117 

```
params = {
"n": 4000055296 * 8        //NUM_BITS, (Veri kümesinin boyutu)
"n_inc": 65536,            // dönem başına n değerindeki artış

"cache_size": 2500,        // hafif istemcinin önbelleğinin boyutu

"diff": 2**14,             // zorluk

"epochtime": 100000,       // blok cinsinden bir dönemin uzunluğu, veri kümesinin ne sıklıkla güncellendiği
"k": 1,                    // bir düğümün ebeveyn sayısı
"w": w                     // modüler üstel karma işlemi için kullanılır.
"accesses": 200,           // hashimoto sırasında veri kümesi erişim sayısı
"p": SAFE_PRIME_512        // karma ve rastgele sayı üretimi için SAFE PRIME
}
```

___

# Dagger Grafigi Oluşturma

```
def produce_dag(params, seed, length):
    P = params["P"]
    picker = init = pow(sha3(seed), params["w"], P)        // parametreler sözlüğünden değerler alınıyor ve sha3 ile başlangıç değeri oluşturuluyor
    o = [init]
    for i in range(1, length):                             // 1'den lenght'e kadar döngü
        x = picker = (picker * init) % P                   
        for _ in range(params["k"]):
            x ^= o[x % i]
        o.append(pow(x, params["w"], P))
    return o
```

 picker bir seçici değişkeni olarak kullanılır, init bir başlangıç değeri veya tohumdur ve P bir sabit veya modül değeridir.

*x = picker = (picker * init) % P ifadesi*, yeni bir picker değeri hesaplamak için kullanılır. picker değeri önceki bir değere (genellikle önceki blokun picker değeri) dayanır ve bu değeri güncellemek için kullanılır. Bu güncelleme işlemi, genellikle bir çeşit rastgelelik ekler ve farklı madencilerin farklı değerlerle denemeler yapmasını sağlar.

Grafikteki her bir düğümün, yalnızca az sayıda düğümden oluşan bir alt ağacı hesaplanarak ve yalnızca az sayıda yardımcı bellek gerektirerek yeniden yapılandırabilmesini sağlamaktadır. *k = 1* durumunda alt ağacın yalnızca DAG'da ki ilk öğeye kadar giden bir değerler zinciri olduğuna dikkat edin. (Grafikteki her bir düğüm, sadece bir kaç düğüm bilgisini kullanarak ve çok az bellek kullanarak oluşturulabilir.) 
