# Formation openshift Gitops

This repo contains the material to deploy si infra.

## Prerequisites 

Argo is already installed using

https://github.com/neutron-IT-organization/formation-openshift-gitops/blob/main/prereq/argoCd.yaml


## Deploy ArgoCDs and create namespaces (in the gitops version we have now, we can't add labels when they are created by ArgoCD)

```shell
oc apply -f prereq/ns.yaml
oc apply -f prereq/sub.yaml
```

## OpenShift GitOps

```sh
oc patch argocd openshift-gitops -n openshift-gitops -p '{"spec":{"server":{"insecure":true,"route":{"enabled": true,"tls":{"termination":"edge","insecureEdgeTerminationPolicy":"Redirect"}}}}}' --type=merge
oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:openshift-gitops-operator:argocd-argocd-application-controller
```

## Configure the infrastructure for the workshop

Create the argoCD headquarter project

```shell
oc apply -f gitops/project.yaml
oc apply -f gitops/hashicorp/application.yaml
oc apply -f gitops/aap/application.yaml
```

## Cleanup

```shell
oc delete -f gitops/hashicorp/application.yaml
oc delete -f prereq/ns.yaml
oc delete -f prereq/sub.yaml
```

