# Démo MCF

## Démarrage
Démarrer le conteneur Elasticsearch en arrière plan ```docker-compose up -d elasticsearch```
Démarrer le conteneur Kibana en arrière plan ```docker-compose up -d kibana```
Se connecter à Kibana depuis son navigateur : ```localhost:5601``` Identifiant par défaut ```elastic/changeme```

## Ingérer un fichier de log

* Depuis l'écran d'accueil cliquez sur le bouton Add Data et de là allez sur l'onglet Upload file
* Choisir un fichier de log à importer, il ne doit pas dépasser 100Mo
* Pour éviter les messages d'erreur on retire les lignes d'en-tête avec ```sed -i '/^#/ d' *.log```
* Kibana analyse le fichier et propose de lui appliquer la structure qui lui semble la plus appropriée
* On modifie cette proposition pour lui appliquer notre template:

```
%{TIMESTAMP_ISO8601:timestamp} %{IPORHOST:site} %{WORD:method} %{URIPATH:uristem} %{NOTSPACE:uriquery} %{NUMBER:port} %{NOTSPACE:username} %{IPORHOST:clienthost} %{NOTSPACE:useragent} %{NOTSPACE:referer} %{NUMBER:status} %{NUMBER:substatus} %{NUMBER:win32status} %{NUMBER:time_taken}

%{TIMESTAMP_ISO8601:timestamp} %{IPORHOST:site} %{WORD:method} %{URIPATH:uristem} %{NOTSPACE:uriquery} %{NUMBER:port} %{NOTSPACE:username} %{IPORHOST:clienthost} %{NOTSPACE:protocol} %{NOTSPACE:useragent} %{NOTSPACE:referer} %{NUMBER:status} %{NUMBER:substatus} %{NUMBER:win32status} %{NUMBER:time_taken}
```

## Visualiser les log avec un dashboard

* Depuis le site d'Elastic https://demo.elastic.co/app/management/kibana/objects, on exporte les Dashboards de démo dans un json.
* Depuis le menu Management, choisr Stack Management et importer le fichier json précédent. Au passage on modifie l'```index pattern```
* Modifier l'index utiliser par les objets importés
