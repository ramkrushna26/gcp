## Synchronous Communication
![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/603fba64-3f7a-4004-8a55-47bb3ca89317)  

Applications on your web server make synchronous calls to the logging service  
If logging service goes down, will your application go down as well?  
Log server goes down very often if there is high load & lot of logs coming in  

</br>

## Asynchronous Communication
![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/7b0a48fb-d1ef-41a9-a72d-56fcbaea3a98)  

Create a topic & have your applications put log messages on the topic  
Logging service pick them up for procecessing when ready  
Advantages:
  * **Decoupling**: Publisher (apps) don't care about who is listening
  * **Availability**: Publisher (apps) up even when subscriber (logging service) goes down
  * **Scalability**: Scale consumer instances (logging service) under high load
  * **Durability**: Message is not lost even if subscriber (loggin service) goes down

