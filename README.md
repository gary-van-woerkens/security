# Sécurité

1. Les personnes et les rôles
2. Les données
3. Les environnements
4. Les applications
5. Les services
6. L'infrastructure

## Les personnes et les rôles

L'organisation [SocialGouv de Github](https://github.com/orgs/SocialGouv/) est notre referentiel. Elle rescence l'ensemble des personnes travaillant sur les applications et l'infrastructure. Elle est divisée en plusieurs groupes, ceux relatifs aux applications ou produits et ceux concernant les outils internes ou la gestion des environnements.

Il y a donc deux rôles distincts, les membres des equipes produits ou *startups* et les administrateurs ou *core team*.

### Les membres des startups

Les membres d'une startup ont différents profils: `développeurs`, `UX/UI`, `product owners` ou `intrapreneurs`. Ils sont le plus souvent externes et centrés sur un unique produit. Leur positionnement et leur rôle implique des permissions spécifiques concernant les accès aux ressources mises à leur disposition.

### Les administrateurs



## Les données

### Les différents types de données

#### Les données à caractère personnel

##### Les données privées

Concernant les données liées aux individus la définition de la [CNIL](https://www.cnil.fr/fr/definition/donnee-personnelle) est la suivante:

> Une donnée personnelle est toute information se rapportant à une personne physique identifiée ou identifiable.
> 
> Une personne physique peut être identifiée :
> 
> - Directement, à partir d’une seule donnée (exemple : nom et prénom) ;
> - Indirectement, à partir du croisement d’un ensemble de données (exemple : par un numéro de téléphone ou de plaque d’immatriculation, ou encore une femme vivant à telle adresse, née tel jour et membre dans telle association).


##### Les données sensibles

Concernant les données liées aux individus la définition de la [CNIL](https://www.cnil.fr/fr/definition/donnee-sensible) est la suivante:

> Les données sensibles forment une catégorie particulière des données personnelles.
> 
> Ce sont des informations qui révèlent la prétendue origine raciale ou ethnique, les opinions politiques, les convictions religieuses ou philosophiques ou l'appartenance syndicale, ainsi que le traitement des données génétiques, des données biométriques aux fins d'identifier une personne physique de manière unique, des données concernant la santé ou des données concernant la vie sexuelle ou l'orientation sexuelle d'une personne physique. 

##### Les données de santé

La définition de la [CNIL](https://www.cnil.fr/fr/quest-ce-ce-quune-donnee-de-sante) est la suivante:

> Les données à caractère personnel concernant la santé sont les données relatives à la santé physique ou mentale, passée, présente ou future, d’une personne physique (y compris la prestation de services de soins de santé) qui révèlent des informations sur l’état de santé de cette personne.

##### Les données publiques

Par essence, rentre dans la catégorie des données publiques, toute donnée n'appartenant pas aux catégories susmentionnées.

La [CNIL](https://www.cnil.fr/fr/definition/donnee-personnelle) en donne un exemple:

> Des coordonnées d’entreprises (par exemple, l’entreprise « Compagnie A » avec son adresse postale, le numéro de téléphone de son standard et un courriel de contact générique « compagnie1@email.fr ») ne sont pas, en principe, des données personnelles.

#### Les données techniques

##### Les données privées ou sensibles

Certaines données techniques nécessaires au fonctionnement d'une application peuvent être considérées comme privées ou sensibles. Quelques exemples:

- Les mots de passe des utilisateurs
- Les secrets d'un application (example: jeton d'accès à un service tiers)
- Les certificats https

##### Les données publiques

Le code source des applications est considéré comme une donnée publique. Il est hébgergé chez [Github](https://github.com/) sur des repositories publics et de ce fait est accessible à tous, sans restriction.

Le fait que le code des applications soit public implique une vigilence particulière quant à la nature des informations qui sont transférées chez [Github](https://github.com/). **Aucune donnée technique sensible ou à caractère personnel ne doit figurer dans le code source des applications**.

### Les différents types de traitements

En fonction du type de données, les regles à observer diffèrent.

- Les données dites `sensibles` et `de santé` **doivent etre chiffrées**, soit en base, soit sur le système de fichiers, tout au long de la vie de l'application.
- Les `mots de passe` des utilisateurs d'une application **doivent etre chiffrés** en base de façon irreversible.
- Les `secrets` et `certificats` **ne doivent pas etre accessibles** en dehors de l'environnement d'execution de l'application.
- Les exports de base de données de production **doivent etre anonymisés** s'ils sont voués à être exploités dans un environnement différent.

Par ailleurs, tous les composants nécessaires au fonctionnement d'une application sont soumis aux règles de sécurité relatives aux données. Il faut considérer la sécurité d'une application dans son ensemble, en prenant en compte l'infrastructure qui la supporte, tout comme les services auxquels elle est connectée, ainsi que les environnements dans lesquels elle est déployée.

## Les environnements

- En fonction de l'environnement t'as pas le droit aux memes trucs, selon qui tu es.
- Les accès.

## Les applications

- DAT
- Matrice des flux

## Les services

- En fonction du type de données qui transite, t'as pas le droit aux memes services.
- Tu dois etre sur que ton service est sécure (garantis?).

## L'infrastructure

L'infrastructure permettant de déployer les applications est composée de deux cluster: **STAGING** et **PRODUCTION**.

- **STAGING** héberge les versions de test des applications ou *feature branches*.
- **PRODUCTION** sert exclusivement à l'hébergement des versions de production des applications.

Seuls les administrateurs de l'infrastructure ont un accès direct aux clusters ainsi qu'aux autres ressources mises à disposition par la plateforme (systèmes de fichiers, proxies, CI/CD, bases de données...).

Pour les développeurs et les product owners, un accès aux ressources de l'environnement de staging est possible par l'intermédiaires de clients web tels que: `Rancher` pour les pods du cluster, `Gitlab` pour le CI/CD  ou encore `Hasura Console` pour les bases de données Postgres.

#### Accès à l'infrastructure de STAGING

|  | Cluster | BDD | Stockage | Reverse proxy | CI/CD |
| -------- | -------- | -------- | -------- | -------- | -------- |
| **Développeur** | HTTPS | HTTPS | *aucun accès* | *aucun accès* | HTTPS (interne) |
| **Administrateur** | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP |

#### Accès à l'infrastructure de PRODUCTION

|  | Cluster | BDD | Stockage | Reverse proxy | CI/CD |
| -------- | -------- | -------- | -------- | -------- | -------- |
| **Développeur** | *aucun accès* | *aucun accès* | *aucun accès* | *aucun accès* | HTTPS (interne) |
| **Administrateur** | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP |

Il est à noté que le cluster de production est certifié [HDS](https://esante.gouv.fr/labels-certifications/hds/certification-des-hebergeurs-de-donnees-de-sante) et peut donc héberger des applications traitants des données de santé.



