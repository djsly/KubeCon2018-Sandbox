Open Policy Agent
=================

kube-mgmt manages instances of the Open Policy Agent on top of Kubernetes. Use kube-mgmt to:

* Load policies into OPA via Kubernetes (see Policies below.)
* Replicate Kubernetes resources into OPA (see Caching below.)

When a policy has been successfully loaded into OPA, the openpolicyagent.org/policy-status annotation is set to {"status": "ok"} on the configmap object.

References
----------
https://github.com/open-policy-agent/opa
https://www.openpolicyagent.org/docs/
https://www.openpolicyagent.org/docs/deployments.html#kubernetes
https://github.com/open-policy-agent/kube-mgmt
https://www.openpolicyagent.org/docs/kubernetes-admission-control.html
https://www.openpolicyagent.org/docs/ssh-and-sudo-authorization.html
https://www.openpolicyagent.org/docs/terraform.html

https://github.com/morvencao/kube-mutating-webhook-tutorial/blob/master/medium-article.md




