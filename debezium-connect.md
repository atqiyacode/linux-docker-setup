curl -X POST -H "Content-Type: application/json" --data '{
  "name": "mysql-connector",
  "config": {
    "connector.class": "io.debezium.connector.mysql.MySqlConnector",
    "database.hostname": "192.168.0.116",
    "database.port": "3306",
    "database.user": "root",
    "database.password": "Bsdcity2024",
    "database.server.id": "184054",
    "database.server.name": "dbserver1",
    "table.include.list": "multi_app.users",
    "database.history.kafka.bootstrap.servers": "kafka:9092",
    "database.history.kafka.topic": "dbhistory.multi_app",
    "topic.prefix": "dbserver1"
  }
}' http://localhost:8083/connectors
