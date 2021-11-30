
##### Monitoring using prometheus and grafana with kubernetes on AWS EKS cluster.


![alt text](https://miro.medium.com/max/1400/0*xmJhkI0IbRmbnlcG.png)


## What is prometheus?

Prometheus is a free software application used for event monitoring and alerting. Prometheus collects and stores metrics. Prometheus is not intended as a dashboarding solution. Although it can be used to graph specific queries, it is not a full-fledged dashboarding solution and needs to be hooked up with Grafana to generate dashboards.

# We want Prometheus to collect metrics related to the Kubernetes services, nodes, and orchestration status. Prometheus Components:

- Kube-state-metrics --> for orchestration and cluster level metrics: deployments, pod metrics, resource reservation, etc.
- Node exporter --> for the classical host-related metrics: cpu, mem, network, etc.
- Pushgateway --> allows you to push metrics from jobs which cannot be scraped. (Pushgateway exists to allow ephemeral and batch jobs to expose their metrics to Prometheus.)
- AlertManager --> when it's detecting performance issues notifies end-user through E-mail, Slack, or other tools. (Alerting helps notify as soon as the problem occurs and allow teams to identify problem through notifications.) P.S. I didn't set it up because it is not required in the current ticket.

To set up prometheus we will us YAML files and Makefile.


![alt text](https://www.whitelabeldevelopers.tech/upload/glossary/384/headimg.webp)


## What is Grafana?

Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources.

To set up grafana we will us YAML files and Makefile.

## What is Ingress-nginx?

Ingress-nginx is an Ingress controller for Kubernetes using NGINX as a reverse proxy and load balancer.

To set up ingress-nginx we will us YAML files.

## What is an ingress rule?

As a short definition, an Ingress is a rule that charts how a service, walled inside the cluster, can bridge the divide to the outside world where clients can use it.



## Issues that I faced:

1. Pod of the ingress-controller in a Pending state, pod that should create a LoadBalancer in AWS.

Solution: By mistake, ClusterRole in EKS was deleted. The cluster did not have permission to create LoadBalancer. As we know EKS makes calls to other AWS services on your behalf to manage the resources that you use with the service. I have recreated the missing cluster role on AWS with the same name as the role that was deleted before. Once it was done the role was automatically attached to the Cluster.

2. PVC in Grafana was in pending status. In events shows "Can't mount EBS volume: 'Permission denied".

Solution: The cluster wasn't able to mount EBS volume from AWS, because of the earlier deleted ClusterRole.



![alt text](https://www.google.com/url?sa=i&url=https%3A%2F%2Fmedium.com%2Favmconsulting-blog%2Fhow-to-monitor-kubernetes-cluster-with-prometheus-and-grafana-8ec7e060896f&psig=AOvVaw0P1nzghEaS8EGPf8cHhMYi&ust=1638328658824000&source=images&cd=vfe&ved=0CAsQjRxqFwoTCLibr72Qv_QCFQAAAAAdAAAAABAJ)

