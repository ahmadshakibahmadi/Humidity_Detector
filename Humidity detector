#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>

// --------- SENSOR SETUP ---------
#define DHTPIN 2        // DHT data pin connected to D2
#define DHTTYPE DHT11   // Use DHT22 if you have that sensor
DHT dht(DHTPIN, DHTTYPE);

// --------- LCD SETUP ---------
LiquidCrystal_I2C lcd(0x27, 16, 2);  
// Change 0x27 to 0x3F if your LCD shows nothing

// --------- BUZZER SETUP ---------
const int BUZZER_PIN = 8; 
const float RH_LOW_ON  = 45.0; // alert if humidity < 45%
const float RH_LOW_OFF = 48.0;
const float RH_HIGH_ON = 75.0; // alert if humidity > 75%
const float RH_HIGH_OFF = 72.0;
bool alarmActive = false;

void setup() {
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);

  lcd.init();      
  lcd.backlight(); 
  dht.begin();
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature(); // Celsius

  // Handle sensor errors
  if (isnan(h) || isnan(t)) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Sensor Error!");
    delay(2000);
    return;
  }

  // ---------- HUMIDITY ALARM LOGIC ----------
  if (!alarmActive && (h < RH_LOW_ON || h > RH_HIGH_ON)) {
    alarmActive = true;
  } else if (alarmActive && (h > RH_LOW_OFF && h < RH_HIGH_OFF)) {
    alarmActive = false;
  }

  // If alarm is active → short beep
  if (alarmActive) {
    digitalWrite(BUZZER_PIN, HIGH);
    delay(150);
    digitalWrite(BUZZER_PIN, LOW);
  }

  // ---------- DISPLAY ON LCD ----------
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(t, 1); // one decimal
  lcd.print((char)223); // degree symbol
  lcd.print("C");

  lcd.setCursor(0, 1);
  lcd.print("Hum: ");
  lcd.print(h, 1);
  lcd.print("%");

  delay(1850); // together with buzzer delay ~2s refresh
}


