# StatefulSets 

* K8S ortamında state bağımlı uygulamalarımızın, kendi dosya sistemi içerisinde tuttukları veriyi olası çökme durumlarına karşın yeniden ayağa kalktığında erişebilmesi için StatefulSet katmanı ile deploymentlarınızı yönetmeniz gerekebilmektedir. 


	    apiVersion: apps/v1
	    kind: StatefulSet
	    metadata:
	      name: web
	    spec:
	      serviceName: "nginx"
	      replicas: 2
	      selector:
		matchLabels:
		  app: nginx
	      template:
		metadata:
		  labels:
		    app: nginx
		spec:
		  containers:
		  - name: nginx
		    image: k8s.gcr.io/nginx-slim:0.8
		    ports:
		    - containerPort: 80
		      name: web
		    volumeMounts:
		    - name: www
		      mountPath: /usr/share/nginx/html
	      volumeClaimTemplates:
	      - metadata:
		  name: www
		spec:
		  accessModes: [ "ReadWriteOnce" ]
		  resources:
		    requests:
		      storage: 1Gi



Yukarıda görüldüğü üzere mountpath altında verilen dosyalar eğer ki statefulset objesine bağlı podlar çökerse veya podu silseniz bile statefulsetler içerisindeki data daima kalıcı olarak yazılması istenen yerde yazılacaktır. 

Bütün bunlara bağlı olarka bakıldığında statefulsetler birçok opsiyonu kendi içerisinde barındırmaktadır. 
 VolumeClaimTemplates içerisinde  

	metadata:
      name: data
        annotations:
          volume.beta.Kubernetes.io/storage-class: miniosc
          
          
          
* Minio'ya bağlı S3 ya da CEPH gibi opsiyonlar ekleyebilirsiniz . Statefulset objeleri deployment objelerini, buna bağlı kalıcılığı sağlamak içinde Persisten Volume Claim (PVC) objeleri oluşturmaktadır. PVC sayesinde stateler kalıcı hale gelmektedir .

### Access mode bir podun o volume nasıl erişebileceğini düzenlemektedir . 

* ReadWriteOnce – Sadece bir nodun yazmasına izin veriyor.
* ReadOnlyMany –  Birden çok nodun okumasına izin veriyor.
* ReadWriteMany – Birden çok nodun yazmasına izin veriyor.

