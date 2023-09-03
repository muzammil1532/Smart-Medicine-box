#include <Arduino.h>
#ifdef ESP32
  #include <WiFi.h>
  #include <AsyncTCP.h>
#else
  #include <ESP8266WiFi.h>
  #include <ESPAsyncTCP.h>
#endif
#include <ESPAsyncWebServer.h>
#include <HTTPClient.h>
#include <SPI.h>
#include <NTPClient.h>
#include <WiFiUdp.h>
#define NTP_OFFSET  19800  
#define NTP_INTERVAL 60 * 1000  
#define NTP_ADDRESS  "1.asia.pool.ntp.org"

WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, NTP_ADDRESS, NTP_OFFSET, NTP_INTERVAL);

AsyncWebServer server(80);
long duration1; // variable for the duration of sound wave travel
float distance1; // variable for the distance measurement
long duration2; // variable for the duration of sound wave travel
float distance2; // variable for the distance measurement
long duration3; // variable for the duration of sound wave travel
float distance3; // variable for the distance measurement
int trigPin1=12;
int echoPin1=13;
int trigPin2=16;
int echoPin2=17;
int trigPin3=22;
int echoPin3=21;
int a=0;
int b=0;
int c=0;
const char* ssid = "Mi 10i";
const char* password = "satwik01";

const char* PARAM_INPUT_1 = "input1";
const char* PARAM_INPUT_2 = "input2";
const char* PARAM_INPUT_3 = "input3";
const char* PARAM_INPUT_4 = "input4";
const char* PARAM_INPUT_5 = "input5";
const char* PARAM_INPUT_6 = "input6";
const char* PARAM_INPUT_7 = "input7";
const char* PARAM_INPUT_8 = "input8";
const char* PARAM_INPUT_9 = "input9";
String inputMessage1;
String inputMessage;
String inputMessage2;
String inputMessage3;
String inputMessage4;
String inputMessage5;
String inputMessage6;
String inputMessage7;
String inputMessage8;
String inputMessage9;
String inputParam;
const char index_html[] PROGMEM = R"rawliteral(
<!DOCTYPE HTML><html><head>
  <title>ESP Input Form</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  </head><body> <center>
 <h1>Enter Tablets timmings in increasing fashion for morning</h1>
  <form action="/get">
    input1: <input type="text" name="input1">
   
  <br>
 
    input2: <input type="text" name="input2">
   
  <br>
 
    input3: <input type="text" name="input3">
   
 
  <br>
  <h1>Enter Tablets timmings in increasing fashion for Afternoon</h1>
 
    input4: <input type="text" name="input4">
 
 
  <br>
 
    input5: <input type="text" name="input5">
   
 
  <br>
 
    input6: <input type="text" name="input6">
   
 
  <br>
  <h1>Enter Tablets timmings in increasing fashion for evening</h1>
    input7: <input type="text" name="input7">
   
 
  <br>
 
    input8: <input type="text" name="input8">
   
 
  <br>
 
    input9: <input type="text" name="input9">
   
<br>
<br>
<br>
 <input type="submit" value="Submit"></center>
    </form>
</body></html>)rawliteral";

void notFound(AsyncWebServerRequest *request) {
  request->send(404, "text/plain", "Not found");
}
String apiKey = "824500";              
String phone_number = "+918074507855";

String url;    
void setup() {
  pinMode(trigPin1, OUTPUT); // Sets the trigPin as an OUTPUT
  pinMode(echoPin1, INPUT); // Sets the echoPin as an INPUT
  pinMode(trigPin2, OUTPUT); // Sets the trigPin as an OUTPUT
  pinMode(echoPin2, INPUT); // Sets the echoPin as an INPUT
  pinMode(trigPin3, OUTPUT); // Sets the trigPin as an OUTPUT
  pinMode(echoPin3, INPUT); // Sets the echoPin as an INPUT
  Serial.begin(9600);
 // Serial.begin(9600);
  pinMode(2,OUTPUT);
  pinMode(18,OUTPUT);
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  if (WiFi.waitForConnectResult() != WL_CONNECTED) {
    Serial.println("WiFi Failed!");
    return;
  }
  Serial.println("IP Address: ");
  Serial.println(WiFi.localIP());
 
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send_P(200, "text/html", index_html);
  });

  server.on("/get", HTTP_GET, [] (AsyncWebServerRequest *request) {
   
    if (request->hasParam(PARAM_INPUT_1)) {
      inputMessage1 = request->getParam(PARAM_INPUT_1)->value();
      inputParam = PARAM_INPUT_1;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage1 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if (request->hasParam(PARAM_INPUT_2)) {
      inputMessage2 = request->getParam(PARAM_INPUT_2)->value();
      inputParam = PARAM_INPUT_2;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage2 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if (request->hasParam(PARAM_INPUT_3)) {
      inputMessage3 = request->getParam(PARAM_INPUT_3)->value();
      inputParam = PARAM_INPUT_3;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage3 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if (request->hasParam(PARAM_INPUT_4)) {
      inputMessage4 = request->getParam(PARAM_INPUT_4)->value();
      inputParam = PARAM_INPUT_4;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage4 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if (request->hasParam(PARAM_INPUT_5)) {
      inputMessage5 = request->getParam(PARAM_INPUT_5)->value();
      inputParam = PARAM_INPUT_5;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage5 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if (request->hasParam(PARAM_INPUT_6)) {
      inputMessage6 = request->getParam(PARAM_INPUT_6)->value();
      inputParam = PARAM_INPUT_6;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage6 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if (request->hasParam(PARAM_INPUT_7)) {
      inputMessage7 = request->getParam(PARAM_INPUT_7)->value();
      inputParam = PARAM_INPUT_7;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage7 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if (request->hasParam(PARAM_INPUT_8)) {
      inputMessage8 = request->getParam(PARAM_INPUT_8)->value();
      inputParam = PARAM_INPUT_8;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage8 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
    if(request->hasParam(PARAM_INPUT_9)) {
      inputMessage9 = request->getParam(PARAM_INPUT_9)->value();
      inputParam = PARAM_INPUT_9;
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage9 +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }
   /* else {
      inputMessage = "No message sent";
      inputParam = "none";
       request->send(200, "text/html", "HTTP GET request sent to your ESP on input field ("
                                     + inputParam + ") with value: " + inputMessage +
                                     "<br><a href=\"/\">Return to Home Page</a>");
    }*/
   
   
  });
  server.onNotFound(notFound);
  server.begin();
  timeClient.begin();
}
String urlencode(String str)  
{
    String encodedString="";
    char c;
    char code0;
    char code1;
    char code2;
    for (int i =0; i < str.length(); i++){
      c=str.charAt(i);
      if (c == ' '){
        encodedString+= '+';
      } else if (isalnum(c)){
        encodedString+=c;
      } else{
        code1=(c & 0xf)+'0';
        if ((c & 0xf) >9){
            code1=(c & 0xf) - 10 + 'A';
        }
        c=(c>>4)&0xf;
        code0=c+'0';
        if (c > 9){
            code0=c - 10 + 'A';
        }
        code2='\0';
        encodedString+='%';
        encodedString+=code0;
        encodedString+=code1;
      }
      yield();
    }
    return encodedString;
}

void loop() {
  timeClient.update();
String formattedTime = timeClient.getFormattedTime();
Serial.println(formattedTime);
//delay(1000);
if(formattedTime==inputMessage1||formattedTime==inputMessage2||formattedTime==inputMessage3)
{int x;
  if(formattedTime==inputMessage1){
    x=1;
  message_to_whatsapp("Reminder of tablet to be taken from box 1.");
  }
  else if(formattedTime==inputMessage2){
    x=2;
    message_to_whatsapp("Reminder of tablet to be taken from box 2.");
  }
  else{
    x=3;
    message_to_whatsapp("Reminder of tablet to be taken from box 3.");
    }
   int p=0;
  while(a!=1|a!=2|a!=3){
    if(p==10){
      break;}
      p=p+1;
    if(a==x)
    break;
  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  // Sets the trigPin HIGH (ACTIVE) for 10 microseconds
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin2, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration2 = pulseIn(echoPin2, HIGH);
  // Calculating the distance
  distance1 = duration2*0.034/2;
  a=-1;
 
  if(distance1<8.9)
  {
    a=1;
    }
  else if(distance1<=17.8)
  {
    a=2;
    }
  else if(distance1<20)
  {
    a=3;
    }
   
    Serial.print(a);
       digitalWrite(18,HIGH);
       delay(500);
       digitalWrite(18,LOW);
       delay(500);
 
  }
digitalWrite(18,LOW);
  if(a==-1)
  message_to_whatsapp("You missed taking the tablet");
  else
    message_to_whatsapp("You took the medicine");

 /* Serial.print(a);
  delay(500);// Speed of sound wave divided by 2 (go and back)
  // Displays the distance on the Serial Monitor
  Serial.print("Distance 1: ");
  Serial.print(distance1);
  Serial.println(" cm");
  digitalWrite(18,HIGH);
  for(int i=0;i<10;i++){
    digitalWrite(2,HIGH);
  delay(500);
  digitalWrite(2,LOW);
  }*/
}
if(formattedTime==inputMessage4||formattedTime==inputMessage5||formattedTime==inputMessage6)
{int y;
if(formattedTime==inputMessage4){
  y=1;
  message_to_whatsapp("Reminder of tablet 4.");
}
else if(formattedTime==inputMessage5){
  y=2;
  message_to_whatsapp("Reminder of tablet 5.");
}
else{
  y=3;
  message_to_whatsapp("Reminder of tablet 6.");
}
int p1=0;
while(c!=1|c!=2|c!=3){
  if(p1==10){
    break;}
    p1++;
    digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  // Sets the trigPin HIGH (ACTIVE) for 10 microseconds
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration1 = pulseIn(echoPin1, HIGH);
  // Calculating the distance
  distance2 = duration1*0.034/2;
  c=-1;
  if(distance2<=6)
  {
    c=1;
    }
  else if(distance2<=14.5)
  {
    c=2;
    }
  else if(distance2<=19)
  {
    c=3;
    }
 Serial.print(c);
  delay(500);// Speed of sound wave divided by 2 (go and back)
  // Displays the distance on the Serial Monitor
  Serial.print("Distance 2: ");
  Serial.print(distance2);
  Serial.println(" cm");
  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  if(c==y){
    break;
    }
    digitalWrite(18,HIGH);
    delay(500);
    digitalWrite(18,LOW);
    delay(500);
}
  digitalWrite(18,LOW);
  if(c==-1)
  message_to_whatsapp("You missed taking the tablet");
  else
    message_to_whatsapp("You took the medicine");

}
if(formattedTime==inputMessage7||formattedTime==inputMessage8||formattedTime==inputMessage9)
{int z;
if(formattedTime==inputMessage7){
  z=1;
  message_to_whatsapp("Reminder of tablet 7.");
}
else if(formattedTime==inputMessage8){
  z=2;
  message_to_whatsapp("Reminder of tablet 8.");
}
else{
  z=3;
  message_to_whatsapp("Reminder of tablet 9.");
}
int p2=0;
while(b!=1||b!=2||b!=3){
  if(p2==10){
    break;}
    p2=p2+1;
  if(b==z){
    break;
    }
digitalWrite(trigPin3, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin3, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration3 = pulseIn(echoPin3, HIGH);
  // Calculating the distance
  distance3 = duration3*0.034/2;
    b=-1;
  if(distance3<5)
  {
    b=1;
    }
  else if(distance3<=15.5)
  {
    b=2;
    }
  else if(distance3<17)
  {
    b=3;
    }
 
    Serial.print(b);
    delay(500);// Speed of sound wave divided by 2 (go and back)
  // Displays the distance on the Serial Monitor
  Serial.print("Distance 3: ");
  Serial.print(distance3);
  Serial.println(" cm");
   digitalWrite(18,HIGH);
    delay(500);
    digitalWrite(18,LOW);
    delay(500);
}
digitalWrite(18,LOW);
if(b==-1)
message_to_whatsapp("You missed taking the tablet");
else
message_to_whatsapp("You took the medicine");

}
}

void  message_to_whatsapp(String message)      
{
  url = "https://api.callmebot.com/whatsapp.php?phone=" + phone_number + "&apikey=" + apiKey + "&text=" + urlencode(message);

  postData();
}

void postData()    
{
  int httpCode;    
  HTTPClient http;  
  http.begin(url);  
  httpCode = http.POST(url);
  if (httpCode == 200)      
  {
    Serial.println("Sent ok.");
  }
  else                      
  {
    Serial.println("Error.");
  }
  http.end();          

}