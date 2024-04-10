# Pub/Sub

**Reliable, scalable, fully-managed** asynchronous messaging service  
Backbone for highly available & highly scalable solutions  
  * Auto scale to process billions of messages per day
  * low cost (pay for use)  

**Use case**: event ingestion & event streaming analytics pipeline  
Supports push & pull message deliveries  

![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/0d95b47b-8a6c-426d-929b-40623bc08217)  

**Publisher** (sender of message) - send mesasge by making HTTPS requests to pusub.googleapis.com  
**Subscriber** (receiver of message)
  * **Pull** (subscriber pulls message when ready) - subscriber makes HTTPS requests to pubsub.googleapis.com
  * **Push** (Messages are sent to subscribers) - subscriber provide a web hook endpoint at the time of registration
    * when a message is received on the topic, a HTTPS POST request is sent to web hook endpoint

Very flexible publisher & subscriber relationships: many to one, one to many, many to many  

## Pub/Sub - Topic & Subscriptions
![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/fafe692d-3e8b-4766-83a2-a0e20aeae975)   

Step 1: Topic is created  
Step 2: subscriptions are created  
  * subscribers registers for topics
  * each subscription represents discrete pull of messages from a topic
    * multiple clients pull same subscription => messages split between clients
    * multiple clients create a subscription each => each client will get every message


## Pub/Sub - Sending & Receiving messages
![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/ee390183-dece-45c8-88b0-b4c210f82184)   

Publisher sends a message to topic  
Message is individually delivered to each & every subscription  
  * subscribers can receive message either by
    * **push**: Pub/Sub sends message to subscriber
    * **pull**: subscribers poll for messages

Subscribers send acknowledgements  
Messages are removed from subscription message queue  
  * Pub/Sub ensures the message is retained per subscription until it is acknowledged  


### To create a topic
```
gcloud config set project <project>
gcloud pubsub topics create <topic>
```
### To create a subscription
```
gcloud pubsub subscriptions create <subscription-1> --topic=<topic>
gcloud pubsub subscriptions create <subscription-2> --topic=<topic>
OPTIONS:
  --enable-message_ordering - to maintain order of msgs in which they arrive
  --ack-deadline - how long you have to wait for ack
  --message-filter - to specify message filters
```
### publishing messages to topics
```
gcloud pubsub topics publish topic-from-gcloud --message="My Message 1"
gcloud pubsub topics publish topic-from-gcloud --message="My Message 2"
gcloud pubsub topics publish topic-from-gcloud --message="My Message 3"
```
### Pulling messages from subscription without ack
```
gcloud pubsub subscriptions pull <subscription-2>
gcloud pubsub subscriptions pull <subscription-1>
```
### Pulling messages from subscription with ack
```
gcloud pubsub subscriptions pull subscription-1 --auto-ack
gcloud pubsub subscriptions pull subscription-2 --auto-ack
OPTIONS:
  --limit - to limit number of messages
```
### List all active topics
```
gcloud pubsub topics list
```
### List subscription in topics
```
gcloud pubsub topics list-subscriptions my-first-topic
```
### To delete the topic
```
gcloud pubsub topics delete <topic>
```
