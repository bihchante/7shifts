Steps:

1. covert folder into working directory at the backend by using;
   git init
2. Move files into staging area by using:
   git add .
3. linked my local repository to remote github repository:
   git remote add origin https://github.com/bihchante/7-shifts-DevOps-Exercise.git
4. push code into remote github repository:
   git push -u origin main
5. clone into the k8s cluster:
   git clone https://github.com/bihchante/7-shifts-DevOps-Exercise.git



1. created 4 yml files using atom editor:

- vim redis-deployment.yml
---
apiVersion: apps/v1
kind: Deployment
metadata:
 name: redis-deployment
 labels:
  author: chante
  type: redis
spec:
 selector:
  matchLabels:
   type: redis
 template:
  metadata:
   name: redis-pod
   labels:
    type: redis
  spec:
   containers:
    - name: redis
      image: redis:5.0.4
      resources:
       limits:
        cpu: 200m
       requests:
        cpu: 100m

- kubectl apply -f redis-deployment.yml


vim postgres-deployment.yml
---
apiVersion: apps/v1
kind: Deployment
metadata:
 name: postgres
 labels:
  author: chante
  type: postgres
  namespace: test
spec:
 selector:
  matchLabels:
   type: postgres
 template:
  metadata:
    name: postgres
    labels:
     type: postgres
  spec:
   containers:
    - name: postgres
      image: postgres
      resources:
       limits:
        cpu: 200m
       requests:
        cpu: 100m

- kubectl create -f  postgres-deployment.yml

vim redis-autoscaling.yml
---
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: redis-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: redis
  minReplicas: 2
  maxReplicas: 10
  metrics:
   - type: Resource
     resource:
      name: cpu
      target:
       type: Utilization
       averageUtilization: 70

- kubectl create -f redis-deployment.yml

vim postgres-autoscaling.yml
---
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: redis-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: postgres
  minReplicas: 2
  maxReplicas: 10
  metrics:
   - type: Resource
     resource:
      name: cpu
      target:
       type: Utilization
       averageUtilization: 70

- kubectl create postgres-autoscaling.yml



