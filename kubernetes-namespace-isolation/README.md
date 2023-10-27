# Namespace App
## Uygulama Adımları
Öncelikle bir Node.js app oluşturuyoruz.

![image](https://github.com/LOG-IN-DEVOPS-BOOTCAMP/kubernetes-assignment-1-yusuf-dnz/assets/101550162/231adb46-1b13-4bda-ad8f-3f627a3ff271)

Node.js web uygulamamızı içeren iki farklı docker image oluşturmamız isteniyor.
```shell
docker build -t web-app:v1 .
docker build -t web-app:v2 .
````
![image](https://github.com/LOG-IN-DEVOPS-BOOTCAMP/kubernetes-assignment-1-yusuf-dnz/assets/101550162/3e770407-21c9-4f26-8400-3cf57c957a79)

Bu iki image farklı ortamlarda izole çalışsın isteniyor. Bunun için default namespace yerine uygulamamızı dağıtabileceğimiz iki ortam oluşturalım.
```shell
kubectl create namespace web-v1
kubectl create namespace web-v2
````
![image](https://github.com/LOG-IN-DEVOPS-BOOTCAMP/kubernetes-assignment-1-yusuf-dnz/assets/101550162/098b5806-debf-4795-ab2f-635839ba8fb1)

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
![image](https://github.com/LOG-IN-DEVOPS-BOOTCAMP/kubernetes-assignment-1-yusuf-dnz/assets/101550162/23a4416d-38f5-465f-9e6e-20ba0dd29ec2)

Kendi namespace leri içerisinde dağıttımız pod lara başka ortamdan erişim yapamıyoruz. Bu yüzden içerisindeki pod lara namespace üzerinden erişim sağlayıp uygulamamıza erişim sağlayabiliriz.
- -n flag i kullanarak hedef namespace i belirtiriz
```shell
kubectl exec -n web-v1 web-app-deployment-v1-696b79c974-hc5lr -it bash
````
Bu şekilde pod içerisine erişip izole ortamımızda gerekli değişiklikleri sistemde uygulayabiliriz.
