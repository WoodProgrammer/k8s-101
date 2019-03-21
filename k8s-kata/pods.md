# PODS 

<img src="pods.png"></img>

*  K8S ortamında tüm konteynır objeleri podlar içerisinde yaşamaktadır. K8S diğer sistemlerden ayrı olarak konteynırları pod adını verdiği üst seviye bir yapı üzerinden yönetmektedir.

* Podlar içerisinde bulunan konteynırlar aynı kaynaklara erişebilmektedirler . Pod içerisindeki konteynırlar kendi içinde de iletişim kurabilirler. (bkz. ingres, egress)

* K8S'in amacı itibariyle podların yüksek bir yük aldığı durumu otomatik bir biçimde artı veya eksi yönde ölçekleyebilmektir. 

 * Bir pod içerisinde birden çok konteynır olabilmektedir ama olası ölçekleme durumları için K8S'de bir pod içerisinde bir konteynır bulunması tavsiye edilmektedir.(istio, init containers)




