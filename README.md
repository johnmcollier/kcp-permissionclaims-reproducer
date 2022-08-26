# kcp-permissionclaims-reproducer

To reproduce:

1) Start kcp with `kcp start`, and configure your Kubeconfig in another terminal to point to it

2) Create a workspace, `gitops`, in kcp: 
    ```bash
    kubectl kcp ws create gitops --enter
    ```

3) Create the APIExport and APIResourceSchemas inside the `gitops` workspace:
   
   ```bash
   kubectl apply -f gitops/apiresourceschema.yaml
   kubectl apply -f gitops/apiexport.yaml
   ```

4) Once the APIExport has been created, run the following command to retrieve the identity hash of the APIExport:

   ```bash
   kubectl get apiexport gitops -o jsonpath={.status.identityHash}
   ```
   
5) With the identity hash known, edit `has/apiexport.yaml` and `has/apibinding.yaml` and change `<identity-hash> to the identity hash that was retrieved.

6) Create a new workspace, `has`:
   
   ```bash
   kubectl kcp ws ..
   kubectl kcp ws create has --enter 
   ```
   
7) Inside the `has` workspace, create the APIResourceSchema, APIExport, and APIBinding resources for has:

   ```bash
   kubectl apply -f has/apiresourceschema.yaml
   kubectl apply -f has/apiexport.yaml
   kubectl apply -f has/apibinding.yaml
   ```
   
8) Validate that the APIBinding was created successfully:

   ```bash
   kubectl get apibinding has-binding -o yaml
   ```
   
   ```
   conditions:
   - lastTransitionTime: "2022-08-26T18:24:05Z"
     status: "True"
     type: Ready
   - lastTransitionTime: "2022-08-26T18:24:05Z"
     status: "True"
     type: APIExportValid
   - lastTransitionTime: "2022-08-26T18:24:05Z"
     status: "True"
     type: BindingUpToDate
   - lastTransitionTime: "2022-08-26T18:24:05Z"
     status: "True"
     type: InitialBindingCompleted
   - lastTransitionTime: "2022-08-26T18:24:05Z"
     status: "True"
     type: PermissionClaimsApplied
   - lastTransitionTime: "2022-08-26T18:24:05Z"
     status: "True"
     type: PermissionClaimsValid
   ```

9) If you run `kubectl get environments` or `kubectl get applicationsnapshotenvironmentbindings`, you'll receive the following error:


   ```
   johncollier@jcollier-mac kcp-permissionclaims-reproducer % kubectl get environments
   error: the server doesn't have a resource type "environments"
   
   ```