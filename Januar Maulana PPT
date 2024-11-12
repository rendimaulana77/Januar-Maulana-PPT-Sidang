#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "Pasti Dadi";
const char* password = "bejo0808";

const int triggerPin1 = 13;
const int echoPin1 = 12;
const int ultrasonicSensorId1 = 1;

const int triggerPin2 = 27;
const int echoPin2 = 26;
const int ultrasonicSensorId2 = 2;

const int triggerPin3 = 33;
const int echoPin3 = 32;
const int ultrasonicSensorId3 = 3;


const char* serverUrl = "http://192.168.145.154/parkir/kirimdata.php";

void setup() {
  Serial.begin(115200);
  pinMode(triggerPin1, OUTPUT);
  pinMode(echoPin1, INPUT);
  pinMode(triggerPin2, OUTPUT);
  pinMode(echoPin2, INPUT);
  pinMode(triggerPin3, OUTPUT);
  pinMode(echoPin3, INPUT);
  
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");
}

void loop() {
  long duration1, distance1;
  long duration2, distance2;
  long duration3, distance3;

  // Measure distance for sensor 1
  digitalWrite(triggerPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(triggerPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distance1 = duration1 * 0.034 / 2;

  // Measure distance for sensor 2
  digitalWrite(triggerPin2, LOW);
  delayMicroseconds(2);
  digitalWrite(triggerPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin2, LOW);
  duration2 = pulseIn(echoPin2, HIGH);
  distance2 = duration2 * 0.034 / 2;

  // Measure distance for sensor 3
  digitalWrite(triggerPin3, LOW);
  delayMicroseconds(2);
  digitalWrite(triggerPin3, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin3, LOW);
  duration3 = pulseIn(echoPin3, HIGH);
  distance3 = duration3 * 0.034 / 2;

  // Serial.print("Distance Sensor 1: ");
  // Serial.println(distance1);
  // Serial.print("Distance Sensor 2: ");
  // Serial.println(distance2);
  // Serial.print("Distance Sensor 3: ");
  // Serial.println(distance3);

  int ultrasonicStatus1, ultrasonicStatus2, ultrasonicStatus3;

  if (distance1 < 10) {
    ultrasonicStatus1 = 1;
      delay(1000);
  } else {

    ultrasonicStatus1 = 0;
      delay(1000);
  }

  if (distance2 < 10) {
    ultrasonicStatus2 = 1;
      delay(1000);
  } else {
    ultrasonicStatus2 = 0;
      delay(1000);
  }

  if (distance3 < 10) {
    ultrasonicStatus3 = 1;
      delay(1000);
  } else {
    ultrasonicStatus3 = 0;
      delay(1000);
  }

  sendDataToServer(ultrasonicSensorId1, ultrasonicStatus1);
  sendDataToServer(ultrasonicSensorId2, ultrasonicStatus2);
  sendDataToServer(ultrasonicSensorId3, ultrasonicStatus3);

  delay(1000);
}

void sendDataToServer(int id, int status) {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    String data = "idsensor=" + String(id) + "&status=" + String(status);
    Serial.println("Sending data: " + data);

    http.begin(serverUrl);
    http.addHeader("Content-Type", "application/x-www-form-urlencoded");
    int httpResponseCode = http.POST(data);

    if (httpResponseCode > 0) {
      String response = http.getString();
      Serial.println("HTTP Response: " + response);
    } else {
      Serial.println("Error sending data to server");
    }

    http.end();
  } else {
    Serial.println("WiFi Disconnected. Unable to send data.");
  }
}
