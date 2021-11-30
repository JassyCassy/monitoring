# Get the current cluster name
cluster = Ubuntu-team-EKS-DEV
#cluster = $(shell basename $(kubectl config current-context))

# configutation
version = 1.21
namespace = monitoring
include cluster/$(cluster)/config
region = us-east-1

# do not change
kubectl := kubectl -n $(namespace)

#env-vars for replacements
replacements="\
s/NAMESPACE/$(namespace)/g;\
s/STORAGESIZE/$(storagesize)/g;\
s/CLUSTER/$(cluster)/g;\
s/RETENTIONPERIOD/$(retentionPeriod)/g\
"

namespace:
	@cat namespace.yaml | sed $(replacements) | kubectl apply -f -

run-prometheus: namespace
	@cat server/cluster-role.yaml | sed $(replacements) | kubectl apply -f -
	@cat server/pvc.yaml | sed $(replacements) | kubectl apply -f -
	@cat server/config-map.yaml | sed $(replacements) | kubectl apply -f -
	@cat server/deployment.yaml | sed $(replacements) | kubectl apply -f -
	@cat server/service.yaml | sed $(replacements) | kubectl apply -f -
	@###
	@cat kube-state-metrics/cluster-role.yaml | sed $(replacements) | kubectl apply -f -
	@cat kube-state-metrics/deployment.yaml | sed $(replacements) | kubectl apply -f -
	@cat kube-state-metrics/service.yaml | sed $(replacements) | kubectl apply -f -
	@###
	@cat node-exporter/daemonset.yaml | sed $(replacements) | kubectl apply -f -
	@cat node-exporter/service.yaml | sed $(replacements) | kubectl apply -f -
	@###
	@cat pushgateway/cluster-role.yaml | sed $(replacements) | kubectl apply -f -
	@cat pushgateway/deployment.yaml | sed $(replacements) | kubectl apply -f -
	@cat pushgateway/service.yaml | sed $(replacements) | kubectl apply -f -

delete-prometheus: namespace
	@cat server/cluster-role.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat server/config-map.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat server/deployment.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat server/service.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@###
	@cat kube-state-metrics/cluster-role.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat kube-state-metrics/deployment.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat kube-state-metrics/service.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@###
	@cat node-exporter/daemonset.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat node-exporter/service.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@###
	@cat pushgateway/cluster-role.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat pushgateway/deployment.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
	@cat pushgateway/service.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found

destroy-pvc:
	@cat server/pvc.yaml | sed $(replacements) | kubectl delete -f - --ignore-not-found
