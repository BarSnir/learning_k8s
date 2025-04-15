## Some rollout commands
```k rollout status deployment.apps/myapp-deployment``` --> Status
```k rollout history deployment.apps/myapp-deployment``` --> History
```k rollout undo  deployment.apps/myapp-deployment``` --> Undo one version before

## Using records (Deprecated)
```k create -f deployment-file-path/resource.yaml --record``` (cuase of change)

## Applying resources on-flight
```k edit deployment myapp-deployment --record```