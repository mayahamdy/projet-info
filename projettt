#include <Adafruit_NeoPixel.h>
#include <LiquidCrystal_I2C.h>
#include <stdlib.h> // Required for random number generation



#define PIN 12
#define NUMPIXELS 2
Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);



LiquidCrystal_I2C lcd(0x27, 20, 4);
int bp1 = 2;
int bp2 = 4;
int bp3 = 8;
bool rep = false;

// Struct to hold a question and its answer
struct Question {
  const char* question;
  bool answer;
};

// Array of true/false questions and their answers
Question questions[] = {
  {"Fernando Alonso a le record de victoires au Mans", false},
  {"Jacky Icks a le recordde victoires au Mans", false},
  {"Tom Kristensen a le recordde victoires au Mans", true},
  {"Toshio Suzuki s’est envole dans la ligne droite des Hunaudieres en 1999", false},
  {"Henry II est ne au vieux Mans", true},
  {"La ville du Mans existe depuis 1156", false},
  {"La Cathedrale a ete brulee en 1130", true},
  // Add more questions and answers as needed
};
const int numQuestions = sizeof(questions) / sizeof(questions[0]);

const int LCD_COLUMNS = 20; // Assuming 20 columns for a 20x4 LCD
const int LCD_ROWS = 4;     // Number of rows for a 20x4 LCD

// Function to display a question on the LCD, with text wrapping
void displayQuestion(const char* question) {
  lcd.clear();
  int currentRow = 0;
  int currentColumn = 0;

  // Loop through each character of the question
  for (int i = 0; question[i] != '\0'; i++) {
    // Check if the current character is a space or if it's at the end of the line
    if (question[i] == ' ' || currentColumn >= LCD_COLUMNS) {
      // Move to the next row if it's not the last row and the current character is not a space
      if (currentRow < LCD_ROWS - 1 && question[i] != ' ') {
        currentRow++;
        currentColumn = 0;
        lcd.setCursor(currentColumn, currentRow); // Set cursor to the beginning of the next line
      }
    } else {
      // Print the current character
      lcd.print(question[i]);
      currentColumn++;
    }
  }
}





void setup() {
  lcd.init();
  pixels.begin();
  pixels.show();
  pinMode(bp1, INPUT_PULLUP);
  pinMode(bp2, INPUT_PULLUP);
  pinMode(bp3, INPUT_PULLUP);
  Serial.begin(9600);
  lcd.backlight();
  randomSeed(analogRead(0)); // Initialize random seed
}






void loop() {
  lcd.setCursor(4, 0);
  lcd.print("============");
  lcd.setCursor(0, 1);
  lcd.print("btn 3 pour commencer");
  lcd.setCursor(1, 2);
  lcd.print("tu as 20 secondes");
  lcd.setCursor(4, 3);
  lcd.print("============");

  if (digitalRead(bp3) == LOW) {
    rep = true;
    while (rep) {
      lcd.clear();
      // Select a random question
      int index = random(numQuestions);
      lcd.setCursor(0, 0);
      lcd.print(questions[index].question);

      // Wait for user's input
      unsigned long startTime = millis();
      while (millis() - startTime < 20000) { // Wait for 10 seconds
        if (digitalRead(bp1) == LOW || digitalRead(bp2) == LOW) {
          lcd.clear();
          lcd.setCursor(0, 0);
          if ((digitalRead(bp1) == LOW && questions[index].answer == true) ||
              (digitalRead(bp2) == LOW && questions[index].answer == false)) {
                
            pixels.setPixelColor(0, 150, 0, 0);
            pixels.setPixelColor(1, 150, 0, 0);
            
            pixels.show();
            lcd.print("=========");
            lcd.setCursor(0, 1);
            lcd.print("CORRECT");
            lcd.setCursor(8, 2);
            lcd.print("yay :)");
            
          } else {
            
            pixels.setPixelColor(0, 0, 150, 0);
            pixels.setPixelColor(1, 0, 150, 0);

            pixels.show();
            lcd.print("=========");
            lcd.setCursor(0, 1);
            lcd.print("INCORRECT");
            lcd.setCursor(0, 2);
            lcd.print("dumbass");
          }
          lcd.setCursor(0, 3);
          lcd.print("=========");
          delay(4000);
          lcd.clear();
          pixels.setPixelColor(0, 0, 0, 0);
          pixels.setPixelColor(1, 0, 0, 0);
          pixels.show();
          rep = false; // Exit the loop
          break;
        }
      }
      if (rep) { // If loop exited without user's input, reset the loop
        break;
      }
    }
  }
}
