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
