
gangireddydanam/gameofpods

alias k='kubectl'
alias kgp='kubectl get po'
alias kgs='kubectl get svc'
alias kgd='kubectl get deployments'

k create ns vote

vote-deployment
Create a deployment: name = 'vote-deployment'
image = 'kodekloud/examplevotingapp_vote:before'
status: 'Running'

k create deployment vote-deployment --image=kodekloud/examplevotingapp_vote:before -n vote

apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: vote-deployment
  name: vote-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: vote-deployment
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: vote-deployment
    spec:
      containers:
      - image: kodekloud/examplevotingapp_vote:before
        name: examplevotingapp
        resources: {}
status: {}
vote-svc


reate a new service: name = vote-service
port = '5000'
targetPort = '80'
nodePort= '31000'
service endpoint exposes deployment 'vote-deployment'

k expose deployment vote-deployment --port=80 --name=vote-service --dry-run -o yaml >vote_svc.yaml





apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: vote-deployment
  name: vote-service
  namespace: vote
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
    nodePort: 31000
  selector:
    app: vote-deployment
  type: NodePort







Create new deployment, name: 'redis-deployment'
image: 'redis:alpine'
Volume Type: 'EmptyDir'
Volume Name: 'redis-data'
mountPath: '/data'
status: 'Running'

k create deployment redis-deployment --image=redis:alpine --dry-run -o yaml >redis.yaml 


apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: redis-deployment
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-deployment
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: redis-deployment
    spec:
      containers:
      - image: redis:alpine
        name: redis
        volumeMounts:
        - mountPath: /data
          name: redis-data
      volumes:
      - name: redis-data
        emptyDir: {}



New Service, name = 'redis'
port: '6379'
targetPort: '6379'
type: 'ClusterIP'
service endpoint exposes deployment 'redis-deployment'

k expose deployment redis-deployment --port=6379 --name=redis -n vote


Create new deployment. name: 'worker'
image: 'kodekloud/examplevotingapp_worker'
status: 'Running'


k create deployment worker --image=kodekloud/examplevotingapp_worker --dry-run -o yaml >worker.yaml



Create new deployment. name: 'db-deployment'
image: 'postgres:9.4'
Volume Type: 'EmptyDir'
Volume Name: 'db-data'
mountPath: '/var/lib/postgresql/data'
status: 'Running'
