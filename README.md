# easyTravel-OpenShift

![easyTravel Logo](https://github.com/holylai830830/easyTravel-OpenShift/blob/master/screen.png)

This project deploys the [Dynatrace easyTravel](https://community.dynatrace.com/community/display/DL/Demo+Applications+-+easyTravel) demo application on [RedHat OpenShift](https://www.openshift.com).

## Application Components

| Service             | Description
|:--------------------|:-----------
| easytravel-mongodb  | A pre-populated travel database (MongoDB).
| easytravel-backend  | The easyTravel Business Backend (Java).
| easytravel-frontend | The easyTravel Customer Frontend (Java).
| easytravel-www      | A reverse-proxy for the easyTravel Customer Frontend (NGINX).

## Prerequisites

1. Access to an [OpenShift](https://www.openshift.com) environment is required. If you don't have a cluster up and running and are unsure on how to set one up, you can follow the tutorial in [README-deployOpenShiftWithAnsibleOnAWS.md](https://github.com/dynatrace-innovationlab/easyTravel-OpenShift/blob/master/README-deployOpenShiftOnAwsWithAnsible.md).
2. The [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/get_started_cli.html) has to be installed.
3. Configuration for deploying easyTravel on OpenShift is stored in [config/os-settings.sh](https://github.com/dynatrace-innovationlab/easyTravel-OpenShift/blob/master/config/os-settings.sh). Adapt to suit your needs.

## Running easyTravel on OpenShift

### 0. Bootstrap

Login as *system:admin* and grant rights to user *admin* via:

```
oc login -u $OCP_ADMIN_USER  https://$API_URL:6443
oc new-project easytravel
oc adm policy add-role-to-user cluster-admin admin -n easytravel
oc adm policy add-scc-to-user anyuid -z default -n easytravel
```

###label the node
kubectl label node <node_name> <label> --overwrite

### 1. Create project 

##deploy the project with node-selector to control workload to desired nodes to minimize dynatrace license consumption 

oc adm new-project --node-selector=<label>  easytravel

### 2. Deploy all resources 
oc project easytravel
oc apply -f easytravel.yml

### 3. Expose Services

```
oc get svc  -n easytravel
oc expose service easytravel-www
```


## Monitoring easyTravel with Dynatrace

Please refer to the [tutorials section](https://github.com/dynatrace-innovationlab/easyTravel-OpenShift/tree/master/tutorials/) on how to set up Dynatrace for OpenShift using Ansible.

## License

Licensed under the MIT License. See the [LICENSE](https://github.com/dynatrace-innovationlab/easyTravel-OpenShift/blob/master/LICENSE) file for details.
