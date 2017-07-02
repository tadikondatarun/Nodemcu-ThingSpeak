# Nodemcu-ThingSpeak
Logging the data of temperature and humidity into ThingSpeak cloud from Nodemcu
## Nodemcu
It based on ESP8266, integates GPIO, PWM, IIC, 1-Wire and ADC all in one board. Power your developement in the fastest way combinating with NodeMcu Firmware.It is Open-source, Interactive, Programmable, Low cost, Simple, Smart, WI-FI enabled.


![esp8266-node-mcu-pinout](https://user-images.githubusercontent.com/25893079/27630293-aaf27dce-5c12-11e7-8082-447fa4b8f6d8.png)
## ThingSpeak
ThingSpeak is an IoT analytics platform service that allows you to aggregate, visualize and analyze live data streams in the cloud. ThingSpeak provides instant visualizations of data posted by your devices to ThingSpeak. With the ability to execute MATLAB® code in ThingSpeak you can perform online analysis and processing of the data as it comes in.
## Connecting Nodemcu to ThingSpeak
https://thingspeak.com/
Login into ThingSpeak and create a channel in it.
![screenshot 1](https://user-images.githubusercontent.com/25893079/27622964-d0a75af0-5bf7-11e7-89f2-af9f87bcad2c.png)
After creating a channel go to the API key and note down the API code generated.

![screenshot 2](https://user-images.githubusercontent.com/25893079/27622987-e7e661b6-5bf7-11e7-81c7-0fca7d1541d1.png)
Open the Arduino IDE and copy down the code.
#include <dht.h> //  DHT.h library

#include <ESP8266WiFi.h> // ESP8266WiFi.h library


int pin=13;// connect the dht11 sensor to D7 pin on nodemcu

dht DHT;

const char* ssid     = "ssid";

const char* password = "password";

const char* host = "api.thingspeak.com";

const char* writeAPIKey = "api key";



void setup()
{
 

//  Connect to WiFi network

  WiFi.begin(ssid, password);

while (WiFi.status() != WL_CONNECTED) {

delay(500);

}

}

void loop() {

DHT.read11(pin);

int humidity=DHT.humidity;

int temperature=DHT.temperature;

  if (isnan(humidity) || isnan(temperature)) {

return;

}

// make TCP connections

WiFiClient client;

const int httpPort = 80;

if (!client.connect(host, httpPort)) {

return;

}

  String url = "/update?key=";

url+=writeAPIKey;

url+="&field1=";

url+=String(temperature);

url+="&field2=";

url+=String(humidity);

url+="\r\n";
  
  // Request to the server

client.print(String("GET ") + url + " HTTP/1.1\r\n" +

"Host: " + host + "\r\n" + 

"Connection: close\r\n\r\n");

delay(1000);

}

Connect the DHT11 sensor to Nodemcu. connect 

![screenshot 3](https://user-images.githubusercontent.com/25893079/27622997-f24d475a-5bf7-11e7-8faf-f68be842cc75.png)

