# Deployments

* K8S kendisi içerisinde sürekli soyutlama katlamanları bulundurmaktadır, konteynır üzerinde podlar, podlar üzerinde ise deployment katmanı bulunmaktadır. 


Deploymentların birincil amaçları, pod kümelerini birlikte yönetebilmektir. Bir pod durduğu zaman yenisini yaratmayı veya olası durumda yeniden başlatmayı yine yönetmektedir.

Bunun yanında tüm podların sayısını sürekli tutmayı veya bunlara bağlı olarak monitor edilmesini de sağlamaktadır .



Bütün bunlar haricinde, CI ve CD süreçlerinde de deployment katmanı podların roll-up ve roll-back yapabilmek için beraberinde yönetilebilmektedir. 

Geçmişi tutabilmek için --record ekleyerek historyleri tutabilirsiniz . 

  Deployment objeleri geçmişi kendi içerisinde tutmaktadırlar. 
  

    kubectl rollout history deployment/$DEPLOYMENT
   
    kubectl rollout history deployment/$DEPLOYMENT --revision 11
    
    kubectl rollout undo deployment/$DEPLOYMENT

    kubectl rollout undo deployment/$DEPLOYMENT --to-revision 21

    
    
Ayrı bir yöntem olarak kendiniz patch komutunu kullanarak rolling-back, roll-out yapabilirsiniz .


     kubectl patch deployment $DEPLOYMENT \
        -p'{"spec":{"template":{"spec":{"containers":[{"name":"site","image":"$HOST/$USER/$IMAGE:$VERSION"}]}}}}'
        



## Makefile : 



    NAME ?= awebsome
    STAGE ?= staging
    K8S_DEPLOYMENT ?= $(NAME)-$(STAGE)
    VERSION ?= $(shell git describe)

    .PHONY: rollout rollback

    rollout:
        kubectl patch deployment $(K8S_DEPLOYMENT) \
            -p'{"spec":{"template":{"spec":{"containers":[{"name":"$(NAME)","image":"$(DOCKER_REGISTRY)/$(DOCKER_REGISTRY_USER)/$(NAME):$(VERSION)"}]}}}}'

    rollback:
        kubectl rollout undo deployment/$(K8S_DEPLOYMENT)
        
        
        
        
# Deploymentlarda Yaşam Döngüsü 

### Readness Probe : Uygulama ayağa kalkarken kontrol etmenizi sağlamaktadır. TCP, Http , ping (ICMP) ile kontrol edilebilir .

### Liveness Probe : Uygulama yaşamı boyunca kontrol yapmaktadırlar . 

Örnek bir Jenkins uygulaması health checkleri ..


          livenessProbe:
            httpGet:
              path: /login
              port: 8080
            initialDelaySeconds: 60
            timeoutSeconds: 5
            failureThreshold: 12 # ~2 minutes
          readinessProbe:
            httpGet:
              path: /login
              port: 8080
            initialDelaySeconds: 60
            timeoutSeconds: 5
            failureThreshold: 12 # ~2 minutes
            
            

K8S Ortamında RollingUpdate Yaparak 
Bir uygulamayı tüm envlere atabilirsiniz 
Geçmiş versiyonlara geri dönebilirsiniz 
