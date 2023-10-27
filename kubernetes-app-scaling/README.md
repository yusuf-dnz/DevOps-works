# Scale App 

## Uygulama Adımları
Örnek olarak daha önce kullandığımız node.js web uygulamamızı bir docker container içerisinde barındıracak şekilde image oluşturalım.

![image](https://github.com/LOG-IN-DEVOPS-BOOTCAMP/kubernetes-assignment-2-yusuf-dnz/assets/101550162/4fbdec75-64ff-442d-9cd8-c478565200ed)

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
![image](https://github.com/LOG-IN-DEVOPS-BOOTCAMP/kubernetes-assignment-2-yusuf-dnz/assets/101550162/37a95a7c-181e-4097-9914-8f2d33768e68)

Uygulamamız şuanda ölçeklenebilir durumda deploy edildi, scale işlemini iki şekilde yapabiliriz.

Manuel olarak komut girerek bu işlemi yapabiliriz.
- başlangıçta 2 replica ile çalışan pod kümesini gösterilen komut ile manuel olarak ölçekleyebiliriz.
```shell
kubectl scale deployment scale-app-deployment --replicas=5
````
![image](https://github.com/LOG-IN-DEVOPS-BOOTCAMP/kubernetes-assignment-2-yusuf-dnz/assets/101550162/f9c90636-50cd-4829-8cb9-3e24bdc1bf82)

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


