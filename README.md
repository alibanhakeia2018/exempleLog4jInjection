# CVE-2022-23305 Log4j JDBCAppender injection SQL POC

Il s'agit d'une application très simple basée sur Spring Boot qui démontre la vulnérabilité CVE-2022-23305. Elle utilise Apache Maven, Spring Boot, Spring MVC et la base de données en mémoire H2 pour enregistrer une seule entrée simple, prise comme paramètre de chaîne de requête URL. Étant donné que Log4J est configuré pour utiliser un JDBCAppender, il est vulnérable à l'injection SQL.

Voir src/main/java/poc/InjectionController.java pour l'instruction de journalisation.
Voir le dossier src/main/resource pour tous les fichiers de configuration.

Vous pouvez exécuter l'application Docker, comme suit :
 docker build --tag log4j-poc .
 docker run -p 8080:8080 log4j-poc

L'application sera disponible à http://localhost:8080/.

Pour exploiter la vulnérabilité, soumettez une déclaration SQL injectée comme le paramètre qui est enregistré :
 "http://localhost:8080/?param=');insert into logs values('alibabntest"

Le retour listera les entrées de journal ajoutées, contenant celle qui a été ajoutée par l'injection SQL dans le paramètre.