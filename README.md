# Tanzu Kubernetes Grid Integrated (TKGI) / PKS  Cluster Name Override Addon

## What does this do?

This will ensure that Kube Controller Manager in TKGI/PKS clusters gets a cluster-name equal to the GUID of the cluster itself.  

Note;  This is fixed as of PKS 1.7 and no longer necessary

## How do I install it?

1. Open a shell prompt on a BOSH CLI with access to your PKS bosh director, such as Ops Manager.
2. Export your BOSH credentials to the enviornment.  These can be accessed via the Ops Manager GUI -> BOSH Director Tile -> Credentials Tab -> Bosh Commandline Credentials.    

e.g.
```
export BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=fakesecret BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate  BOSH_ENVIRONMENT=10.0.0.10
```
3. Copy or clone this repository onto this BOSH CLI workstation and create+upload the BOSH release to the director

```
git clone https://github.com/svrc-pivotal/pks-cluster-override-release && cd pks-cluster-override-release
bosh create-release --force
bosh upload-release ./dev_releases/cluster-override/cluster-override-0+dev.1.yml 

```
4. Configure the addon from this repo
```
bosh -n update-config --name=pks-cluster-override --type=runtime ./addon.yml
```
5. Update your PKS clusters via the PKS CLI and/or Ops Manager "Apply Pending Changes" button with the PKS upgrade errand enabled.  This addon will automatically be installed on all worker nodes with the default manifest `pks-dpw-manifest.yml`



