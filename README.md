# kcp-permissionclaims-reproducer

To reproduce:

1) Create a workspace, `gitops`, in kcp: 
    ```bash
    kubectl kcp ws create gitops --enter
    ```

2) Create the APIExport and APIResourceSchemas inside the `gitops` workspace:
   
   ```
   kubectl apply -f gitops/apiresourceschema.yaml
   kubectl apply -f gitops/apiexport.yaml
   ```

