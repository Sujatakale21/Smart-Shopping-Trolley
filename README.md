# Smart-Shopping-Trolley
#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN 10
#define RST_PIN 9

MFRC522 rfid(SS_PIN, RST_PIN);

float totalBill = 0;

void setup() {
  Serial.begin(9600);
  SPI.begin();
  rfid.PCD_Init();

  Serial.println("Smart Shopping Trolley Started");
}

void loop() {

  if (!rfid.PICC_IsNewCardPresent())
    return;

  if (!rfid.PICC_ReadCardSerial())
    return;

  String uid = "";

  for (byte i = 0; i < rfid.uid.size; i++) {
    uid += String(rfid.uid.uidByte[i], HEX);
  }

  uid.toUpperCase();

  if (uid == "A1B2C3D4") {
    totalBill += 50;
    Serial.println("Milk Added - Rs 50");
  }
  else if (uid == "E5F6G7H8") {
    totalBill += 30;
    Serial.println("Bread Added - Rs 30");
  }

  Serial.print("Total Bill: Rs ");
  Serial.println(totalBill);

  rfid.PICC_HaltA();
}
