1. Deployments:
k rollout status deployment/<deployment>

k rollout history deployment/<deployment>

k set image deployment/<deployment> <container>=<image><tag>
(Will generate new revision)

k rollout undo deployment/<deployment>