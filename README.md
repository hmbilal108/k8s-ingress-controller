# k8s-ingress-controller mention by AB

```
minikube addons enable ingress
```

```
vi ingress-nginx-namespace.yaml
```

```
apiVersion: v1
kind: Namespace
metadata:
  name: ingress-nginx
```

```
kubectl apply -f ingress-nginx-namespace.yaml
```

```
vi custom-apache-deployment.yaml
```

```
#custom-apache-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: custom-apache-deployment
  namespace: ingress-nginx
  labels:
    app: custom-apache
spec:
  replicas: 1
  selector:
    matchLabels:
      app: custom-apache
  template:
    metadata:
      labels:
        app: custom-apache
    spec:
      containers:
      - name: custom-apache
        image: httpd:2.4
        ports:
        - containerPort: 80 
```


```
kubectl apply -f custom-apache-deployment.yaml
```

```
vi apache-deployment.yaml
```


```
#apache-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: apache-deployment
  namespace: ingress-nginx
  labels:
    app: apache
spec:
  replicas: 1
  selector:
    matchLabels:
      app: apache
  template:
    metadata:
      labels:
        app: apache
    spec:
      containers:
      - name: apache
        image: theabdullahchaudhary/my-apache-server-with-love-message:latest
        ports:
        - containerPort: 80

```

```
kubectl apply -f apache-deployment.yaml
```

```
vi custom-apache-service.yaml
```

```
#custom-apache-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: custom-apache-service
  namespace: ingress-nginx

  labels:
    app: custom-apache
spec:
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    app: custom-apache
```

```
kubectl apply -f custom-apache-service.yaml
```


```
vi apache-service.yaml
```

```
#apache-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: apache-service
  namespace: ingress-nginx
  labels:
    app: apache
spec:
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    app: apache
  type: ClusterIP
```

```
kubectl apply -f apache-service.yaml
```

```
vi apache-ingress.yaml
```

```
# apache-ingress.yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: apache-ingress
  namespace: ingress-nginx
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: aws.digitxel.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: apache-service
            port:
              number: 80
  - host: aws1.digitxel.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: custom-apache-service
            port:
              number: 80
```

```
kubectl apply -f apache-ingress.yaml
```

```
kubectl get all -n ingress-nginx
```


```
```



























































































































































































