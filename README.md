# Sécurité

Comme tout SI appartenant à l'administration la Fabrique Numérique et les produits qu'elle réalise se doivent d'etre en conformité avec le référentiel général de sécurité (RGS) établi par l'ANSSI. Ce dernier liste l'ensemble des méthodes, conventions et bonnes pratiques permettant d'assurer un niveau de sécurité satisfaisant du SI et de ses composants. L'ensemble de la documentation nécessaire à sa compréhension est disponible sur [le site de l'ANSSI](https://www.ssi.gouv.fr/entreprise/reglementation/confiance-numerique/liste-des-documents-constitutifs-du-rgs-v-2-0/).

Dans ce context tout composant du SI de la Fabrique Numérique (produit, service, environnement de déploiement...) se doit de respecter les régles de sécurité minimales édictées dans le présent document et regroupées dans les chapitres suivants:

1. Les personnes et les rôles
2. Les données
3. Les applications
4. Les services
5. L'infrastructure & les environnements

## 1. Les personnes et les rôles

L'organisation [SocialGouv de Github](https://github.com/orgs/SocialGouv/) est le referentiel de la Fabrique Numérique. Elle rescence l'ensemble des personnes travaillant sur les applications et l'infrastructure. Elle est divisée en plusieurs groupes, ceux relatifs aux applications ou produits et ceux concernant les outils internes ou la gestion des environnements.

Il y a donc deux rôles distincts, les membres des equipes produits ou *startups* et les administrateurs.

**Les membres d'une startup** ont différents profils: `développeurs`, `UX/UI`, `product owners` ou `intrapreneurs`. Ils sont le plus souvent externes et centrés sur un unique produit. Leur positionnement et leur rôle impliquent des permissions spécifiques concernant les accès aux ressources mises à leur disposition.

**Les administrateurs** sont membres de la [Core Team](https://github.com/orgs/SocialGouv/teams/core-team/members) ou membres de l'équipe [Ops](https://github.com/orgs/SocialGouv/teams/ops/members). Pour des raisons opérationnelles les administrateurs ont les pleins privilèges sur l'ensemble de l'infrastructure et de ses ressources, car ils sont responsables des *actions d'administration* définies comme suit par l'ANSSI:

> Ensemble des actions d’installation, de suppression, de modification et de consulta- tion de la configuration d’un système participant au SI et susceptibles de modifier le fonctionnement ou la sécurité de celui-ci.

Quelque soit le rôle auquel il aspire, un membre doit commencer par relier son compte [Github](https://github.com/) à L'organisation [SocialGouv](https://github.com/orgs/SocialGouv/) afin de se voir octroyer les accès aux ressources avec le niveau de permission adéquat.

Une revue des comptes est effectuée régulièrement, lors de l'arrivée ou du départ d'un membre d'une équipe, ainsi que de façon semestrielle.
Suite à chaque revue, les comptes d'accès aux ressources n'appartenant pas à un membre de l'organisation [SocialGouv](https://github.com/orgs/SocialGouv/) sont radiés.

## 2. Les données

Différents types de données sont utilisés par les applications. On peut distinguer les données dites `techniques` des données dites `à caractère personnel`. Il est aussi possible de distinguer les données en fonction de leurs criticités, certaines d'entre elles etant plus sensibles que d'autres.

### 2.1. Les données à caractère personnel

Concernant les individus, la **CNIL** répertorie au moins trois types de données dont elle donne les définitions.

#### Les données privées

La définition des données privées selon la [CNIL](https://www.cnil.fr/fr/definition/donnee-personnelle) est la suivante:

> Une donnée personnelle est toute information se rapportant à une personne physique identifiée ou identifiable.
> 
> Une personne physique peut être identifiée :
> 
> - Directement, à partir d’une seule donnée (exemple : nom et prénom) ;
> - Indirectement, à partir du croisement d’un ensemble de données (exemple : par un numéro de téléphone ou de plaque d’immatriculation, ou encore une femme vivant à telle adresse, née tel jour et membre dans telle association).


#### Les données sensibles

La définition des données sensibles selon la [CNIL](https://www.cnil.fr/fr/definition/donnee-sensible) est la suivante:

> Les données sensibles forment une catégorie particulière des données personnelles.
> 
> Ce sont des informations qui révèlent la prétendue origine raciale ou ethnique, les opinions politiques, les convictions religieuses ou philosophiques ou l'appartenance syndicale, ainsi que le traitement des données génétiques, des données biométriques aux fins d'identifier une personne physique de manière unique, des données concernant la santé ou des données concernant la vie sexuelle ou l'orientation sexuelle d'une personne physique. 

#### Les données de santé

La définition des données de santé de la [CNIL](https://www.cnil.fr/fr/quest-ce-ce-quune-donnee-de-sante) est la suivante:

> Les données à caractère personnel concernant la santé sont les données relatives à la santé physique ou mentale, passée, présente ou future, d’une personne physique (y compris la prestation de services de soins de santé) qui révèlent des informations sur l’état de santé de cette personne.

#### Les données publiques

Par essence, entre dans la catégorie des données publiques, toute donnée ne pouvant être associée aux catégories susmentionnées.

A défaut d'une définition précise, la [CNIL](https://www.cnil.fr/fr/definition/donnee-personnelle) en donne un exemple:

> Des coordonnées d’entreprises (par exemple, l’entreprise « Compagnie A » avec son adresse postale, le numéro de téléphone de son standard et un courriel de contact générique « compagnie1@email.fr ») ne sont pas, en principe, des données personnelles.

### 2.2. Les données techniques

#### Les données privées ou sensibles

Certaines données techniques nécessaires au fonctionnement d'une application peuvent être considérées comme privées ou sensibles. Quelques exemples:

- Les mots de passe des utilisateurs
- Les secrets d'un application (example: jeton d'accès à un service tiers)
- Les certificats https

#### Les données publiques

Le code source des applications est considéré comme une donnée publique. Il est hébgergé chez [Github](https://github.com/) sur des repositories publics et de ce fait est accessible à tous, sans restriction.

Le fait que le code des applications soit public implique une vigilence particulière quant à la nature des informations qui sont transférées chez [Github](https://github.com/). **Aucune donnée technique sensible ou à caractère personnel ne doit figurer dans le code source des applications**.

### 2.3. Les différents types de traitements

En fonction du type de données, les regles à observer diffèrent.

- Les données dites `sensibles` et `de santé` **doivent etre chiffrées**, soit en base, soit sur le système de fichiers, tout au long de la vie de l'application.
- Les `mots de passe` des utilisateurs d'une application **doivent etre chiffrés** en base de façon irreversible.
- Les `secrets` et `certificats` **ne doivent pas etre accessibles** en dehors de l'environnement d'execution de l'application.
- Les exports de base de données de production **doivent etre anonymisés** s'ils sont voués à être exploités dans un environnement différent.

Par ailleurs, tous les composants nécessaires au fonctionnement d'une application sont soumis aux règles de sécurité relatives aux données. Il faut considérer la sécurité d'une application dans son ensemble, en prenant en compte les services auxquels elle est connectée ainsi que les environnements dans lesquels elle est déployée.

## 3. Les applications (à faire...)

- DAT & Matrice des flux
- La gestion des comptes utilisateurs et administrateurs
    - la gestion de passwords
    - La revue régulière des comptes et privilèges
- Analyse de risques & homologation

[PSSIE](https://www.ssi.gouv.fr/uploads/2014/11/pssie_anssi.pdf):

> Tout système d’information doit faire l’objet d’une décision d’homologation de sa sécurité avant sa mise en exploitation dans les conditions d’emploi définies. L’homologation est l’acte selon lequel l’autorité atteste formellement auprès des utilisateurs que le système d’information est protégé conformément aux objectifs de sécurité fixés. La décision d’homologation est prise par l’autorité d’homologation (désignée par l’autorité qualifiée), le cas échéant après avis de la commission d’homologation. Cette décision s’appuie sur une analyse de risques adaptée aux enjeux du système considéré, et précise les conditions d’emploi.

- Le respect du RGPD  
[Analyse d'impact de la protection des données de la CNIL](https://www.cnil.fr/sites/default/files/atoms/files/infographie_aipd.pdf)  
[Liste des traitements pour lesquels une AIPD est requise](https://www.cnil.fr/sites/default/files/atoms/files/liste-traitements-aipd-requise.pdf)

## 4. Les services

Le recours aux services tiers peut etre à l'origine de fuites de données, qu'elles soient `techniques` ou `à caractère personnel`.
L'utilisation de solutions de stockage, de messagerie ou encore de statistiques peut entrainer la divulgation de données sensibles et mettre à mal la sécurité de nos données comme de nos environnements.

Il faut considérer avec le soin le choix des services auxquels fait appel une application, ainsi que le choix données qui sont amenées à transiter par ces services.

Par précaussion, l'usage des services internes, ou *on premise*, est donc préféré à l'usage des services externes, ou *SaaS*. Différents services internes homologués sont d'ores et déjà à la disposition des applications.

Ci-dessous la liste exhaustive des services existants:

| Services | Azure | SaaS |
| -------- | :------: | :------: |
| [Github](https://github.com/)   |  | X |
| [Gitlab](https://gitlab.com/)   | X |  |
| [Matomo](https://matomo.com/)   | X |  |
| [Sentry](https://sentry.io/)   | X |  |
| [Zammad](https://zammad.com/)   | X |  |
| [Tipimail](https://tipimail.com/) |  | X |
| [Postgres](https://postgresql.org/) | X |  |
| [Let's Encrypt](https://letsencrypt.org/) |  | X |
| [Elasticsearch](https://www.elastic.co/) |  | X |

Lorsqu’ils sont disponibles, des produits ou des services de sécurité labellisés (certifiés, qualifiés) par l’ANSSI doivent être utilisés.

## 5. Infrastructure & environnements

L'infrastructure de la Fabrique Numérique est hébergée par Microsoft Azure. L'hébergeur fourni l'ensemble des services basiques nécessaires: machines, stockages, DNS, proxies, firewalls, bases de données...

Les machines utilisées dans les environnements de tests ou de production sont soit des machines virtuelles gérées par les équipes produit, soit des clusters Kubernetes gérés par les administrateurs de la Fabriques Numériques.

Meme si certains cas peuvent faire exceptions, il est vivement recommandé de s'appuier sur les environnements administrés par la Fabrique Numérique et de ne pas faire recours aux machines virtuelles. En effet ces dernieres impliquent une sécurisation à la charge de l'équipe produit qui sera seule responsable en cas de manquements aux règles de sécurités sur ses environnements.

La politique de sécurité des clusters Azure Kubernetes est décrite dans [la documentation en ligne AKS](https://docs.microsoft.com/fr-fr/azure/aks/concepts-security).

Dans le cas ou une équipe produit souhaiterait administrer elle-meme ses environnements, les principes fondamentaux décris dans [le guide de cloisonnement système ANSSI](https://www.ssi.gouv.fr/guide/recommandations-pour-la-mise-en-place-de-cloisonnement-systeme/) et [les recommandations de sécurité relatives à un système GNU/Linux](https://www.ssi.gouv.fr/guide/recommandations-de-securite-relatives-a-un-systeme-gnulinux/) sont à appliquer.

Quelques soit le mode d'hébergement, machines virtuelles ou cluster AKS, il est soumis aux [SLA Azure](https://azure.microsoft.com/en-gb/support/legal/sla/virtual-machines/v1_9/)

La Fabrique numérique administre une infrastructure permettant de déployer les applications, composée de deux clusters: **STAGING** et **PRODUCTION**.

- **STAGING** héberge les versions de test des applications ou *feature branches*.
- **PRODUCTION** sert exclusivement à l'hébergement des versions de production des applications.

Seuls les administrateurs de l'infrastructure ont un accès direct aux clusters ainsi qu'aux autres ressources mises à disposition par la plateforme (systèmes de fichiers, proxies, CI/CD, bases de données...).

Pour les membres des startups, comme les développeurs et les product owners, un accès aux ressources de l'environnement de staging est possible par l'intermédiaire de clients web tels que: `Rancher` pour les pods du cluster, `Gitlab` pour le CI/CD  ou encore `Hasura Console` pour les bases de données Postgres.

#### Accès à l'infrastructure de STAGING

|  | Cluster | BDD | Stockage | Reverse proxy | CI/CD |
| -------- | -------- | -------- | -------- | -------- | -------- |
| **Startup** | HTTPS | HTTPS | *aucun accès* | *aucun accès* | HTTPS (interne) |
| **Administrateur** | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP |

#### Accès à l'infrastructure de PRODUCTION

|  | Cluster | BDD | Stockage | Reverse proxy | CI/CD |
| -------- | -------- | -------- | -------- | -------- | -------- |
| **Startup** | *aucun accès* | *aucun accès* | *aucun accès* | *aucun accès* | HTTPS (interne) |
| **Administrateur** | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP | HTTPS<br/>TCP/IP |

Il est à noté que le cluster de production est certifié [HDS](https://esante.gouv.fr/labels-certifications/hds/certification-des-hebergeurs-de-donnees-de-sante) et peut donc héberger des applications traitants des données de santé.


