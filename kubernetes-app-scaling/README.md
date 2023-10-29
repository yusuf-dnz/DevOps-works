# Scale App 

## Uygulama Adımları
Örnek olarak daha önce kullandığımız node.js web uygulamamızı bir docker container içerisinde barındıracak şekilde image oluşturalım.

![image](https://github.com/yusuf-dnz/DevOps-works/assets/101550162/945b1018-7444-4d94-b151-2b498aac4d1a)

Docker imajını oluşturmak için aşağıdaki komutu kullanın:
```shell
docker build -t scale-app:latest .
````
"scale-app-deployment.yaml" dosyası içerisinde deployment konfigürasyonunu yapıp deploy edelim.
```shell
kubectl apply -f scale-app-deployment.yaml
````
Yük dengelemesi için LoadBalancer servisini kullanacağız, gerekli konfigürasyonu yapıp deploy edilmeli.
```shell
kubectl apply -f LoadBalancer.yaml
````
![image](https://github.com/yusuf-dnz/DevOps-works/assets/101550162/1976fe47-1857-4c49-b5ff-277ef6027d09)

Uygulamamız şuanda ölçeklenebilir durumda deploy edildi, scale işlemini iki şekilde yapabiliriz.

Manuel olarak komut girerek bu işlemi yapabiliriz.
- başlangıçta 2 replica ile çalışan pod kümesini gösterilen komut ile manuel olarak ölçekleyebiliriz.
```shell
kubectl scale deployment scale-app-deployment --replicas=5
````
![image](https://github.com/yusuf-dnz/DevOps-works/assets/101550162/097b11d8-b637-4370-9a81-72ef4fcd2b20)

Ölçekle işlevi bu kadarla kalmamakta, Auto Scaling yöntemi ile kaynak ve belirli metrikler belirterek uygulamanızın ölçeklenmesini ve gelen request'lere dayanıklı hale gelmesini sağlayabilirsiniz.
- Horizontal ve vertical scale çeşitleri uygulamanızı kullanım senaryosuna göre ölçeklemenize olanak verir, Horizontal pods autoscale(HPA) yatay ölçeklendirme için:
 ```shell
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
.
.
.
  minReplicas: 1
  maxReplicas: 6
  targetCPUUtilizationPercentage: 50

````
"autoscaling/v1" için gerekli yaml dosyasını oluşturup deploy edin. Belirlenen kaynak değerleri için Apache Benchmark veya BusyBox kullanarak sisteme yük bindirebilir ve dinamik ölçekleme yapısını inceleyebilirsiniz.
```shell
kubectl apply -f scale-app-hpa.yaml
````


