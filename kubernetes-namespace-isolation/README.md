# Namespace App
## Uygulama Adımları
Öncelikle bir Node.js app oluşturuyoruz.

![image](https://github.com/yusuf-dnz/DevOps-works/assets/101550162/009c1803-7f78-4f53-b4a5-81ec9665ecc7)


Node.js web uygulamamızı içeren iki farklı docker image oluşturmamız isteniyor.
```shell
docker build -t web-app:v1 .
docker build -t web-app:v2 .
````
![image](https://github.com/yusuf-dnz/DevOps-works/assets/101550162/1357f71e-5485-4602-8f2f-a73f5cc8619d)


Bu iki image farklı ortamlarda izole çalışsın isteniyor. Bunun için default namespace yerine uygulamamızı dağıtabileceğimiz iki ortam oluşturalım.
```shell
kubectl create namespace web-v1
kubectl create namespace web-v2
````
![image](https://github.com/yusuf-dnz/DevOps-works/assets/101550162/2a4e7bb2-c8a7-49ba-ba84-33e3962375aa)


Oluşturduğunuz ortam isimleri bu komutla listeleyebilirsiniz.
```shell
kubectl get namespace 
````
Şimdi her ortamda çalışmak üzere bir Deployment ve bir Service oluşturalım. "metadata" içerisinde uygulamanızı dağıtmak isteğiniz ortamı belirtin.
```shell
metadata:
  name: web-app-deployment-v1
  namespace: web-v1
````
Her ortam için oluşturduğumuz ayrı image leri deploy edeceğiz.
```shell
      containers:
        - name: web-app
          imagePullPolicy: IfNotPresent
          image: web-app:v1
````
Deploy komutları sırasıyla:
```shell
kubectl apply -f web-app-deployment-v1.yaml
kubectl apply -f web-app-deployment-v2.yaml
kubectl apply -f web-app-service-v1.yaml
kubectl apply -f web-app-service-v2.yaml
````
![image](https://github.com/yusuf-dnz/DevOps-works/assets/101550162/861b550c-17e7-4a24-8ec1-5f720f2593aa)


Kendi namespace leri içerisinde dağıttımız pod lara başka ortamdan erişim yapamıyoruz. Bu yüzden içerisindeki pod lara namespace üzerinden erişim sağlayıp uygulamamıza erişim sağlayabiliriz.
- -n flag i kullanarak hedef namespace i belirtiriz
```shell
kubectl exec -n web-v1 web-app-deployment-v1-696b79c974-hc5lr -it bash
````
Bu şekilde pod içerisine erişip izole ortamımızda gerekli değişiklikleri sistemde uygulayabiliriz.
