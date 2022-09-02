<!--
---
linkTitle: "TektonChain"
weight: 9
---
-->
# Tekton Chain

TektonChain custom resource allows user to install and manage [Tekton Chains][chains].

TektonChain is an optional component and currently cannot be installed through TektonConfig. It has to be installed separately.

To install TektonChain on your cluster follow steps as given below:

- Make sure Tekton Pipelines is installed on your cluster, using the Operator.

- Create the TektonChain CR.

    - On Kubernetes, TektonChain CR is as below:

    ```yaml
    apiVersion: operator.tekton.dev/v1alpha1
    kind: TektonChain
    metadata:
      name: chain
    spec:
      targetNamespace: tekton-pipelines
    ```

    - On OpenShift, TektonChain CR is as below:

    ```yaml
    apiVersion: operator.tekton.dev/v1alpha1
    kind: TektonChain
    metadata:
      name: chain
    spec:
      targetNamespace: openshift-pipelines
    ```

- Check the status of installation using following command:

    ```sh
    kubectl get tektonchains.operator.tekton.dev
    ```

## Chain Config 

There are some fields which you can define on Tekton Chains CR to configure the behaviour of the chains

### Properties (Mandatory)

 - `targetNamespace`

    Setting this field to provide the namespace in which you want to install the chains component.

### Properties (Optional)

These fields don't have default values so will be considered only if user passes them. By default, Operator won't add
these fields in CR and won't configure for chains.

The Default values for some of these fields are already set in chains and are not set by Operator. If user passes some 
values then those will be set for the particular field.

Details of the field can be found in [Tekton Chains Config][chains-config]

Chains CR will look like this after providing all the fields.

```yaml
apiVersion: operator.tekton.dev/v1alpha1
kind: TektonChain
metadata:
  name: chain
spec:
  targetNamespace: tekton-pipelines
  artifacts.taskrun.format: tekton
  artifacts.taskrun.storage: tekton,oci (comma separated values)
  artifacts.taskrun.signer: x509
  artifacts.oci.storage: oci (comma separated values)
  artifacts.oci.format: simplesigning
  artifacts.oci.signer: x509
  storage.gcs.bucket: #value
  storage.oci.repository: #value
  storage.oci.repository.insecure: #value (boolean - true/false)
  storage.docdb.url: #value
  storage.grafeas.projectid: #value
  storage.grafeas.noteid: #value
  builder.id: #value
  signers.x509.fulcio.enabled: #value (boolean - true/false)
  signers.x509.fulcio.address: #value
  signers.x509.fulcio.issuer: #value
  signers.x509.fulcio.provider: #value
  signers.kms.kmsref: #value
  signers.kms.kmsref.auth.address: #value
  signers.kms.kmsref.auth.token: #value
  signers.kms.kmsref.auth.oidc.path: #value
  signers.kms.kmsref.auth.oidc.role: #value
  signers.kms.kmsref.auth.spire.sock: #value
  signers.kms.kmsref.auth.spire.audience: #value
  transparency.enabled: #value (boolean - true/false)
  transparency.url: #value
```

[chains]:https://github.com/tektoncd/chains
[chains-config]:https://github.com/tektoncd/chains/blob/main/docs/config.md