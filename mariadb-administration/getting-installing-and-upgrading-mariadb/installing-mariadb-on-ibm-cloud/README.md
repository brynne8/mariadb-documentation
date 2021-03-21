# Installing MariaDB on IBM Cloud

Get MariaDB on IBM Cloud

You should have an IBM Cloud account, otherwise you can [register here](https://cloud.ibm.com/registration).
At the end of the tutorial you will have a cluster with MariaDB up and running. IBM Cloud uses Bitnami charts  to deploy MariaDB on with helm

1. We will provision a new Kubernetes Cluster for you if, you already have one skip to step <strong>2</strong>

2. We will deploy  the IBM Cloud Block Storage plug-in, if already have it skip to step <strong>3</strong>

3. MariaDB deployment

## Step 1 provision Kubernetes Cluster

- Click the <strong>Catalog</strong> button on the top
- Select <strong>Service</strong> from the catalog
- Search for <strong>Kubernetes Service</strong> and click on it

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/kubernetes-select" alt="kubernetes-select" title="kubernetes-select">

- You are now at the Kubernetes deployment page, you need to specify some details about the cluster
- Choose a plan <strong>standard</strong> or <strong>free</strong>, the free plan only has one worker node and no subnet, to provision a standard cluster, you will need to upgrade you account to Pay-As-You-Go
- To upgrade to a Pay-As-You-Go account, complete the following steps:

- In the console, go to Manage &gt; Account.
- Select Account settings, and click Add credit card.
- Enter your payment information, click Next, and submit your information
- Choose <strong>classic</strong> or <strong>VPC</strong>, read the [docs](https://cloud.ibm.com/docs/containers?topic=containers-infrastructure_providers) and choose the most suitable type for yourself

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/infra-select" alt="infra-select" title="infra-select">

- Now choose your location settings, for more information please visit [Locations](https://cloud.ibm.com/docs/containers?topic=containers-regions-and-zones#zones)
- Choose <strong>Geography</strong> (continent)

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/location-geo" alt="location-geo" title="location-geo">

- Choose <strong>Single</strong> or <strong>Multizone</strong>, in single zone your data is only kept in on datacenter, on the other hand with Multizone it is distributed to multiple zones, thus  safer in an unforseen zone failure

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/location-avail" alt="location-avail" title="location-avail">

- Choose a <strong>Worker Zone</strong> if using Single zones or <strong>Metro</strong> if Multizone

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/location-worker" alt="location-worker" title="location-worker">

- If you wish to use Multizone please set up your account with [VRF](https://cloud.ibm.com/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) or [enable Vlan spanning](https://cloud.ibm.com/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning)
- If at your current location selection, there is no available Virtual LAN, a new Vlan will be created for you

- Choose a <strong>Worker node setup</strong> or use the preselected one, set <strong>Worker node amount per zone</strong>

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/worker-pool" alt="worker-pool" title="worker-pool">

- Choose <strong>Master Service Endpoint</strong>,  In VRF-enabled accounts, you can choose private-only to make your master accessible on the private network or via VPN tunnel. Choose public-only to make your master publicly accessible. When you have a VRF-enabled account, your cluster is set up by default to use both private and public endpoints. For more information visit [endpoints](https://cloud.ibm.com/docs/account?topic=account-service-endpoints-overview).

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/endpoints" alt="endpoints" title="endpoints">

- Give cluster a <strong>name</strong>

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/name-new" alt="name-new" title="name-new">

- Give desired <strong>tags</strong> to your cluster, for more information visit [tags](https://cloud.ibm.com/docs/account?topic=account-tag)

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/tasg-new" alt="tasg-new" title="tasg-new">

- Click <strong>create</strong>

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/create-new" alt="create-new" title="create-new">

- Wait for you cluster to be provisioned

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/cluster-prepare" alt="cluster-prepare" title="cluster-prepare">

- Your cluster is ready for usage

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/cluster-done" alt="cluster-done" title="cluster-done">

## Step 2 deploy IBM Cloud Block Storage plug-in

The Block Storage plug-in is a persistent, high-performance iSCSI storage that you can add to your apps by using Kubernetes Persistent Volumes (PVs).

- Click the <strong>Catalog</strong> button on the top
- Select <strong>Software</strong> from the catalog
- Search for <strong>IBM Cloud Block Storage plug-in</strong> and click on it

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/block-search" alt="block-search" title="block-search">

- On the application page Click in the dot next to the cluster, you wish to use
- Click on  <strong>Enter or Select Namespace</strong> and choose the default Namespace or use a custom one (if you get error please wait 30 minutes for the cluster to finalize)

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/block-cluster" alt="block-cluster" title="block-cluster">

- Give a <strong>name</strong> to this workspace
- Click <strong>install</strong> and wait for the deployment

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/block-storage-create" alt="block-storage-create" title="block-storage-create">

## Step 3 deploy MariaDB

We will deploy  MariaDB on our cluster

- Click the <strong>Catalog</strong> button on the top
- Select <strong>Software</strong> from the catalog
- Search for <strong>MariaDB</strong> and click on it

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/search" alt="search" title="search">

- Please select IBM Kubernetes Service

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/target-select" alt="target-select" title="target-select">

- On the application page Click in the dot next to the cluster, you wish to use

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/cluster-select" alt="cluster-select" title="cluster-select">

- Click on  <strong>Enter or Select Namespace</strong> and choose the default Namespace or use a custom one

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/details-namespace" alt="details-namespace" title="details-namespace">

- Give a unique <strong>name</strong> to workspace, which you can easily recognize

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/details-name" alt="details-name" title="details-name">

- Select which resource group you want to use, it's for access controll and billing purposes. For more information please visit [resource groups](https://cloud.ibm.com/docs/account?topic=account-account_setup#bp_resourcegroups)

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/details-resource" alt="details-resource" title="details-resource">

- Give <strong>tags</strong> to your MariaDB, for more information visit [tags](https://cloud.ibm.com/docs/account?topic=account-tag)

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/details-tags" alt="details-tags" title="details-tags">

- Click on <strong>Parameters with default values</strong>, You can set deployment values or use the default ones

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/parameters" alt="parameters" title="parameters">

- Please set the MariaDB root password in the parameters

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/root-password" alt="root-password" title="root-password">

- After finishing everything, <strong>tick</strong> the box next to the agreements and click <strong>install</strong>

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/aggreement-create" alt="aggreement-create" title="aggreement-create">

- The MariaDB workspace will start installing, wait a couple of minutes

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/in-progress" alt="in-progress" title="in-progress">

- Your  MariaDB workspace has been successfully deployed

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/done" alt="done" title="done">

## Verify MariaDB installation

- Go to [Resources](http://cloud.ibm.com/resources) in your browser
- Click on <strong>Clusters</strong>
- Click on your Cluster

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/resource-select" alt="resource-select" title="resource-select">

- Now you are at you clusters overview, here Click on <strong>Actions</strong> and <strong>Web terminal</strong> from the dropdown menu

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/cluster-main" alt="cluster-main" title="cluster-main">

- Click <strong>install</strong> - wait couple of minutes

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/terminal-install" alt="terminal-install" title="terminal-install">

- Click on <strong>Actions</strong>
- Click <strong>Web terminal</strong> --&gt; a terminal will open up

- <strong>Type</strong> in the terminal, please change NAMESPACE to the namespace you choose at the deployment setup:

```sql
$ kubectl get ns
```

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/get-ns" alt="get-ns" title="get-ns">

```sql
$ kubectl get pod -n NAMESPACE -o wide 
```

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/get-pods" alt="get-pods" title="get-pods">

```sql
$ kubectl get service -n NAMESPACE
```

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/get-service" alt="get-service" title="get-service">

- Enter your pod with bash , please replace PODNAME with your mariadb pod's name

```sql
$ kubectl exec --stdin --tty PODNAME -n NAMESPACE -- /bin/bash
```

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/bash" alt="bash" title="bash">

- After you are in your pod please enter enter Mariadb and enter your root password after the prompt

```sql
$ mysql -u root -p
```

<img src="/kb/en/installing-mariadb-on-ibm-cloud/+image/welcome" alt="welcome" title="welcome">

You have succesfully deployed MariaDB IBM Cloud!