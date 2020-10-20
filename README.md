# Traefik V2 with minikube

Example of working with Traefik middleware

![dashboard.png](dashboard.png)

## Links

* [Traefik enterprise](https://doc.traefik.io/traefik-enterprise/)
* Can be configured by [Consul](https://www.consul.io/)
* stable Traefik [Helm Chart](https://github.com/helm/charts/tree/master/stable/traefik) based on v1.7
* using Traefik [behind an ELB](https://guv.cloud/post/traefik-aws-nlb/)
* Setup Traefik 2.1 [Guide](https://ralph.blog.imixs.com/2020/02/01/kubernetes-setup-traefik-2-1/)
* Traefik 2.2 on [GCE](https://github.com/codeaprendiz/kubernetes-kitchen/tree/master/gcp/task-005-traefik-whoami)

## Installation & Run
Checkout
```
$ git clone https://github.com/turneps403/traefik-v2-minikube.git
$ cd traefik-v2-minikube
```
Start minikube
```
$ minikube start --memory=8192 --cpus=2 --vm=true
```
Apply all manifests and check what you get
```
$ helmfile sync
$ kubectl get all --all-namespaces
```
Check list of avaliable services and start `traefikns` with browser
```
$ minikube service list
$ minikube service mytraefik --namespace=traefikns
```
![services](static/services.png)
Add path `/whoami` to url of any of opened tabs, check answer and response headers. That's result of middleware working.
![response](response/services.png)
![headers](static/headers.png)


Checking Traefik dashboard:
0. enable nginx ingress (native for minikube)
1. add minikube ip to `/etc/hosts` with predefined domain
2. check dashboard on `http://traefik-ui.minikube/dashboard/`
```
$ minikube addons enable ingress
$ echo "$(minikube ip) traefik-ui.minikube" | sudo tee -a /etc/hosts
# see result on http://traefik-ui.minikube/dashboard/
```
![dashboard](static/dashboard.png)


## Shutdown
```
$ helmfile destroy
$ minikube stop
$ minikube delete
```
