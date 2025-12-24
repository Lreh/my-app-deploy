copy to apps to add to argocd


If Prune=False in root-app:
 Push Git change $\rightarrow$ THEN run kubectl delete.

 is it possible to set prune = true fos a spcific deploy

 Yes, it is absolutely possible. You have two ways to do this: you can control it at the Application level (the child app) or using Annotations (specific resources).

 1. The Child App Level (Recommended)
 You can leave prune: false on your Root App (for safety) but enable prune: true on your Guestbook App. This means:

 If you delete a file inside the Guestbook repo, it gets deleted from the cluster.

 But if you delete the Guestbook YAML itself from the Root repo, the Root app won't delete the whole service until you manually tell it to.

 Update your apps/guestbook.yaml:

 YAML
 spec:
   syncPolicy:
     automated:
       prune: true      # Only this app will auto-delete its internal resources
       selfHeal: true