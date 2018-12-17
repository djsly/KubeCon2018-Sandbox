
create-cluster:
	kind create cluster --name=opa --config=kind.conf

destroy-cluster:
	kind delete cluster --name=opa

generate-ca:
	mkdir -p .ssl
	openssl genrsa -out .ssl/ca.key 2048
	openssl req -x509 -new -nodes -key .ssl/ca.key -days 100000 -out .ssl/ca.crt -subj "/CN=admission_ca"

generate-cert: generate-ca
	openssl genrsa -out .ssl/server.key 2048
	openssl req -new -key .ssl/server.key -out .ssl/server.csr -subj "/CN=opa.opa.svc" -config server.conf
	openssl x509 -req -in .ssl/server.csr -CA .ssl/ca.crt -CAkey .ssl/ca.key -CAcreateserial -out .ssl/server.crt -days 100000 -extensions v3_req -extfile server.conf

generate-tls-secret: generate-cert
	kubectl create namespace opa
	kubectl create -n opa secret tls opa-server --cert=.ssl/server.crt --key=.ssl/server.key

apply: generate-tls-secret
	kubectl apply -f admission-controller.yaml
	sed "s/<caBundle>/$$(cat .ssl/ca.crt | base64 | tr -d '\n')/g" webhook-configuration.yaml.template > webhook-configuration.yaml
	kubectl apply -f webhook-configuration.yaml
	kubectl create configmap example-policy --from-file example.rego -n opa 

log:
	kubectl logs -l app=opa -c opa -n opa

test:
	kubectl apply -f pod.yaml

clean:
	rm -rf .ssl webhook-configuration.yaml
	kubectl delete namespace opa
	#kubectl delete ValidatingWebhookConfiguration opa-validating-webhook
