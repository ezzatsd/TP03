# TP03 - Kubernetes

## 1. Contexte
Ce document presente le compte rendu du TP03 Kubernetes.  
Le travail couvre l'exploration du cluster, la gestion des contextes/namespaces, les deploiements declaratifs et imperatifs, les strategies de rollout/rollback, la manipulation des pods, et l'usage des labels/selectors.

## 2. Objectifs pedagogiques
- Maitriser les commandes `kubectl` de base et avancees.
- Comprendre le cycle de vie d'un `Deployment` et d'un `ReplicaSet`.
- Verifier les mecanismes de `scaling`, d'auto-healing et de rollback.
- Manipuler des pods en mode imperatif et via manifestes YAML.
- Utiliser les labels/selectors pour filtrer et observer les ressources.

## 3. Environnement de travail
- OS: macOS (terminal zsh)
- Cluster local: Minikube
- Namespace principal: `default`
- Outils utilises: `kubectl`, `jq`, JSONPath, port-forward

## 4. Methode
Le TP a ete realise en appliquant les commandes demandees, avec verification systematique apres chaque action:
- `kubectl get ...`
- `kubectl describe ...`
- `kubectl rollout status ...`
- `kubectl rollout history ...`
- Nettoyage final des ressources

Chaque etape a ete validee avec captures d'ecran (creation, verification fonctionnelle, nettoyage).

## 5. Resultats par bloc

### 5.1 Exploration cluster (etapes 1 a 4)
- Listing des nodes en formats standard, wide et YAML.
- Extraction des capacites via `jq` et JSONPath.
- Verification des contextes et namespaces.
- Creation/suppression d'un contexte dedie (`mini-system`).
- Preparation du dossier de travail TP03.

### 5.2 Deployment kuard declaratif (etapes 5 a 8)
- Creation et application de `kuard-deployment.yaml`.
- Verification `Deployment`, `ReplicaSet`, `Pods`, `selector`.
- Tests de scaling (Deployment vs ReplicaSet) et observation reconciliation controller.
- Verification `ownerReferences` et auto-healing (suppression d'un pod recree automatiquement).
- Validation acces applicatif via:
  - `kubectl port-forward --address 0.0.0.0 deployments/kuard 8081:8080`
  - acces navigateur: `http://localhost:8081`

### 5.3 Challenge imperatif kuard (etape 9)
- Creation imperatif du deployment.
- Scale et exposition service.
- Ajout de labels.
- Export YAML depuis cluster: `kuard-from-cluster.yaml`.
- Edition de l'objet.
- Nettoyage complet.

### 5.4 Strategies de deploiement (etapes 10 et 11)
- RollingUpdate valide (update image, suivi rollout, history, ReplicaSets).
- Rollback valide (`rollout undo` et verification etat stable).
- Strategie `Recreate` validee.
- Mise a jour d'image via CLI (`kubectl set image`).
- Retour arriere via `kubectl edit` (sans `rollout undo`).

### 5.5 Pods nginx et connectivite (etapes 12 et 13)
- Pod nginx cree en imperatif et via manifeste `nginx.yml`.
- Test crash (`kill 1`) et observation de l'increment `RESTARTS`.
- Test intra-cluster via pod `testpod` (alpine) vers IP du pod web.
- Nettoyage des pods en fin de sequence.

### 5.6 Labels & selectors (etapes 14 et 15)
- Challenge `pingpod`: labels, ajout dynamique, filtrage, logs streaming.
- Pod `whoami` via `monpod.yml`.
- Requetes par selector et affichage labels (`-L`, `--show-labels`).
- Recuperation `containerID` via JSONPath.
- Nettoyage final valide.

## 6. Difficultes rencontrees et corrections
- Certaines images `gcr.io/kuar-demo/...` ont provoque `ImagePullBackOff`.
- Solution appliquee: utilisation d'images `kuard` pullables sur l'environnement local pour valider les mecanismes Kubernetes attendus (deploiement, rollout, rollback, port-forward).
- Erreur zsh sur `custom-columns` avec crochets `[]` corrigee en entourant l'option avec des quotes.

## 7. Fichiers produits
- `kuard-deployment.yaml`
- `kuard-from-cluster.yaml`
- `nginx.yml`
- `monpod.yml`

## 8. Conclusion
Les objectifs du TP03 ont ete atteints.  
Les manipulations ont permis de valider l'administration courante d'un cluster Kubernetes: exploration, deploiement, supervision, mise a jour, rollback, etiquetage et nettoyage des ressources.  
Le rendu est conforme aux attendus fonctionnels du TP, avec preuves d'execution par commandes et captures.
