# MicroserviceBasedPetClinic
In diesem Repository entsteht eine Art Development Diary für eine Microservice-basierte Version der bekannten Spring-Pet Clinic 
Applikation. In diesem Repository selbst werden die Fortschritte des Projektes dokumentiert, die Microservices selber werden in 
separaten Repositories entwickelt. In diesem Projekt liegt der Fokus auf der Implementierung einer REST-Api zur Verwaltung der Pet Clinic 
Fachobjekte, also eine API an die ein entsprechendes Frontend einer Pet Clinic Applikation angebunden werden kann. Dieses Dokument, bzw. 
dieses Repository, soll die Entstehung der API dokumentieren. Im Sinne des Prinzips dass man am besten lernt was man erklärt sollen auch 
gemachte Erfahrungen in separaten Einträgen im Wiki des Repositories dokumentiert werden. Dabei erheben diese Einträge keinen Anspruch 
auf "Korrektheit", sondern schildern die Erfahrungen aus meiner Sicht. Ich werde mir aber Mühe geben wo möglich und angebracht Quellen 
einzubinden und meine Ansichten zu begründen. 

Damit soll auch der Einleitung schon genug sein. Es folgt direkt der erste Eintrag.

29.04.2020: Definition der ersten Version der API

Die erste Version dieser an Spring-Pet Clinic angelehnten API wird eine REST-API zur Verwaltung von Pets, Owner, Doctors und Appointments 
zur Verfügung stellen. Die API hat die Aufgabe, die Verwaltung der Fachobjekte zu ermöglichen. Mit Verwaltung sind hier im ersten Schritt 
die Standard-CRUD Operationen gemeint.

Hierbei wird eine Microservice-basierte Architektur verwendet, wobei der Verwaltung von Doctors und Appointments jeweils ein Service 
zugeordnet ist. Pets und Owners werden in einem Service verwaltet da diese beiden Fachobjekte logisch zusammen gehören. Zum Beispiel 
macht es bei einer create-Operation eines Owners keinen Sinn diese von der create-Operation eines Pets zu trennen da ein Owner ohne Pet 
fachlich gesehen genauso wenig Sinn macht wie ein Pet ohne Owner.

Die erste Version der API soll außerdem folgende nicht-fachliche Anforderungen erfüllen:

1. Es soll zusammen mit der API eine Swagger-Dokumentation veröffentlicht werden
2. Die API soll gesichert mit Kubernetes veröffentlicht werden

30.04.2020: Erste Coding-Arbeiten

1. Shared Model für die Services implementiert, verfügbar unter: https://github.com/AndreasMuellerAtStuttgart/PetClinicSharedModel
Shared Model wurde implementiert um Code-Duplikation zwischen Services zu vermeiden. 

2. Shared Model refactored um Ids zur Identifikation der Fachentitäten zu benutzen: Hier habe ich mir auch im Lichte dieses Artikels: https://enterprisecraftsmanship.com/posts/dont-use-ids-domain-entities/ Gedanken gemacht ob ich in den Objekten die nachher über die REST-API an den Client gehen, also im Shared Model, tatsächlich ids brauche, aber ich bin zu dem Schluss gekommen dass dem so ist. Die Fachobjekte bieten keinen eindeutigen Business Key an, und wenn ich zum Beispiel in der Datenbank für die Appointments ein Appointment speichern will brauche ich einen Weg um die Fachentitäten die an dem Appointment teilnehmen eindeutig zu identifizieren. Deshalb brauche ich eine id weil die Identifikation über Kombinationen der anderen Felder nicht eindeutig möglich ist.

3. Erste Überlegungen zum Thema Datenhaltung: Bisher wollte ich die Services ihre eigenen Daten in separaten Datenbanken speichern lassen, dies führt aber zu einem großen Problem beim löschen von Daten. Wenn zum Beispiel ein Doctor gelöscht wird sollen seine Appointments ja mit gelöscht werden, aber das würde bedeuten dass der Doctor-Service beim löschen zur Bewahrung der Datenintegrität vom Appointment-Service abhängen würde, da er diesen über Service-Call zur Löschung aller entsprechenden Appointments aufrufen müsste. Ich bin zu dem Schluss gekommen dass es sicherer und effizienter ist die Datenintegrität an der Datenbank zu gewährleisten, was bedeutet dass eine zentrale Datenbank zur Datenhaltung benutzt wird. Deshalb werde ich vor der Weiterentwicklung der Services einen Persistence Layer implementieren der Service-übergreifend die Persistence Operationen bereitstellt. Die Services stellen dann nur noch die REST-Schnittstelle zu dem Persistence Layer dar. 

4. Implementierung des Persistence Layers: Um auch die Kommunikation zwischen Services zu üben und weil sich dann die Backend-Anbindung unabhängig von den Services die Fachlogik implementieren skalieren lässt wird der Backend-Layer als Backend-Service implementiert.

04.05.2020: Erste Arbeiten am Backend Service abgeschlossen, momentaner Stand ist dass an dem Service-Layer zu Pet-Persistierung MapStruct eingeführt werden muss.

05.05.2020: Heute habe ich in einem Kurs über Microservices gelernt dass es aus security-Gründen schlechte Praxis ist die Datenbank-Id in HTTP-Requests zu verwenden weil man damit feindlichen Benutzern einen Hinweis darauf gibt, wie die Datenbank-Ids aufgebaut sind und er damit malicious requests zum löschen von vielen Usern absetzen könnte. Deshalb benutzt man anscheinend 2 Ids, eine randomly generated Id die in HTTP-Requests benutzt wird und intern dann eine Datenbank-Id die automatisch von der Datenbank generiert wird. Dieses Pattern umzusetzen nehme ich in die projektweiten TO DOs auf, die im wiki dieses repositories geführt werden. 

08.05.2020: Auf internem Service-Layer im PersistenceService Service zum Handling von CRUD-Operationen für Doctor-Objekte implementiert 
und getestet, muss eventuell nochmal auf mögliche Fehlerfälle refactored und Tests erweitert werden.
