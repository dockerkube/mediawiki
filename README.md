# mediawiki
 
MEDIAWIKI DEPLOYMENT ON K8S CLUSTER
[Document subtitle]
 
 
# git clone -b master https://github.com/dockerkube/mediawiki.git
Cloning into 'mediawiki'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 15 (delta 1), reused 12 (delta 1), pack-reused 0
Unpacking objects: 100% (15/15), done.
[opc@brm-dev-06 test]$ ls
mediawiki
[opc@brm-dev-06 test]$ cd mediawiki/
[opc@brm-dev-06 mediawiki]$ ls
mediawiki-chart
[opc@brm-dev-06 mediawiki]$ cd mediawiki-chart/
[opc@brm-dev-06 mediawiki-chart]$ ls
Chart.yaml  README.md  templates
[opc@brm-dev-06 mediawiki-chart]$ cd templates/
[opc@brm-dev-06 templates]$ ls
db-deployment.yaml  db-service.yaml  env-configmap.yaml  mediawiki-deployment.yaml  mediawiki-service.yaml  secret.yaml

# helm install mediawiki -n k8s ./mediawiki-chart
[opc@brm-dev-06 mediawiki]$ helm install mediawiki -n k8s ./mediawiki-chart
NAME: mediawiki
LAST DEPLOYED: Tue Jul 27 02:19:42 2021
NAMESPACE: k8s
STATUS: deployed
REVISION: 1
TEST SUITE: None


# kubectl get pods -n k8s
NAME                         READY   STATUS    RESTARTS   AGE
db-b5ff95b5-9lxgt            1/1     Running   0          10s
mediawiki-7576b79f8d-x2wq2   1/1     Running   0          10s

# kubectl describe svc mediawiki -n k8s
Name:                     mediawiki
Namespace:                k8s
Labels:                   app.kubernetes.io/managed-by=Helm
                          service=mediawiki
Annotations:              meta.helm.sh/release-name: mediawiki
                          meta.helm.sh/release-namespace: k8s
                          service.beta.kubernetes.io/oci-load-balancer-internal: true
                          service.beta.kubernetes.io/oci-load-balancer-subnet1:
                            ocid1.subnet.oc1.uk-london-1.aaaaaaaasfbocitfk42cad7iybyxejmtkjlavdu4zowbimdaqajvtjdu7utq
Selector:                 service=mediawiki
Type:                     LoadBalancer
IP:                       10.96.167.185
LoadBalancer Ingress:     20.0.2.47
Port:                     http  80/TCP
TargetPort:               80/TCP
NodePort:                 http  30779/TCP
Endpoints:                10.244.0.187:80
Port:                     https  443/TCP
TargetPort:               80/TCP
NodePort:                 https  31954/TCP
Endpoints:                10.244.0.187:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type    Reason                Age   From                Message
  ----    ------                ----  ----                -------
  Normal  EnsuringLoadBalancer  38s   service-controller  Ensuring load balancer
  Normal  EnsuredLoadBalancer   5s    service-controller  Ensured load balancer


# kubectl get svc -n k8s
NAME        TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
db          ClusterIP      10.96.64.136    <none>        33060/TCP                    3m45s
mediawiki   LoadBalancer   10.96.111.252   20.0.2.38     80:31955/TCP,443:31239/TCP   3m45s
[opc@brm-dev-06 mediawiki-chart]$

# helm ls -A
NAME                    NAMESPACE               REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
mediawiki               k8s                     1               2021-07-27 02:39:50.188668868 -0700 PDT deployed        mediawiki-chart-0.0.1                
7.	[opc@brm-dev-06 mediawiki-chart]$

# Acess the service using External IP and port which is get assigned to service after creating the service 
http://20.0.2.38/
 

# Delete Helm chart:
1.	[opc@brm-dev-06 mediawiki-chart]$ helm list -n k8s
 
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
 
mediawiki       k8s             1               2021-07-27 02:39:50.188668868 -0700 PDT deployed        mediawiki-chart-0.0.1
 
[opc@brm-dev-06 mediawiki-chart]$ ^C
 
2.	[opc@brm-dev-06 mediawiki-chart]$ helm delete mediawiki -n k8s
 
release "mediawiki" uninstalled
 
[opc@brm-dev-06 mediawiki-chart]$
 
[opc@brm-dev-06 mediawiki-chart]$ helm list -n k8s
 
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION
 
[opc@brm-dev-06 mediawiki-chart]$

3.	[opc@brm-dev-06 mediawiki-chart]$ kubectl get pods -n k8s
 
NAME                         READY   STATUS        RESTARTS   AGE
 
mediawiki-7576b79f8d-9b5l6   1/1     Terminating   0          12m
 
[opc@brm-dev-06 mediawiki-chart]$
4.	You can see mediawiki hosted in apache directory by doing exec in pod

[opc@brm-dev-06 mediawiki]$ kubectl exec -it mediawiki-85ff968bf5-p4mdl -n k8s /bin/bash
 
[root@mediawiki-85ff968bf5-p4mdl html]#
 
[root@mediawiki-85ff968bf5-p4mdl html]# pwd
 
/opt/rh/httpd24/root/var/www/html
 
[root@mediawiki-85ff968bf5-p4mdl html]# ls
 
mediawiki
[root@mediawiki-85ff968bf5-p4mdl html]#
 
[root@mediawiki-85ff968bf5-p4mdl html]#

5.	[opc@brm-dev-06 mediawiki-chart]$ kubectl get pods -n k8s
 
  No resources found.

 


