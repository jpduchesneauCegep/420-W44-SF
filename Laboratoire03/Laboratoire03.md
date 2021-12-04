# Laboratoire 03 - Kubernetes

Le but de se laboratoire est de monter un site Wordpress avec MySQL dans Kubernetes. Wordpress doit avoir 3 replica.

Le laboratoire s'effectue en groupe de 4 maximum. Vous devez créer vos objets dans un espace de nom qui contient la liste de vos matricules séparés par un tiret. Exemple "1234567-2134567-3214567-2234567".

Le temps alloué est de 2 séances. La remise est pour le 6 septembre 2021 minuit.

Vous devez fournir :

- Tous les fichiers YAML
- Un dessin qui illustre les noeuds, les services et les pods ainsi que leurs relations
- Une vidéo d'environ 10 mins (Maximum 15 mins) :
  - Explication de tous vos fichiers
  - Démonstration que le site fonctionne
  - Démonstration que vous êtes capable de faire une mise à l'échelle

À se rappeler :

- Vous aurez besoin d'un volume persistant pour la base de données
- Il faut passer les informations de la base de données à Wordpress et à MySQL par des variables d'environnement
- Si vous exposez votre MySQL avec un service, vous pouvez le contacter avec son adresse DNS

Pour que le montage du volume fonctionne bien pour MySQL, vous devez ajouter une propriété "securityContexte" dans la spec du pod ou du template et ajouter la clef "runAsUser: 2000"  :

```yaml
...
  spec:
    securityContext:
      runAsUser: 2000
```

Références à lire :

- https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/
- https://hub.docker.com/_/wordpress
- https://hub.docker.com/_/mysql
- Outils d'enregistrement : OBS, Screencast-o-matic, etc.

test fork
