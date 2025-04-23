# Golden tips
1. Verify resource creation: --dry-run=client

# Fast K8s Developer:
1. kubectl run <POD>  --image=<IMAGE>
2. kubectl run <POD>  --image=<IMAGE>  --dry-run=client -o yaml
3. kubectl create deployment --image=<IMAGE> <DEPLOYMENT>
4. kubectl create deployment <DEPLOYMENT>  --image=<IMAGE> --replicas=4
5. kubectl expose pod <POD> --port=<PORT> --name <SVC> --type=<SVC_TYPE> --dry-run=client -o yaml

# Altering:
1. kubectl scale deployment <DEPLOYMENT> --replicas=4

# Expose:
1. kubectl expose pod <POD> --port=<PORT> --name <SVC> --dry-run=client -o yaml