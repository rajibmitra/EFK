Create logging namespace.

kubectl create -f ./k8s/efk/namespace.yml
Deploy elasticsearch cluster (3 nodes).

kubectl create -f ./k8s/efk/elasticsearch/
Deploy Kibana.

kubectl create -f ./k8s/efk/kibana/
*** rollout may take some time.

Deploy Fluentd DaemonSet.

# add custom configuration
kubectl create configmap fluentd-conf --from-file=./k8s/efk/fluentd/kubernetes.conf --namespace=efk-logging

# Add necessary permissions
kubectl create -f ./k8s/efk/fluentd/fluentd.rbac.yml

# Start DaemonSet
kubectl create -f ./k8s/efk/fluentd/fluentd.daemon.set.yml
Port forward Kibana dashboard.

kubectl port-forward -n efk-logging svc/kibana 8081:5601
*** EFK setup may take some time, so please be patient.

Open your web browser and go to Kibana dashboard page.

Click Discover tab and then Create index pattern.

Use logstash* pattern to see the logs in kibana