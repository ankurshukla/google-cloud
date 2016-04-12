# Datadog on Google Container Engine using Deployment Manager

#### Step 1. Deploying a cluster and cluster-type

The template files that we will use for deploying the cluster in GCP are:

 - cluster.yaml
 - cluster.jinja
 - cluster.jinja.schema

In cluister,yaml, you need to provide the name of the cluster, the GCP zone where the cluster nodes are deployed and the username/password that you wish to provide the cluster. You can add/edit the scope of the instances as well as change the default machine-types in cluster,jinja files.

To create and deploy the cluster and cluster-type, run the following command

```sh
$ gcloud deployment-manager deployments create cluster --config cluster.yaml
```

This will create a Google Container Engine (GKE) cluster and cluster-type deployment. The GKE cluster will be managed by GCP with usernmae/password as the auth.


### Step 2. Deploying the Datadog Agent to the deployed cluster in GKE

The template files that we will use for deploying the Datadog agent (as Daemonsets) to the GKE cluster are:

 - datadog.yaml
 - datadog,jinja.schema
 - datadog.jinja

The cluster created (via DM templates as step1. or using gcloud directly) needs to be provided as input to the yaml file properties. Along with this, the GKE/kubernetes version (1.2 or earlier) can be configured here. The datadog specific properties like the access-key, docker image and cutom port where the agent runs, also is configured as a part of the config file.

To deploy the Datadog agent, run the following command
```sh
$ gcloud deployment-manager deployments create dd-agent --config datadog.yaml
```

Note that dd-agent is the name we have given to the deployment here.
The biggest advantage of using Deployment Manager templates is that it can be used together to deploy complex resources under a single solution. From maintaining a solution's perspective, you would need to only make changes to the yaml file and run the deployments.
