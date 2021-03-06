## Hands-On Time

---

# Using Services To Enable Communication Between Pods


## Exposing Ports

---

```bash
cat svc/go-demo-2-rs.yml

kubectl create -f svc/go-demo-2-rs.yml

kubectl get -f svc/go-demo-2-rs.yml

kubectl expose rs go-demo-2 --name=go-demo-2-svc --target-port=28017 \
    --type=NodePort
```


<!-- .slide: data-background="img/seq_svc_ch05.png" data-background-size="contain" -->


<!-- .slide: data-background="img/comp_svc_ch05.png" data-background-size="contain" -->


## Exposing Ports

---

```bash
kubectl describe svc go-demo-2-svc

PORT=$(kubectl get svc go-demo-2-svc \
    -o jsonpath="{.spec.ports[0].nodePort}")

IP=$(minikube ip)

open "http://$IP:$PORT"
```


<!-- .slide: data-background="img/svc-expose-rs.png" data-background-size="contain" -->


## Exposing Ports

---

```bash
kubectl delete svc go-demo-2-svc
```


## Declarative Syntax

---

```bash
cat svc/go-demo-2-svc.yml

kubectl create -f svc/go-demo-2-svc.yml

kubectl get -f svc/go-demo-2-svc.yml

open "http://$IP:30001"
```


<!-- .slide: data-background="img/svc-hard-coded-port.png" data-background-size="contain" -->


## Declarative Syntax

---

```bash
kubectl delete -f svc/go-demo-2-svc.yml

kubectl delete -f svc/go-demo-2-rs.yml
```


## Communication Through Services

---

```bash
cat svc/go-demo-2-db-rs.yml

kubectl create -f svc/go-demo-2-db-rs.yml

cat svc/go-demo-2-db-svc.yml

kubectl create -f svc/go-demo-2-db-svc.yml

cat svc/go-demo-2-api-rs.yml

kubectl create -f svc/go-demo-2-api-rs.yml

cat svc/go-demo-2-api-svc.yml

kubectl create -f svc/go-demo-2-api-svc.yml
```


## Communication Through Services

---

```bash
kubectl get all

PORT=$(kubectl get svc go-demo-2-api \
    -o jsonpath="{.spec.ports[0].nodePort}")

curl -i "http://$IP:$PORT/demo/hello"

kubectl delete -f svc/go-demo-2-db-rs.yml

kubectl delete -f svc/go-demo-2-db-svc.yml

kubectl delete -f svc/go-demo-2-api-rs.yml

kubectl delete -f svc/go-demo-2-api-svc.yml
```


## Defining Multiple Objects In The Same YAML file

---

```bash
cat svc/go-demo-2.yml

kubectl create -f svc/go-demo-2.yml

kubectl get -f svc/go-demo-2.yml

PORT=$(kubectl get svc go-demo-2-api \
    -o jsonpath="{.spec.ports[0].nodePort}")

curl -i "http://$IP:$PORT/demo/hello"

POD_NAME=$(kubectl get pod --no-headers \
    -o=custom-columns=NAME:.metadata.name \
    -l type=api,service=go-demo-2 | tail -1)

kubectl exec $POD_NAME env

kubectl describe svc go-demo-2-db

cat ./svc/go-demo-2-api-rs.yml
```


<!-- .slide: data-background="img/flow_svc_ch05.png" data-background-size="contain" -->


## Services?

---

* Communication between Pods<!-- .element: class="fragment" -->
* Communication from outside the cluster<!-- .element: class="fragment" -->


<!-- .slide: data-background="img/svc-components.png" data-background-size="contain" -->


## What Now?

---

```bash
kubectl delete -f svc/go-demo-2.yml
```
