TME 10-11 : Intégration IoT via MQTT et Kafka

Le but de la seconde partie du projet, est de mettre en place un support de communication permettant à un objet connecté (via MQTT) de mettre à jour le stock d’un produit, et que cette info remonte jusqu’à Strapi, grâce à Kafka.
Prérequis

    Mettre en place un scripte dans /mosquitto/mosquitto.conf

persistence true
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log
allow_anonymous true
listener 1883
protocol websockets

    Configurer le docker compose en ajoutant le lancement du scripte mosquitto et de la connexion avec kafka

Architecture mise en place

Frontend (MQTT over WebSocket)
   ↓
Mosquitto (broker MQTT)
   ↓
Connecteur MQTT → Kafka (arthurescriou/mqtt-kafka-connector)
   ↓
Kafka (topic: stock)
   ↓
Consumer Kafka
   ↓
Strapi (mise à jour du stock produit)

Déroulé de la chaîne

    Le navigateur publie un message MQTT vers Mosquitto, via WebSocket (port 1883) (site:https://mqtt-test-front.onrender.com/ ).
    Le connecteur mqtt-kafka-connector écoute le topic MQTT "topic" et publie le message dans le topic Kafka "stock".
    Un consumer Kafka lit le message et effectue une requête vers le Strapi pour mettre à jour le stock du produit correspondant.
    Le changement de stock est alors visible dans le dashboard Strapi et dans le Frontend.

Logs

    log_mosquitto_connexion : Logs des réceptions des messages et la transmission des messages à kafka.
    log_stock_consumer_mqtt : Logs du consumer pour les mouvements de stock.
