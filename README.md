# B2B Transporter [![GitHub issues](https://img.shields.io/github/release/conuti-das/b2btransporter.svg)](https://github.com/conuti-das/b2btransporter/blob/master/VERSION.md)  [![Website](https://img.shields.io/website-up-down-green-red/http/shields.io.svg?label=Download)](https://github.com/conuti-das/b2btransporter/blob/master/DOWNLOAD.md)




Informationen zum Transport System für die B2B by Practice entwickelt von [ConUti](http://conuti.de) 

## Allgemeines 

Der Transport ist eine Webapplikation, die es ermöglicht ausgewähltest B2BbP Customizing zwischen B2BbP Landschaften zu transportieren. Die Landschaften werden 

![](https://github.com/conuti-das/b2btransporter/blob/master/CBTS1.png)
![](https://github.com/conuti-das/b2btransporter/blob/master/CBTS2.png)

Sowohl Slave, als auch Master sind eine identische eigenständige Webapplikation, die auf dem Play Framework 1.4.x basiert und als Exploded WAR ausgeliefert wird. Der Transporter persistiert mit den JPA und HIBERNATE Funktionen, die das Framework liefert. Damit ist die Datenbank die Schnittstelle zur B2BbP, die durch das B2B Datenbankschema vorgegeben ist. Änderungen am Schema sind mit minimalem Aufwand umsetzbar. Diese Architektur ermöglicht ein einfaches, B2B unabhängiges Deployment der Anwendung

![](https://github.com/conuti-das/b2btransporter/blob/master/CBTS3.png)

## Übersicht Entwicklungsstand

Auf der Seite (https://github.com/conuti-das/b2btransporter/issues) werden aktuelle Bugs und Features erfasst. Diese werden bewertet und einem Meilenstein (entspricht neuem Release) zugeordnet. Die aktuelle Meilensteinplanung inklusive Releasedatum findet sich auf dieser Seite (https://github.com/conuti-das/b2btransporter/milestones). 
Zwischenreleases aufgrund von Bugs oder relevanten Änderungen in der B2B, werden bei Bedarf ergänzt.
Der Umfang eines Release wächst dabei entsprechend des verfügbaren Rahmens mit der Anzahl der Kunden, die einen Wartungsvertrag abgeschlossen haben.


[![GitHub issues](https://img.shields.io/github/issues/conuti-das/b2btransporter.svg)](https://github.com/conuti-das/b2btransporter/issues)[![GitHub issues](https://img.shields.io/github/issues-closed/conuti-das/b2btransporter.svg)](https://github.com/conuti-das/b2btransporter/issues)

[![Stories in Ready](https://badge.waffle.io/conuti-das/b2btransporter.svg?label=bug&title=bug)](https://huboard.com/conuti-das/b2btransporter#/) [![Stories in Ready](https://badge.waffle.io/conuti-das/b2btransporter.svg?label=enhancement&title=Enhancment)](https://huboard.com/conuti-das/b2btransporter#/)

## Aktuelle Version

Informationen zur aktuellen Version finden sich hier : [VERSION](https://github.com/conuti-das/b2btransporter/blob/master/VERSION.md)


## Download Transporter

Die aktuellen Version hier kann hier heruntergeladen werden : [DOWNLOAD](https://github.com/conuti-das/b2btransporter/blob/master/DOWNLOAD.md)

## Installation

### Datenbankverbindung einrichten

Tomcat stoppen. Kopieren der b2bbp-engine.xml im Verzeichnis Tomcat/conf/Catalina/localhost/ und speichern unter cbts.xml. Anpassung des Pfades in der Datei von b2bbp-engine auf cbts.Falls der ctbs Master in einer separaten Datenbank arbeiten soll, müssen beim CBTS Master hier entsprechend die Zugangsdaten zu dieser DB angegeben werden.

Beispiel cbts.xml : 

```
<Context path="/cbts" reloadable="true" crossContext="true">
   <Resources
        cachingAllowed="true"
        cacheMaxSize="100000"/>
   <Resource name="jdbc/b2bbp"
		auth="Container"
		type="javax.sql.DataSource"
		factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
		driverClassName="org.postgresql.Driver"
		url="jdbc:postgresql://localhost:5432/b2b"
		username="admin"
		password="***"					 
		maxActive="100" 
		maxIdle="10" 
		maxWait="-1"
		 />
</Context>
```
### Webarchiv deployen

Dateien in Tomcat webapps Verzeichnis kopieren. Die Auslieferung cbts.zip muss direkt ins Tomcat Webapps Verzeichnis kopiert und entpackt werden.

### Konfigurationsanpassungen in cbts.conf

Unter webapps/cbts/WEB-INF/application/conf befinden sich die Konfigurationsdatei cbts.conf. Hier wirde festgelegt ob eine Installation ein Master oder Slave ist. Außerdem muss ein SQL Dialect gepflegt werden, damit die Anwendung automatisch die SQL Skripte passend für die Datenbank erzeugt. Es ist beispielhaft eine Master und Slave Konfiguration mit Postgres DB enthalten. 

#### Die Slave Konfiguration
```
#
# SLAVE CONFIGURATION
# Datasource name from Tomcat/conf/Catalina/localhost/cbts.xml file 
db.default=java:/comp/env/jdbc/b2bbp
# Used SQL dialect
jpa.default.dialect=org.hibernate.dialect.PostgreSQLDialect
# Transporter modus master or slave
cbts.mode=slave
```

#### Die Master Konfiguration
```
# MASTER CONFIGURATION
# uncomment if this instance is acting as CBTS server (and comment SLAVE CONFIGURATION)
# Datasource name from Tomcat/conf/Catalina/localhost/cbts.xml file 
db.default=java:/comp/env/jdbc/b2bbp
# enable Sql debug information
jpa.default.debugSQL=true
# Used SQL dialect
jpa.default.dialect=org.hibernate.dialect.PostgreSQLDialect
# SQL UPDATE MODE ( SQL migration script will be executed automatically ) 
jpa.default.ddl=update
# Transporter modus master or slave
cbts.mode=master
```

#### SQL Dialekt 

In der cbts.conf muss je nach Datenbankversion der Dialect angepasst werden, damit die SQL Skripte automatisch funktionieren.

Für Postgres ist dies beispielsweise:
jpa.default.dialect=org.hibernate.dialect.PostgreSQLDialect

Andere Datenbanken :
```
PostgreSQL			org.hibernate.dialect.PostgreSQLDialect
MySQL5				org.hibernate.dialect.MySQL5Dialect
MySQL5 with InnoDB		org.hibernate.dialect.MySQL5InnoDBDialect
MySQL mit MyISAM		org.hibernate.dialect.MySQLMyISAMDialect
Oracle (alle Versionen)		org.hibernate.dialect.OracleDialect
Oracle 9i			org.hibernate.dialect.Oracle9iDialect
Oracle 10g			org.hibernate.dialect.Oracle10gDialect
Oracle 11g			org.hibernate.dialect.Oracle10gDialect
Microsoft SQL Server 2000	org.hibernate.dialect.SQLServerDialect
Microsoft SQL Server 2005	org.hibernate.dialect.SQLServer2005Dialect
Microsoft SQL Server 2008	org.hibernate.dialect.SQLServer2008Dialect
SAP DB				org.hibernate.dialect.SAPDBDialect
H2 Database			org.hibernate.dialect.H2Dialect
Progress			org.hibernate.dialect.ProgressDialect
```


Diese Datei im Tomcat Root/conf/cbts/cbts.conf ablegen (der Speicherort kann in der application.conf angepasst werden). Nach Speicherung der Konfiguration kann der Tomcat gestartet werden. 

Es empfiehlt sich diese Datei unter Tomcat/conf/cbts/ abzulegen, damit diese bei zukünftigen Updates nicht wieder angepasst werden muss.





### Installation ausführen

Im letzten Schritt werden Tabellen angelegt und mit Default Daten gefüllt. Dazu muss nach dem Starten des Tomcats die Adresse http://servername:port/cbts aufgerufen werden. Der Transporter erkennt den Master Modus operiert und wird dann durch die Installation führen.

Sollte die Anwendung nicht starten, findet sich im b2b-transporter.log File ein Hinweis auf das Problem. 

## Update

### Webarchiv deployen

Tomcat stoppen. Dateien in Tomcat webapps Verzeichnis kopieren. Die Auslieferung cbts.zip muss direkt ins Tomcat Webapps Verzeichnis kopiert und entpackt werden. Nach dem Starten des Tomcats ist die neue Version verfügbar. Änderungen an der Datenbank werden automatisch ausgeführt.

## Update auf Version 1.40 von Version 1.3x 

Für ein Update von Version 1.3x auf > 1.4. gilt folgendes : Vor dem Update sollten alle Tabellen gelöscht werden und der Installer erneut ausgeführt werden (siehe Installation)



