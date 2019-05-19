**Kubernetes_PAG_Mysql**

**PROMETHEUS + ALERTMANAGER + GARFANA + KUBERNETES + HELM + MINIKUBE**

**Configure Prometheus with alertmanager on grafana UI for mysql instance STACK and provisioned on kubernetes cluster using minikube**

Used helm to install prometheus,alertmanager,grafana and MySQL server instance and also enable slow-log query. I have used customized grafana dashboard for MySQL instance which will show you MySQL POD status, Slow log query, handles, create/drop DB status.


Please follow below steps to provisioned Prometheus with alertmanager on grafana UI for mysql instance on kubernetes cluster using minikube.

```
$ git clone https://github.com/manukoli1986/Kubernetes_PAG_Mysql-.git
```

You will see below output once you create prometheus and grafana with customized values.yaml

```
$ helm install prometheus  --name prometheus --namespace monitoring -f prometheus/prometheus-values.yaml
NAME:   prometheus
LAST DEPLOYED: Mon May 20 02:20:27 2019
NAMESPACE: monitoring
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                     DATA  AGE
prometheus-alertmanager  1     2s
prometheus-server        3     2s

==> v1/PersistentVolumeClaim
NAME                     STATUS  VOLUME                                    CAPACITY  ACCESS MODES  STORAGECLASS  AGE
prometheus-alertmanager  Bound   pvc-bbd50b58-7a77-11e9-bf4c-080027e482c1  2Gi       RWO           standard      2s
prometheus-server        Bound   pvc-bbd7b3c2-7a77-11e9-bf4c-080027e482c1  8Gi       RWO           standard      2s

==> v1/Pod(related)
NAME                                           READY  STATUS             RESTARTS  AGE
prometheus-alertmanager-5b4544dfc9-w2vzz       0/2    ContainerCreating  0         2s
prometheus-kube-state-metrics-b7c85c6dd-ntr4n  0/1    ContainerCreating  0         2s
prometheus-node-exporter-6bdwm                 0/1    ContainerCreating  0         2s
prometheus-pushgateway-788949d7c4-d4z59        0/1    ContainerCreating  0         2s
prometheus-server-577b557577-7j8qb             0/2    Pending            0         2s

==> v1/Service
NAME                           TYPE       CLUSTER-IP     EXTERNAL-IP  PORT(S)       AGE
prometheus-alertmanager        ClusterIP  10.97.51.150   <none>       80/TCP        2s
prometheus-kube-state-metrics  ClusterIP  None           <none>       80/TCP        2s
prometheus-node-exporter       ClusterIP  None           <none>       9100/TCP      2s
prometheus-pushgateway         ClusterIP  10.109.77.250  <none>       9091/TCP      2s
prometheus-server              NodePort   10.101.129.28  <none>       80:30909/TCP  2s

==> v1/ServiceAccount
NAME                           SECRETS  AGE
prometheus-alertmanager        1        2s
prometheus-kube-state-metrics  1        2s
prometheus-node-exporter       1        2s
prometheus-pushgateway         1        2s
prometheus-server              1        2s

==> v1beta1/ClusterRole
NAME                           AGE
prometheus-kube-state-metrics  2s
prometheus-server              2s

==> v1beta1/ClusterRoleBinding
NAME                           AGE
prometheus-kube-state-metrics  2s
prometheus-server              2s

==> v1beta1/DaemonSet
NAME                      DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
prometheus-node-exporter  1        1        0      1           0          <none>         2s

==> v1beta1/Deployment
NAME                           READY  UP-TO-DATE  AVAILABLE  AGE
prometheus-alertmanager        0/1    1           0          2s
prometheus-kube-state-metrics  0/1    1           0          2s
prometheus-pushgateway         0/1    1           0          2s
prometheus-server              0/1    1           0          2s


NOTES:
The Prometheus server can be accessed via port 80 on the following DNS name from within your cluster:
prometheus-server.monitoring.svc.cluster.local


Get the Prometheus server URL by running these commands in the same shell:
  export NODE_PORT=$(kubectl get --namespace monitoring -o jsonpath="{.spec.ports[0].nodePort}" services prometheus-server)
  export NODE_IP=$(kubectl get nodes --namespace monitoring -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT


The Prometheus alertmanager can be accessed via port 80 on the following DNS name from within your cluster:
prometheus-alertmanager.monitoring.svc.cluster.local


Get the Alertmanager URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=alertmanager" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace monitoring port-forward $POD_NAME 9093


The Prometheus PushGateway can be accessed via port 9091 on the following DNS name from within your cluster:
prometheus-pushgateway.monitoring.svc.cluster.local


Get the PushGateway URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=pushgateway" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace monitoring port-forward $POD_NAME 9091

For more information on running Prometheus, visit:
https://prometheus.io/
```

For grafana
```
$ helm install grafana --name grafana --namespace monitoring -f grafana/grafana-values.yaml
NAME:   grafana
LAST DEPLOYED: Mon May 20 02:46:17 2019
NAMESPACE: monitoring
STATUS: DEPLOYED

RESOURCES:
==> v1/ClusterRole
NAME                 AGE
grafana-clusterrole  1s

==> v1/ClusterRoleBinding
NAME                        AGE
grafana-clusterrolebinding  1s

==> v1/ConfigMap
NAME                        DATA  AGE
grafana                     4     1s
grafana-dashboards-default  2     1s
grafana-test                1     1s

==> v1/Pod(related)
NAME                     READY  STATUS    RESTARTS  AGE
grafana-cf4f5dd77-ml585  0/1    Init:0/1  0         0s

==> v1/Role
NAME          AGE
grafana-test  1s

==> v1/RoleBinding
NAME          AGE
grafana-test  1s

==> v1/Secret
NAME     TYPE    DATA  AGE
grafana  Opaque  3     1s

==> v1/Service
NAME     TYPE      CLUSTER-IP      EXTERNAL-IP  PORT(S)       AGE
grafana  NodePort  10.105.218.192  <none>       80:31644/TCP  0s

==> v1/ServiceAccount
NAME          SECRETS  AGE
grafana       1        1s
grafana-test  1        1s

==> v1beta1/PodSecurityPolicy
NAME          PRIV   CAPS      SELINUX   RUNASUSER  FSGROUP   SUPGROUP  READONLYROOTFS  VOLUMES
grafana       false  RunAsAny  RunAsAny  RunAsAny   RunAsAny  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
grafana-test  false  RunAsAny  RunAsAny  RunAsAny   RunAsAny  false     configMap,downwardAPI,emptyDir,projected,secret

==> v1beta1/Role
NAME     AGE
grafana  1s

==> v1beta1/RoleBinding
NAME     AGE
grafana  1s

==> v1beta2/Deployment
NAME     READY  UP-TO-DATE  AVAILABLE  AGE
grafana  0/1    1           0          0s


NOTES:
1. Get your 'admin' user password by running:

   kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

2. The Grafana server can be accessed via port 80 on the following DNS name from within your cluster:

   grafana.monitoring.svc.cluster.local

   Get the Grafana URL to visit by running these commands in the same shell:
export NODE_PORT=$(kubectl get --namespace monitoring -o jsonpath="{.spec.ports[0].nodePort}" services grafana)
     export NODE_IP=$(kubectl get nodes --namespace monitoring -o jsonpath="{.items[0].status.addresses[0].address}")
     echo http://$NODE_IP:$NODE_PORT


3. Login with the password from step 1 and the username: admin
#################################################################################
######   WARNING: Persistence is disabled!!! You will lose your data when   #####
######            the Grafana pod is terminated.                            #####
#################################################################################

```

Now lets launch MySQL instance using helm 
```
$ helm install mysql --name mysql -f mysql/values.yaml
```

Once stack is UP and running we can check the pod status and exposed service port and access the grafana with default credentials (user: "admin" || pass: "admin123") 

```
$ kubectl.exe get pods -n monitoring
NAME                                            READY   STATUS    RESTARTS   AGE
grafana-cf4f5dd77-ml585                         1/1     Running   0          3m55s
prometheus-alertmanager-5b4544dfc9-hx2j5        1/2     Running   0          26s
prometheus-kube-state-metrics-b7c85c6dd-4pjb5   1/1     Running   0          26s
prometheus-node-exporter-2bjqk                  1/1     Running   0          26s
prometheus-pushgateway-788949d7c4-xpvf2         1/1     Running   0          26s
prometheus-server-577b557577-fcjrl              1/2     Running   0          26s

$ kubectl.exe get svc -n monitoring
NAME                            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
grafana                         NodePort    10.105.218.192   <none>        80:31644/TCP   4m32s
prometheus-alertmanager         ClusterIP   10.102.79.114    <none>        80/TCP         63s
prometheus-kube-state-metrics   ClusterIP   None             <none>        80/TCP         63s
prometheus-node-exporter        ClusterIP   None             <none>        9100/TCP       63s
prometheus-pushgateway          ClusterIP   10.99.226.56     <none>        9091/TCP       63s
prometheus-server               NodePort    10.111.136.237   <none>        80:30909/TCP   63s

```

Then grafana URL will be:
```
$ minikube.exe ip
192.168.99.101
```
URL: (http://192.168.99.101:31644)
user: admin
pass: admin123



Checking alert on bases of MySQL pods increases more than 4 then it will check the count of total pods running in default namespace.
```
kubectl scale --current-replicas=3 --replicas=1 deployment/mysql
```


Using pagerduty for alert
```
(https://kube-alert.pagerduty.com/incidents)
```

To list running resource 
```
$ helm ls
NAME            REVISION        UPDATED                         STATUS          CHART                   APP VERSION     NAMESPACE
grafana         1               Mon May 20 00:36:57 2019        DEPLOYED        grafana-3.3.8           6.1.6           monitoring
mysql           1               Mon May 20 01:50:11 2019        DEPLOYED        mysql-1.1.1             5.7.14          default
prometheus      1               Mon May 20 02:20:27 2019        DEPLOYED        prometheus-8.11.2       2.9.2           monitoring
```
To delete create resource using helm charts
```
$ helm del --purge grafana mysql prometheus
release "grafana" deleted
release "mysql" deleted
release "prometheus" deleted
```


**Task Done By Mayank Koli < Mayank.c.koli@gmail.com>**
