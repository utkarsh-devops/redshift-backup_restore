# manage-redshift

Amazon Redshift is a cloud data warehouse tool included in AWS. You can manage your data warehouse clusters using the AWS management Console or using the Amazon Redshift APIs. This post demonstrates a small subset of the Redshift API to create clusters from a snapshot and vice versa using a bash shell script.

If you want to delete a cluster, you have a choice of saving it first as a snapshot or deleting it with no final snapshot. With a snapshot, you can restore the cluster at a later time. If you choose not to save it as a snapshot, then the cluster will be deleted permanently with no restore option. In this case, you would have to recreate the cluster manually from scratch.

The script below only gives the user the option to save an existing cluster to a snapshot or restoring a cluster from an existing snapshot. To provide a measure of security and protection, you should set up IAM policies that walls off clusters and snapshots from accidental deletes or unprivileged access to your data. This topic is not in the scope of this article.

The script will allow you to:

* (1) List clusters
* (2) List snapshots
* (3) Create cluster from snapshot
* (4) Create snapshot from cluster (also deletes the cluster)
* (5) Quit

Prerequisite: Install the AWS Command Line API.

Here are some additional useful Redshift API commands that are not included in the script above:

**Create a cluster**
`
aws redshift create-cluster  --db-name 'mydb' 
    --cluster-identifier "my-cluster"  
    --cluster-type 'multi-node' 
    --node-type 'dc1.large'  
    --master-username 'jae'  
    --preferred-maintenance-window 'Mon:19:00-Mon:19:30'  
    --automated-snapshot-retention-period '0'  
    --port '5439'  
    --allow-version-upgrade   
    --cluster-subnet-group-name 'my-redshift-subnet' 
    --vpc-security-group-ids 'sg-12345678' 
    --number-of-nodes '2'  
    --master-user-password [redacted_password]
` 
**Delete cluster without making a snapshot (this operation takes some time to finish)**
`
aws redshift delete-cluster --cluster-identifier my-cluster --skip-final-cluster-snapshot
`
