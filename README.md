# Kubernetes-ecommerce-viagens
 Arquivos YAML para configuração K8s e Istio

![alt text](https://github.com/douglasnogueiram/Kubernetes-ecommerce-viagens/blob/master/Captura%20de%20Tela%202020-08-02%20a%CC%80s%2012.38.09.png)

Os microsserviços de exemplo estão nos repositórios:
- https://github.com/douglasnogueiram/ms-communication-bank
- https://github.com/douglasnogueiram/ms-communication-buyfeedback
- https://github.com/douglasnogueiram/ms-communication-buyprocess
- https://github.com/douglasnogueiram/ms-communication-buytrip

Para fazer os testes, seguir os passos:

1 - Gerar o build dos componentes, na pasta ms-communication-master

mvn clean package dockerfile:push


2 - Confirmar se imagens subiram para Docker Hub


3 - Subir minikube e Istio

minikube start --memory=4096 --cpus=3
minikube dashboard

export PATH=$pwd/bin:$PATH

istioctl install --set profile=demo
kubectl label namespace default istio-injection=enabled
istioctl dashboard kiali
Kiaki: user "admin"    senha "admin"


4 - Subir serviços básicos

kubectl create -f mysql.yml

kubectl create -f rabbitmq.yml
minikube service rs-rmq-mgt

kubectl create -f redis.yml


5 - Acompanhar os logs de execução via Dashboard Kubernetes ou via terminal

kubectl logs -t <nome do pod>

6 - Criar as filas do RabbitMQ via client

fila-compras-aguardando
fila-compras-finalizado


7 - Subir MS Banco

kubectl create -f bank.yml
kubectl create -f bank-v2.yml
minikube service service-bank --url


8 - Subir MS buyfeedback

kubectl create -f buyfeedback.yml
minikube service service-buyfeedback --url


9 - Subir MS buyprocess

kubectl create -f buyprocess.yml
minikube service service-buyprocess --url


10 - Subir MS buytrip

kubectl create -f buytrip.yml
kubectl create -f buytrip-v2.yml
minikube service service-buytrip --url


11 - Subir o gateway e os virtualservices

kubectl create -f gateway.yml

kubectl create -f destinationrule-bank.yml
kubectl create -f destinationrule-buytrip.yml

kubectl create -f virtualservices-bank.yml
kubectl create -f virtualservices-buytrip.yml
