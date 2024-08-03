#include <LiquidCrystal.h>
#include <Keypad.h>
#include <EEPROM.h>

#define Password_Length 5

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

const byte ROWS = 4; 
const byte COLS = 4; 
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {6, 7, 8, 9}; 
byte colPins[COLS] = {A1, A2, A3, A4}; 

char Data[Password_Length]; 
char Data2[Password_Length];
char Master[Password_Length];
byte data_count = 0;
bool Pass_is_good;
char key;
byte mode = 0;

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

void setup() {
  Serial.begin(9600);
  Check_EEPROM();

  lcd.begin(16, 2);

  pinMode(13, OUTPUT); // Green LED
  pinMode(10, OUTPUT); // Red LED
  digitalWrite(13, LOW);
  digitalWrite(10, LOW);
}

void loop() {
  key = keypad.getKey();

  if (key) {
    Serial.println(key);
    if (key == '#' && mode == 3) {
      mode = 1; // Enter change password mode
      lcd.clear();
      clearData();
    } else if (key == '*' && mode == 3) {
      mode = 0; // Lock the system
      lcd.clear();
      lcd.setCursor(4, 0);
      lcd.print("LOCKED");
      digitalWrite(13, LOW);
      digitalWrite(10, LOW);
      delay(2000);
    } else if (mode != 3) {
      collectKey();
    }
  }

  displayMessage();

  if (data_count == Password_Length - 1) {
    processPassword();
  }
}

void collectKey() {
  Data[data_count] = key;
  lcd.setCursor(4 + data_count, 1);
  lcd.print("*");
  data_count++;
}

void clearData() {
  while (data_count != 0) {
    Data[data_count--] = 0;
  }
}

void Check_EEPROM() {
  EEPROM.get(0, Master);
  if (Master[0] == 0 && Master[1] == 0 && Master[2] == 0 && Master[3] == 0) {
    Serial.println("No EEPROM PASSWORD FOUND");
    char FirstTimePassword[] = {'0','0','0','0'};
    EEPROM.put(0, FirstTimePassword);
    EEPROM.get(0, Master);
  }
}

void displayMessage() {
  if (mode == 0) {
    lcd.setCursor(1, 0);
    lcd.print("Enter Password");
  } else if (mode == 1) {
    lcd.setCursor(0, 0);
    lcd.print("Set New Password");
  } else if (mode == 2) {
    lcd.setCursor(0, 0);
    lcd.print("Password Again");
  } else if (mode == 3) {
    lcd.setCursor(4, 0);
    lcd.print("UNLOCKED");
  }
}

void processPassword() {
  if (mode == 0) { // Enter Password Mode
    lcd.clear();
    if (!strcmp(Data, Master)) {
      lcd.setCursor(0, 0);
      lcd.print("WELCOME Back!");
      lcd.setCursor(0, 1);
      lcd.print("By Jubran Khoury");

      digitalWrite(13, HIGH); // Green LED ON
      digitalWrite(10, LOW);  // Red LED OFF

      delay(5000); // Keep the green LED on for 5 seconds

      digitalWrite(13, LOW); // Turn off green LED after delay

      mode = 3; // Unlocked mode
      lcd.clear();
    } else {
      lcd.setCursor(2, 0);
      lcd.print("INCORRECT !");
      lcd.setCursor(4, 1);
      lcd.print("PASSWORD");

      digitalWrite(13, LOW);  // Green LED OFF
      digitalWrite(10, HIGH); // Red LED ON

      delay(2000);
      digitalWrite(10, LOW); 
      lcd.clear();
    }
    delay(1000);
  } else if (mode == 1) { // Set New Password Mode
    lcd.clear();
    mode = 2;
    for (int i = 0; i < Password_Length; i++) {
      Data2[i] = Data[i];
    }
    clearData();
  } else if (mode == 2) { // Confirm New Password Mode
    if (!strcmp(Data, Data2)) {
      lcd.clear();
      lcd.setCursor(0, 0);
      lcd.print("New Password is ");
      lcd.setCursor(4, 1);
      lcd.print(Data);
      delay(2000);
      lcd.clear();
      lcd.setCursor(4, 0);
      lcd.print("Saving...");
      for (int i = 0; i <= 100; i += 10) {
        lcd.setCursor(4, 1);
        lcd.print(i);
        lcd.setCursor(7, 1);
        lcd.print("%");
        delay(200);
      }
      EEPROM.put(0, Data);
      EEPROM.get(0, Master);
      delay(500);
    } else {
      lcd.clear();
      lcd.setCursor(4, 0);
      lcd.print("PASSWORD");
      lcd.setCursor(3, 1);
      lcd.print("NOT MATCH!");
      delay(2000);
    }
    mode = 0; // Go back to locked mode
    lcd.clear();
  }
  clearData();
}
