# ETHASH NASIL ÇALIŞIR?
  Memory Hardness, nonce ve blok başlığına bağlı olarak sabit bir kaynağın alt kümelerinin
seçilmesini gerektiren bir *proof of work* algoritmasıyla elde edilir. Bu kaynağa (birkaç gigabayt boyutunda) DAG adı verilir. 
(DAG bir grafik türüdür, genellikle verilerin veya işlemlerin belirli sırayla gerçekleştirilmesini temsil etmek için gösterilir. Yani, 
düğümler arasında ki yönlendirilmiş kenarlar, bir şeyin bir başka şeyden önce veya sonra gerçekleştiğini gösterir).

  DAG her an 30.000 blokta bir değiştirilir; buna dönem(epoch) adı verilir ~125 saatlik bir zaman aralığıdır. DAG blok yüksekliğine bağlı olduğundan önceden oluşturulabilir. Ancak, istemci blok oluşturmak için bu süreci beklemek zorundaysa ve DAG'leri önceden oluşturup önbelleğe almazsa, ağ her dönem(epoch) geçişinde 
büyük bir blok gecikmesi yaşayabilir. 



___
Algoritmanın (ETHASH) genel olarak izlediği yol şu şekildedir:
 
**1-** O noktaya kadar (bir bloktan önceki tüm başlıklarının tarandığı) blok başlıklarının tarayarak her blok için hesaplanabilen bir tohum vardır.

**2-** Tohumdan 16MB'lık bir sözde rastgele önbellek hesaplanabilir. Light istemciler öncelleği depolar.

**3-** Önbellekten, veri kümesindeki her öğenin önbellekten yalnızca az sayıda öğeye bağlı olması özelliğine sahip 1GB'lık bir veri kümesi oluşturulabilir.
Tam istemciler ve madenciler veri kümesini saklar. Veri kümesi zamanla doğrusal olarak büyür.

**4-** Madencilik, veri kümesinin rastgele dilimlerini alıp bunları bir araya getirmeyi içerir. Doğrulama, ihtiyaç duyduğunuz veri kümesinin belirli
parçalarını yeniden oluşturmak için önbellekten kullanılarak düşük bellekle yapılabilir; böylece yalnızca önbelleği saklamak gerekir.
