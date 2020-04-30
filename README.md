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
