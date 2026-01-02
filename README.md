# Introduction à Spring Web MVC

Spring Web MVC est le module Spring consacré au développement d’application Web et d’API Web. Le nom de ce module renvoie directement au modèle MVC (Modèle Vue Contrôleur). Le modèle MVC n’est pas réservé au développement Web et même, son application n’a pas vraiment de sens pour le développement d’API Web. Quoi qu’il en soit, les notions de modèle, de contrôleur et de vue sont centrales pour Spring Web MVC. Dans ce chapitre, nous rappellerons le principe du modèle MVC et nous verrons comment intégrer Spring Web MVC dans une application Spring Boot et dans une application sans Spring Boot.


- Le modèle est constitué de classes JAVA.

- La vue est constituée par des objets capables de générer des pages HTML. Spring Web MVC permet d'utiliser Tymeleaf (moteur de génération de vue).

- Pour le controller, qui est l'interface entre les deux et met à jour la vue, Spring Web MVC fournit un ensemble d'annotations.


## Intégration de Spring Web MVC

Spring Web MVC est un module pour le développement d’application Web ou d’API Web. Donc, cela suppose le recours à un serveur pour traiter les requêtes. Le Spring Framework ne fournit pas de serveur Web. Pour exécuter une application Spring Web MVC nous avons donc besoin de déployer notre application dans un serveur.

Comme il a été dit dans l’introduction générale au Spring Framework, ce dernier a été conçu comme une approche différente en terme d’architecture par rapport à J2EE (puis Java EE et Jakarta EE). La différence fondamentale est que chaque application Spring embarque son propre conteneur avec les services dont elle a besoin. Donc une application Spring Web MVC n’a pas besoin d’un serveur d’application Java EE complet comme Wildfly, GlassFish ou TomEE. Elle peut s’exécuter dans un conteneur Web plus léger comme Tomcat ou Jetty qui offre le service minimal dont une application Spring Web MVC à besoin : le lancement d’un serveur HTTP et la possibilité de déléguer le traitement des requêtes au code de l’application.

Le choix d’utiliser ou non Spring Boot pour configurer votre application va être déterminant pour l’intégration de Spring Web MVC et le déploiement de votre application.

### Intégration dans une application avec Spring Boot

Pour intégrer Spring Web MVC, vous devez rajouter une dépendance au module `spring-boot-starter-web`.

Il est fortement recommandé d’ajouter également une dépendance au module `spring-boot-devtools`. Ce dernier est très pratique pour la phase de développement. Il permet notamment le redémarrage à chaud du serveur lorsqu’on effectue une modification dans le code source.

*Lorsque la dépendance à spring-boot-starter-web est présente, le comportement de Spring Boot change au lancement de votre application. Il va automatiquement lancer un serveur HTTP (Tomcat par défaut) et déployer votre application dans ce serveur. Du point de vue de l’application, cela change radicalement le choix d’architecture. Plutôt que de disposer d’un serveur central Java EE dans lequel nous déployons nos applications, c’est chaque application qui dispose de son serveur embarqué.*

Le serveur est configurable à travers les nombreux paramètres fournis par Spring Boot. Le plus utile pour démarrer est sans doute le paramètre `server.port` qui permet de donner le port d’écoute du serveur (`8080` par défaut). Donc, si vous voulez que votre serveur écoute sur le port `9090`, il suffit d’ajouter dans le fichier `application.properties` :

```java
server.port = 9090
```

## L’encodage des paramètres de requêtes

L’encodage des paramètres des requêtes est source d’erreur dans le développement d’applications Web. En effet, même si l’encodage le plus couramment utilisé actuellement est `UTF-8`, il ne faut pas oublier que l’encodage par défaut sur le Web est Latin-1 (`ISO-8859-1`). Selon les navigateurs (et notamment suivant les versions des navigateurs), il peut y avoir des comportements légèrement différents. Ce n’est pas parce qu’une page HTML est encodée en UTF-8 que le formulaire qu’elle contient soumettra nécessairement des données en `UTF-8`. Pour contrôler au mieux le comportement des navigateurs, il est conseillé d’utiliser systématiquement l’attribut accept-charset sur la balise `<form>`. Cet attribut permet justement de spécifier l’encodage à utiliser pour envoyer les données au serveur :

```html
<form action="..." method="post" accept-charset="utf-8">
  <input type="text" name="nom">
  <input type="submit">Envoyer</input>
</form>
```

Côté serveur, vous devez vous assurer que l’encodage utilisé pour traiter les paramètres des requêtes est correctement positionné.

Pour une application avec Spring Boot, l’encodage par défaut pour les paramètres des requêtes est `UTF-8`. Si vous voulez le modifier, il faut déclarer la propriété `server.tomcat.uri-encoding` dans le fichier `application.properties` :
Changement de l’encodage par défaut pour une application Spring Boot

```java
server.tomcat.uri-encoding = iso-8859-1
```

