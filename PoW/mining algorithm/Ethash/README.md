# ETHASH NASIL ÇALIŞIR?
  Memory Hardness, nonce ve blok başlığına bağlı olarak sabit bir kaynağın alt kümelerinin
seçilmesini gerektiren bir *proof of work* algoritmasıyla elde edilir. Bu kaynağa (birkaç gigabayt boyutunda) DAG adı verilir. 
(DAG bir grafik türüdür, genellikle verilerin veya işlemlerin belirli sırayla gerçekleştirilmesini temsil etmek için gösterilir. Yani, 
düğümler arasında ki yönlendirilmiş kenarlar, bir şeyin bir başka şeyden önce veya sonra gerçekleştiğini gösterir).

  DAG her an 30.000 blokta bir değiştirilir; buna dönem(epoch) adı verilir ~125 saatlik bir zaman aralığıdır. DAG blok yüksekliğine bağlı olduğundan önceden oluşturulabilir. Ancak, istemci blok oluşturmak için bu süreci beklemek zorundaysa ve DAG'leri önceden oluşturup önbelleğe almazsa, ağ her dönem(epoch) geçişinde 
büyük bir blok gecikmesi yaşayabilir. 
