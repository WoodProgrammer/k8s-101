# PODS 

<img src="pods.png"></img>

*  K8S ortamında tüm konteynır objeleri podlar içerisinde yaşamaktadır. K8S diğer sistemlerden aynı olarak konteynırları pod adını verdiği üst seviye bir yapı üzerinden yönetmektedir.

* Podlar içerisinde bulunan konteynırlar aynı kaynaklara erişebilmektedirler . Pod içerisindeki konteynırlar kendi içindede iletişim kurabilirler (bkz. ingres, egress)

* K8S amacı itibariyle podları yüksek bir yük aldığı durum otomatik bir biçimde artı veya eksi yönde ölçekleyebilmektedir. 

 * Bir pod içerisinde birden çok konteynır olabilmektedir ama olası ölçekleme durumları içi K8S'de bir pod içerisinde bir konteynır bulunması tavsiye edilmektedir.(istio, init containers)




