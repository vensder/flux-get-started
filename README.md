# flux-get-started

[![CircleCI](https://circleci.com/gh/fluxcd/flux-get-started.svg?style=svg)](https://circleci.com/gh/fluxcd/flux-get-started)

We published a step-by-step run-through on how to use Flux and Helm Operator [over
here](https://github.com/fluxcd/flux/blob/master/docs/tutorials/get-started-helm.md).

## Workloads

[podinfo](https://github.com/stefanprodan/podinfo)
* Kubernetes deployment, ClusterIP service and Horizontal Pod Autoscaler
* init container automated image updates (regular expression filter)
* container automated image updates (semantic versioning filter)

## Helm Releases

Mongodb
* Source: Helm repository (stable)
* Kubernetes deployment
* automated image updates (semantic versioning filter)

Redis
* Source: Helm repository (stable)
* Kubernetes stateful set 
* locked automated image updates (semantic versioning filter)

Ghost
* Source: Git repository
* disabled automated image updates (glob filter)
* has external dependency - mariadb (stable)

## Manifests Validation

CircleCI [jobs](./.circleci/config.yml):
* validate Kubernetes manifests with [kubeval](https://github.com/instrumenta/kubeval)
* validate Flux Helm Releases with [hrval](https://github.com/stefanprodan/hrval-action)

### <a name="help"></a>Getting Help

If you have any questions about, feedback for or problems with `flux-get-started`:


- Invite yourself to the <a href="https://slack.cncf.io" target="_blank">CNCF community</a>
  slack and ask a question on the [#flux](https://cloud-native.slack.com/messages/flux/)
  channel.
- To be part of the conversation about Flux's development, join the
  [flux-dev mailing list](https://lists.cncf.io/g/cncf-flux-dev).
- [File an issue.](https://github.com/fluxcd/flux/issues/new)

Your feedback is always welcome!

--------------------

```bash
systemctl start snapd  
sudo systemctl restart docker
microk8s start 
sudo pacman -S fluxctl

kubectl create ns flux 
export GHUSER=vensder
microk8s enable rbac 
fluxctl install \                                                                                                19:43:17 
--git-user=${GHUSER} \
--git-email=${GHUSER}@users.noreply.github.com \
--git-url=git@github.com:${GHUSER}/flux-get-started \
--git-path=namespaces,workloads \
--namespace=flux | kubectl apply -f -
serviceaccount/flux unchanged
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
clusterrole.rbac.authorization.k8s.io/flux unchanged
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
clusterrolebinding.rbac.authorization.k8s.io/flux unchanged
deployment.apps/flux unchanged
secret/flux-git-deploy unchanged
deployment.apps/memcached unchanged
service/memcached unchanged

kubectl -n flux rollout status deployment/flux                                                                   19:43:23 
deployment "flux" successfully rolled out
```

```bash
kubectl get pods -A                                                                                              20:18:19 
NAMESPACE     NAME                                      READY   STATUS    RESTARTS   AGE
ingress       nginx-ingress-microk8s-controller-cd587   1/1     Running   2          43d
kube-system   calico-kube-controllers-847c8c99d-bpdkr   1/1     Running   2          43d
kube-system   coredns-86f78bb79c-phnkm                  1/1     Running   2          43d
kube-system   calico-node-glwtr                         1/1     Running   2          43d
flux          memcached-c86cd995d-9x7tm                 1/1     Running   0          36m
flux          flux-58c6dc4ff7-8qd6l                     1/1     Running   0          36m
demo          podinfo-6496f8785c-8nj7b                  1/1     Running   0          13m
demo          podinfo-6496f8785c-q8dxd                  1/1     Running   0          13m
```

```
kubectl get namespace                                                                                            20:19:38 
NAME              STATUS   AGE
kube-system       Active   43d
kube-public       Active   43d
kube-node-lease   Active   43d
default           Active   43d
ingress           Active   43d
flux              Active   38m
demo              Active   27m
```

```bash
kubectl get pods --namespace=demo                                                                                20:21:25 
NAME                       READY   STATUS    RESTARTS   AGE
podinfo-5895f977b6-d5r9j   1/1     Running   0          86s
podinfo-5895f977b6-gqkrj   1/1     Running   0          79s
```

```ubectl get services                                                                                             20:21:37 
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.152.183.1   <none>        443/TCP   43d
```

```bash
kubectl get services --namespace=demo                                                                            20:22:11 
NAME      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
podinfo   ClusterIP   10.152.183.146   <none>        9898/TCP   28m
```


```bash
curl 10.152.183.146:9898                                                                                         20:22:18 
{
  "hostname": "podinfo-5895f977b6-d5r9j",
  "version": "3.1.5",
  "revision": "948de81ed319b4cae86ef9758866165acd4426a2",
  "color": "#34577c",
  "logo": "https://raw.githubusercontent.com/stefanprodan/podinfo/gh-pages/cuddle_clap.gif",
  "message": "'Welcome to Flux hell'",
  "goos": "linux",
  "goarch": "amd64",
  "runtime": "go1.13.6",
  "num_goroutine": "8",
  "num_cpu": "4"
}%                                                            
```
