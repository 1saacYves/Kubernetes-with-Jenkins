# Kubernetes-with-Jenkins

Pré-requisitos:
- K3s
- Docker

  ## Configurar o Ingress

1. Verifique se o K3s está rodando:

```
sudo systemctl status k3s
```

2. Configurar o _Ingress Controller_:

- Nesse caso vou usar o __Nginx Ingress Controller__.
- Use esse comando para instalar o controlador do __Ingress__ no __Cluster__:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/cloud/deploy.yaml
```

## Jenkins

1. Crie um arquivo **YAML**, para fazer a implementação do **Jenkins**

- Use o comando, para criar o arquivo:

```
touch jenkins-deploy.yaml
```

- e __ls__, para verificar se foi criado

2. Configurar o arquivo __jenkins-deploy.yaml__

- Para editar o arquivo, vou utilizar o editor __vim__, caso não o tenha instalado, é só usar o seguinte comando __yum install vim__

```
sudo vim jenkins-deploy.yaml
```

- Coloque as seguintes informações nesse arquivo:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
      - name: jenkins
        image: jenkins/jenkins:lts
        ports:
        - containerPort: 8080
        - containerPort: 50000
```

- Salve o arquivo __Esc :wq__

3. Depois de salvar o arquivo, aplique-o ao cluster __K3s__:

- Use o comando:

```
kubectl apply -f seu-arquivo-de-implantacao.yaml
```

## Service para o Jenkins

- Crie um arquivo __YAML__ para o serviço que irá expor o __Jenkins__:

```
touch service.yaml
```

- Em seguida, edite esse arquivo com as seguintes informações:

```
sudo vim service.yaml
```

```
Em seguida, edite esse arquivo com as seguintes informações:

```

## Depois de salvar o arquivo, aplique-o ao cluster K3s:

```
kubectl apply -f seu-arquivo-service.yaml
```

## Configurar o Ingress para Jenkins

1. Crie um arquivo para configurar o __ingress__

```
touch jenkins-ingress.yaml
```

- Dentro desse arquivo, coloque as seguintes informações:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: jenkins-ingress
spec:
  rules:
  - host: jenkins.example.com  # Substitua pelo seu domínio ou IP desejado
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: jenkins
            port:
              number: 80
```

- Depois de salvar o arquivo, aplique-o:

```
kubectl apply -f jenkins-ingress.yaml
```

## Para verificar se a implantação do Jenkins e do Ingress foi bem-sucedida em seu cluster K3s, você pode realizar algumas etapas de verificação:


- Verifique o Deployment:

```
kubectl get deployment
```

- Verifique o Service:

```
kubectl get services
```

- Verifique o Ingress:

```
kubectl get ingress
```
