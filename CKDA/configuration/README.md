# Important notes for the section:

## Config-Map-Mandatory properties
apiVersion: v1
kind:
metadata:
data:

## Secrets-Mandatory properties
apiVersion: v1
kind:
metadata:
data:

## Secrets cmds
k get secrets
k get secret <secret-name> -o yaml
echo -n "secret" | base64
echo -n "secret" | base64 --decode

## Entire secret to env
envFrom:
    secretRef:
        name: <secret-name>

## Single Secret
env:
    - name: ENV_VAR_NAME
      valueFrom:
        secretKeyRef:
            name: <secret-name>
            key: <key-name-in-secret>

## Volumes: Entire secret to env files
volumes:
    - name: app-secret-volume
      secret:
        secretName: app-secret

## Encryption
1. Install etcd-client + hexdump[bsdmainutils]
2. Create EncryptionConfiguration resource [Optional for Demo] [/etc/kubernetes/enc/enc.yaml]
``` 
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <BASE64>
      - identity: {} 
```
3. Verify that kubernetes-apiserver.yaml is configured to encrypt. Link --> ```https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#use-the-new-encryption-configuration-file```
4. Create new secret.
5. Verify encryption:
```ETCDCTL_API=3 etcdctl --cacert=/var/lib/minikube/certs/etcd/ca.crt --cert=/var/lib/minikube/certs/etcd/server.crt --key=/var/lib/minikube/certs/etcd/server.key get /registry/secrets/<NS>/<SECRET-NAME> | hexdump -C ```

5. Encrypt all secrets:
```kubectl get secrets --all-namespaces -o json | kubectl replace -f -```



## SA Tokens:
```kubectl create token dashboard-sa```