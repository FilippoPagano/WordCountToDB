installare le dipendenze
estrarre wordcount

IN UN NUOVO TERMINALE
title zookeeper
cd Desktop\kafka
bin\windows\zookeeper-server-start.bat config\zookeeper.properties

IN UN NUOVO TERMINALE
title kafka server broker 0
cd Desktop\kafka
bin\windows\kafka-server-start.bat config\server.properties

SOLO UNA VOLTA
bin\windows\kafka-topics.bat --create     --bootstrap-server localhost:9092     --replication-factor 1     --partitions 1     --topic streams-plaintext-input
bin\windows\kafka-topics.bat --create     --bootstrap-server localhost:9092     --replication-factor 1     --partitions 1     --topic streams-wordcount-output     --config cleanup.policy=compact

IN UN NUOVO TERMINALE
title wordcount
cd Desktop\wordCount
mvn clean package
mvn exec:java -Dexec.mainClass=myapps.WordCount

IN UN NUOVO TERMINALE
title consumer
cd Desktop\kafka
bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092     --topic streams-wordcount-output     --from-beginning     --property print.key=true     --property print.value=true  --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer

IN UN NUOVO TERMINALE
title producer
cd Desktop\kafka
bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic streams-plaintext-input
scrivere input
aspettare che il consumer produca output o che wordcount dia output
chiudere wordcount (ctrl+C)
si possono verificare le scritture nel database H2: ATTENZIONE: NON è POSSIBILE AVERE APERTURE SIMULTANEE DEL DATABASE 
IN QUESTA DEMO, PER UN CORRETTO FUNZIONAMENTO IL DATABASE PUò ESSERE APERTO 
O DALL'APPLICATIVO O DAL BROWSER MA MAI DA ENTRAMBI CONTEMPORANEAMENTE
per aprire il DB aprire H2 Console
JDBC Url: jdbc:h2:~/wordCount
(il file sul vostro pc è C:\Users\<USERNAME>\wordCount.h2.db)
Nome utente: sa
password: nessuna password, lasciare il campo vuoto
