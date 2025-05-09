---
Title: Deploying EKS clusters with CDK
date: 2025-05-09
tags:
- cdk
- eks
- aws
---

In the past I have found that deploying an EKS cluster with the CDK has ended me up in a state where I cannot access the cluster using `kubectl`. I did some research and found I needed to specify that the `mastersRole` of the cluster can be assumed by my principal.


```typescript
const mastersRole = new Role(this, "MastersRole", {
    assumedBy: new ArnPrincipal("<your principal or user/role arn>"),
});
// add other access, e.g.
// mastersRole.addManagedPolicy(ManagedPolicy.fromAwsManagedPolicyName("AdministratorAccess"));

const cluster = new Cluster(this, "Cluster", {
    version: KubernetesVersion.V1_32,
    kubectlLayer: new KubectlV32Layer(this, "kubectl"),
    defaultCapacity: 0,
    mastersRole,
});

cluster.addNodegroupCapacity("ManagedNodeGroup", {
    instanceTypes: [new InstanceType("t3.medium")],
    desiredSize: 2,
    minSize: 1,
    maxSize: 4,
    diskSize: 20,
    subnets: { subnetType: SubnetType.PRIVATE_WITH_EGRESS },
    amiType: NodegroupAmiType.AL2_X86_64,
});

mastersRole.addToPrincipalPolicy(new PolicyStatement({
    actions: [
    'eks:AccessKubernetesApi',
    'eks:Describe*',
    'eks:List*',
    ],
    resources: [cluster.clusterArn],
}))

new CfnOutput(this, 'ClusterName', { value: cluster.clusterName });
new CfnOutput(this, 'KubectlCommand', {
    value: `aws eks update-kubeconfig --name ${cluster.clusterName}`,
});
new CfnOutput(this, "RoleArn", {
    value: mastersRole.roleArn,
});
```
