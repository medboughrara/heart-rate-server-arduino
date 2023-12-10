.#include <SoftwareSerial.h>
#include <WiFi.h>
#include <SPI.h>
#define USE_ARDUINO_INTERRUPTS true    // Set-up low-level interrupts for most acurate BPM math
#include <PulseSensorPlayground.h>     // Includes the PulseSensorPlayground Library

const int PulseWire = 0;       // 'S' Signal pin connected to A0
int Threshold = 550;           // Determine which Signal to "count as a beat" and which to ignore
                               
PulseSensorPlayground pulseSensor;  // Creates an object


const char WEBSITE[] = "api.pushingbox.com"; //pushingbox API server
const String devid = "v2DF8E5DAEB0D87E"; //device ID on Pushingbox for our Scenario

const char* MY_SSID = "Assaflrr";
const char* MY_PWD =  "0542409999";

WiFiClient client;  

int status = WL_IDLE_STATUS;

void setup() {
  //Initialize serial and wait for port to open:
  Serial.begin(9600);
  while (!Serial) 
  {
    ; // wait for serial port to connect. Needed for native USB port only
  }

  // Configure the PulseSensor object, by assigning our variables to it
  pulseSensor.analogInput(PulseWire);   
  pulseSensor.setThreshold(Threshold);   

  // Double-check the "pulseSensor" object was created and began seeing a signal
  if (pulseSensor.begin()) {
    Serial.println("PulseSensor object created!");
  }

  // check for the presence of the shield:
  if (WiFi.status() == WL_NO_SHIELD) {
    Serial.println("WiFi shield not present");
    // don't continue:
    while (true);
  }

  // attempt to connect to Wifi network:
  while (status != WL_CONNECTED) 
  {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(MY_SSID);
    //Connect to WPA/WPA2 network.Change this line if using open/WEP network
    status = WiFi.begin(MY_SSID, MY_PWD);

    // wait 10 seconds for connection:
    delay(10000);
  }
  
  Serial.println("Connected to wifi");
  printWifiStatus();
  
}

void loop() {
  int myBPM = pulseSensor.getBeatsPerMinute();      // Calculates BPM

  if (pulseSensor.sawStartOfBeat()) {               // Constantly test to see if a beat happened
    Serial.println("♥  A HeartBeat Happened ! "); // If true, print a message
    Serial.print("BPM: ");
    Serial.println(myBPM);                        // Print the BPM value
    }

  delay(20);
client.print("GET /pushingbox?devid=" + devid + "&myBPM=" + (String) myBPM);

Serial.print("Heart Rate: ");
Serial.print(myBPM);
Serial.println(" bpm");

Serial.println("\nSending Data to Server..."); 
  // if you get a connection, report back via serial:

    //API service using WiFi Client through PushingBox then relayed to Google
    if (client.connect(WEBSITE, 80))
      { 
        Serial.println("\nserver");
         client.print("GET /pushingbox?devid=" + devid
       + "&myBPM=" + (String) myBPM
         );

      // HTTP 1.1 provides a persistent connection, allowing batched requests
      // or pipelined to an output buffer
      client.println(" HTTP/1.1"); 
      client.print("Host: ");
      client.println(WEBSITE);
      client.println();
      Serial.println("\nData Sent"); 
      }
}


void printWifiStatus() {
  // print the SSID of the network you're attached to:
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());

  // print your WiFi shield's IP address:
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);

  // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("signal strength (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
}
