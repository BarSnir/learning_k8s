# Get Pods in Namespace
k -n <ns> get po

# Switch context to Namespace
k config set-context $(k config current-context) --namespace=dev

# Get all resources from all Ns
k get <resource> --all-namespaces

# Verify resource first:
--dry-run=client