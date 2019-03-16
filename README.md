# IoT-based-smart-door-lock
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <DNSServer.h>
#include <ESP8266WebServer.h>
#include <WiFiManager.h> 
#include <EEPROM.h>
#include <Servo.h>

Servo myservo;
char auth[] = "d475dd9903e94fa3840b55f2e883af94";

void setup()
{
  EEPROM.begin(512);
  Serial.begin(9600);
  myservo.attach(4);
  myservo.write(180);
  WiFiManager wifiManager;
   wifiManager.autoConnect("fairy_link");
  Serial.println("connected...:)");
  pinMode(LED_BUILTIN, OUTPUT);     
  Blynk.begin(auth, WiFi.SSID().c_str(), WiFi.psk().c_str());
}

BLYNK_WRITE(V2)
{
  int pinValue = param.asInt(); 
  Serial.print("V1 Slider value is: ");
  Serial.println(pinValue);
  Blynk.virtualWrite(V0, "Door State");
  if(pinValue)
  {	 Blynk.virtualWrite(V1, "Open");
    	servo(); }
else 
  {	 Blynk.virtualWrite(V1, "Closed");
    	myservo.write(180); } }
void loop()
{	Blynk.run();  }

void servo()
{	myservo.write(90); }
