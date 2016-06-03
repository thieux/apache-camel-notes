# Notes sur Apache Camel (juin 2016)

Apache Camel est un framework qui simplifie l'intégration des applications entre elles.
L'intégrateur n'a pas besoin de comprendre le fonctionnement interne de chaque application.
Il peut se concentrer sur la façon d'interagir avec chacune d'elle de l'extérieur.
Plus précisément Camel gère le routage de messages dans l'entreprise (EAI).
Une application typique gérant cette intégration est le middleware.
Avec Camel le développeur se concentre sur la glue liant les applications entre elles. Le développeur déclare des routes mais laisse Camel gérer comment ses routes sont implémentées.

L'implémentation est en Java. Elle est open source.
De nombreux protocoles de transports sont pris en charge.
Il existe également des greffons supportants des formats de données (CSV, JSON, Zip, etc.).
Camel est une implémentation alternative à Spring Integration.

Elle met à disposition des développeurs les patterns d'intégration d'entreprise (EIP). Il en existe plus de 60.
La référence des pattern d'intégration d'entreprise est le livre de Gregor Hohpe and Bobby Woolfe's : "Enterprise Integration Patterns: Designing, Building, and Deploying Messaging Solutions".

Le framework fournit également un DSL en Java et Spring XML.
En fournissant un langage, le développeur peut se focaliser les problèmes d'intégration en définissant des routes.
Camel définit de multiples DSL. Ils permettent de composer des ```Processors``` et ```Endpoints```. On peut écrire en Java, Groovy ou Scala.

Pour créer une règle de routage qui charge le contenu du dossier /tmp et l'envoie à une queue JMS.
On définit une route avec un DSL en Java :

```java
from("file:/tmp").to("jms:aQueue");
```

On utilise des URIs pour envoyer ou recevoir des message de manière uniforme.
Exemples d'URI : ```jml:aQueue``` ou ```file:/tmp```.

Attention, Camel n'est pas un bus de services. Il est spécialisé dans le routage des messages.
En revanche, il peut communiquer avec un bus (par ex. JMS) pour le transport des messages.

Le lexique de Camel est assez simple : les ```Components```, ```Endpoints```, ```Processors```, et DSLs.

Le ```Processor``` manipule et filtre les messages. Il gère ce qui se passe entre les ```Endpoint```s (EIP, Routing, Transformation, Mediation, Enrichment, Validation, Interception).
Les patterns sont tous définis comme des processeurs.

Les ```Components``` sont les points d'extension de Camel. Ils font la connexion aux autres systèmes (exemple de protocoles : File, JMS, HTTP, etc.). Ils fournissent des Endpoints.
Le coeur de Camel est assez petit : il contient moins de 15 ```Component```s.
Par exemple, ```FileComponent``` référencé par ```file``` dans ```from("file:data/inbox")``` crée des ```FileEndpoint```s.

Un ```Endpoint``` correspond à un des bouts d'un canal de communication.

Le ```Message``` contient les données transportées et routées par Camel.

Voilà le "Hello World!" du livre "Camel In Action" qui route un fichier de ```data/inbox``` vers ```data/outbox``` :

```java
public class FileCopierWithCamel {
  public static void main(String args[]) throws Exception {
    CamelContext context = new DefaultCamelContext();
    context.addRoutes(new RouteBuilder() {
      public void configure() {
        from("file:data/inbox?noop=true")
        .to("file:data/outbox");
        }
    });
    context.start();

    Thread.sleep(10000);
    context.stop();
  }
}
```

# Sources

* http://camel.apache.org/
* https://dzone.com/articles/open-source-integration-apache
* https://www.manning.com/books/camel-in-action
* http://blog.soat.fr/2010/01/eip-quest-ce-que-cest/

