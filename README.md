hello 
Templates dynamiques
Helm utilise des fichiers de templates (en YAML + Go templating) pour générer dynamiquement les manifests Kubernetes.

Fichier values.yaml
C’est là que tu définis toutes les valeurs personnalisables (noms, ports, images, environnement, etc.).
Ces valeurs sont injectées dans les templates via {{ .Values.xxx }}.

Variables locales ($var)
Tu peux créer des variables dans les templates pour simplifier le code :

yaml
Copier
Modifier
{{- $env := .Values.context.env }}
Conditions (if)
Permet de générer ou ignorer des ressources selon une valeur booléenne :

yaml
Copier
Modifier
{{- if .Values.enabled }}
  ... YAML ...
{{- end }}
Boucles (range)
Pour générer plusieurs objets dynamiquement (ex. plusieurs services, ports, routes) :

yaml
Copier
Modifier
{{- range .Values.microServices }}
  ... YAML ...
{{- end }}
Blocs with
Pour simplifier l’accès à des sous-objets :

yaml
Copier
Modifier
{{- with .Values.service }}
  {{ .name }}
{{- end }}
Filtres (toYaml, nindent)
Pour insérer des blocs complexes dans un manifest YAML tout en respectant l’indentation :

yaml
Copier
Modifier
{{- toYaml .env | nindent 12 }}
Gestion des environnements
Tu peux adapter dynamiquement les ressources selon l’environnement (dev, qa, prod) avec :

yaml
Copier
Modifier
{{- if eq $env "dev" }}
Gestion des releases
Helm crée une "release" avec un nom unique, ce qui te permet de gérer le versioning, la mise à jour et la rollback.

Packaging avec Charts
Un Chart Helm regroupe tous les templates, valeurs et métadonnées nécessaires pour déployer une application.



Une release Helm représente une instance déployée d’un chart Helm dans un cluster Kubernetes. Chaque fois qu’un chart est installé à l’aide de la commande `helm install`, Helm crée une release unique identifiée par un nom. Cette release contient toutes les informations nécessaires à son déploiement, comme les valeurs, l’historique, et l’état courant des ressources déployées.

Grâce à ce système, Helm permet :
- Le versioning : chaque mise à jour d’une release est historisée (via `helm upgrade`).
- Le rollback : on peut revenir à une version précédente avec `helm rollback`.
- Le suivi d’état : `helm status` permet de voir l’état actuel d’une release.

Les releases facilitent la gestion de plusieurs déploiements du même chart dans différents environnements (par exemple : `myapp-dev`, `myapp-prod`).
