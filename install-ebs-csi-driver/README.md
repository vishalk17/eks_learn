# Installation of ebs driver

EKS EBS CSI Driver is a Container Storage Interface (CSI) driver for Amazon Elastic Kubernetes Service (EKS) that allows Kubernetes clusters to use Amazon Elastic Block Store (EBS) volumes for persistent storage.


### Associating IAM OIDC Provider with my cluster

```
eksctl utils associate-iam-oidc-provider --cluster <cluster-name> --approve --region <your-region>
```

### Creating an IAM role with the necessary permissions for the EBS CSI Driver and setting up a trust relationship between this IAM role and the Kubernetes service account.

```bash
eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster <cluster-name> \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve \
  --role-only \
  --role-name AmazonEKS_EBS_CSI_Driver_Role \
  --region <your-region>
```

### Install the AWS EBS CSI Driver Addon

```bash
eksctl create addon --name aws-ebs-csi-driver --cluster <cluster-name> --service-account-role-arn arn:aws:iam::<Account-ID>:role/AmazonEKS_EBS_CSI_Driver_Role --region <your-region> --force
```

### Install Storage class --> make it default if you wish to 

**Storage Class Name :** ebs-sc

```bash
kubectl apply -f ebs-storageclass.yaml
```
