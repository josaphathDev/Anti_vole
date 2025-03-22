 #include <SoftwareSerial.h>

SoftwareSerial mySerial(7, 8);  // RX sur pin 7, TX sur pin 8
int relais = 4;  // Pin du relais

void setup() {
    Serial.begin(19200);
    mySerial.begin(19200);
    
    pinMode(relais, OUTPUT);
    digitalWrite(relais, LOW);  // Assurez-vous que le relais est désactivé au démarrage

    Serial.println("Module GSM prêt...");
    mySerial.println("AT");  // Vérifier la communication avec le module
    delay(1000);
}

void loop() {
    if (mySerial.available()) {
        String data = mySerial.readString();  // Lire la réponse du module
        Serial.println(data);  // Afficher la réponse dans le moniteur série

        if (data.indexOf("RING") != -1) {  // Si un appel est reçu
            digitalWrite(relais, HIGH);  // Activer le relais (allumer la LED)
            Serial.println("📞 Appel reçu : relais activé, LED allumée !");
        }

        if (data.indexOf("NO CARRIER") != -1 || data.indexOf("BUSY") != -1) {  
            digitalWrite(relais, LOW);  // Désactiver le relais (éteindre la LED)
            Serial.println("🚫 Appel terminé : relais désactivé, LED éteinte !");
        }
    }
}
