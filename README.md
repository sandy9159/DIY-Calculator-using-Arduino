# DIY-Calculator-using-Arduino

![image](https://user-images.githubusercontent.com/19898602/170180751-7e73e287-5521-4cb6-a4d2-49aeb388fbfe.png)

Performing simple mathematical operations was the basis on which one of the first computing machines designed by Charles Babbage who is considered as the father of computers was a steam-driven machine used to compute tables of numbers. 

In this tutorial, we will build our own simple calculator using an Arduino and 4×4 keypad and display the result on a 16×2 dot matrix LCD Screen. Our calculator will be able to perform Addition, Subtraction, Multiplication, and Division. If you want to create your own mathematical function, you can do the same. 

Just play around with the program and you will understand.

 

# COMPONENTS REQUIRED

Breadboard

Arduino Uno

Arduino Uno Programming Cable

4×4 Keypad

16×2 LCD Screen

Jumper Wires

10kΩ Potentiometer

![image](https://user-images.githubusercontent.com/19898602/170180833-c77f9c8b-748c-4bc7-9102-8408e04a2f1f.png)


# CIRCUIT DIAGRAM

In this diagram, we can see that there are eight pins coming out of the 4×4 Keypad. It has 4 pins for rows and 4 pins for columns.

These 8 PINS are driven out from 16 buttons present in the MODULE. Those 16 alphanumeric digits on the MODULE surface are the 16 buttons arranged in MATRIX formation. These are mostly used for the control system or embedded systems where we need to give some command to the system to perform a given task 

or when we need to perform multiple tasks but with a smaller number of pins.



![image](https://user-images.githubusercontent.com/19898602/170180900-0ae46082-ddd6-4ada-8a07-5cefd61727bc.png)

Understanding how 4×4 Keypad works is a little tricky. The module only has 8 pins to interface 16 buttons. To understand consider we have connected the pad to a microcontroller now set all the rows to output and set them logic HIGH that is 5V. 

Now set all the columns to INPUT to make them sense logic HIGH.



![image](https://user-images.githubusercontent.com/19898602/170180946-8fa6b62d-4356-4fa2-a7da-26fb10b5bc4a.png)
![image](https://user-images.githubusercontent.com/19898602/170180947-e5fed386-29a6-410a-9417-2cd4f3401329.png)

Now as the key is pressed at ROW 3 and COLUMN 2, the current flows as shown in the figure. As all Columns are inputs sensing high, so controller sense that logic HIGH is present at C2 pin and store it in its memory that button pressed is in Column 2.

![image](https://user-images.githubusercontent.com/19898602/170180994-4ef6b111-ab4d-42df-a78a-0862752b5abc.png)


Now all Column pins are set HIGH and all Rows pin are set as INPUT to sense the incoming logic HIGH.

Now the current flows as shown in the figure and Microcontroller sense that Pin R3 is getting Logic High, so button pressed is in Row 3 and it knew that button is in Column 2 from earlier settings. So in this way, the controller senses which key is being pressed on the keypad.


# ARDUINO CODE EXPLANATION


First, you have to make sure that you have the necessary libraries installed in your Arduino IDE from the links given below-

For LCD screen library see https://www.arduino.cc/en/Reference/LiquidCrystal

For 4*4 Keypad Library download from https://github.com/Chris--A/Keypad

 

Start the code by including the libraries in the code and then define the keys on the pad. Put in the pins, the Keypad and LCD screen is connected to. Create the keypad variable, so that you don’t have to deal with the identification process of the pressed key, let the library take care of it.

#include <LiquidCrystal.h>

#include <Keypad.h> // Download library for Keypad from https://github.com/Chris--A/Keypad

const byte ROWS = 4;

const byte COLS = 4;

char keys[ROWS][COLS] = {

  {'1','2','3','A'},

  {'4','5','6','B'},

  {'7','8','9','C'},

  {'*','0','#','D'}

};    // defining the keys in the keypad

byte rowPins[ROWS] = { 2, 3, 4, 5 };

byte colPins[COLS] = { 6, 7, 8, 9 };

Keypad kpd = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );

 

Now initialize the serial monitor and start giving the inputs from the keypad to the Arduino. The functions defined in the code will take the input and display the result on the LCD screen. Make sure that you press the ‘#’ button to get the result after pressing the numbers and operand.

 

The arithmetical operations will be performed by-



Character on Keypad

Assumed to be

‘A’

Addition (+)

‘B’

Subtraction (-)

‘C’

Multiplication (*)

‘D’

Division (/)

‘*’

Clear (C)

‘#’

Equals (=)


The final result will be updated in the second line of the LCD screen as shown and given by the following code-

void DisplayResult()

{

  lcd.setCursor(0, 0);   // set the cursor to column 0, line 1

  lcd.print(Num1); lcd.print(action); lcd.print(Num2);  // Display the result on LCD Screen

  Serial.println();

  Serial.println(Num1); Serial.print(action); Serial.print(Num2);  // Display the result on Serial Monitor

  if (result==true)

  //{lcd.print(" ="); lcd.print(Number);} //Display the result

  Serial.print(" = "); Serial.print(Number);

  lcd.setCursor(0, 1);   // set the cursor to column 0, line 1

  lcd.print(Number); //Display the result

  // delay(1000); change this to suit your needs

}



# ARDUINO CALCULATOR CODE 

```
#include <LiquidCrystal.h> //See the functions and commands in library for LCD screen from this url https://www.arduino.cc/en/Reference/LiquidCrystal
#include <Keypad.h> // Download library for Keypad from https://github.com/Chris--A/Keypad

const byte ROWS = 4; // Four rows
const byte COLS = 4; // Four columns as this is a 4*4 Keypad

char keys[ROWS][COLS] = {

{'1','2','3','A'},

{'4','5','6','B'},

{'7','8','9','C'},

{'*','0','#','D'}

}; // defining the keys in the keypad

byte rowPins[ROWS] = { 2, 3, 4, 5 };// Connect keypad ROW0, ROW1, ROW2 and ROW3 to these Arduino pins.
byte colPins[COLS] = { 6, 7, 8, 9 }; // Connect keypad COL0, COL1, COL2 and COL3 to these Arduino pins.

Keypad kpd = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS ); // Create the Keypad variable to identify and store the pressed key.

const int rs = A0, en = A1, d4 = 10, d5 = 11, d6 = 12, d7 = 13; //Pins to which LCD is connected
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

long Num1,Num2,Number; // defining the variables for the operations
char key,action;
boolean result = false; // boolean variable so that we display the result whenever its available

void setup() {
lcd.begin(16, 2); //We are using a 16*2 LCD display
lcd.print("4*4 Calculator"); //Display a intro message
lcd.setCursor(0, 1); // set the cursor to column 0, line 1
lcd.print("Quartz Components"); //Display a intro message
Serial.begin(9600); // Initialise the serial communication
Serial.println("Ready to initiate"); // for displaying on serial monitor
delay(3000); //Wait for display to show info
lcd.clear(); //Then clean it
}

void loop() {

key = kpd.getKey(); // for storing pressed key value in a character variable (char)

if (key!=NO_KEY)
DetectButtons(); // Identify which button is being pressed and work accordingly

if (result==true)
CalculateResult(); // If the result needs to be calculated then this function will take care of it

DisplayResult(); // Display the obtained result on the Display or serial monitor
}

void DetectButtons()
{
lcd.clear(); // First clear the LCD
if (key=='*') //If cancel Button is pressed perform the below mentioned equation or function
{Serial.println ("Button Cancel"); Number=Num1=Num2=0; result=false;}

if (key == '1') //If Button 1 is pressed
{Serial.println ("Button 1");
if (Number==0)
Number=1;
else
Number = (Number*10) + 1; // Perform this if the button is Pressed twice
}

if (key == '4') //If Button 4 is pressed
{Serial.println ("Button 4");
if (Number==0)
Number=4;
else
Number = (Number*10) + 4; // Perform this if the button is Pressed twice
}

if (key == '7') //If Button 7 is pressed
{Serial.println ("Button 7");
if (Number==0)
Number=7;
else
Number = (Number*10) + 7; // Perform this if the button is Pressed twice
}


if (key == '0')
{Serial.println ("Button 0"); //Button 0 is Pressed
if (Number==0)
Number=0;
else
Number = (Number*10) + 0; // Perform this if the button is Pressed twice
}

if (key == '2') //Button 2 is Pressed
{Serial.println ("Button 2");
if (Number==0)
Number=2;
else
Number = (Number*10) + 2; // Perform this if the button is Pressed twice
}

if (key == '5')
{Serial.println ("Button 5");
if (Number==0)
Number=5;
else
Number = (Number*10) + 5; //Pressed twice
}

if (key == '8')
{Serial.println ("Button 8");
if (Number==0)
Number=8;
else
Number = (Number*10) + 8; //Pressed twice
}


if (key == '#')
{Serial.println ("Button Equal");
Num2=Number;
result = true;
}

if (key == '3')
{Serial.println ("Button 3");
if (Number==0)
Number=3;
else
Number = (Number*10) + 3; //Pressed twice
}

if (key == '6')
{Serial.println ("Button 6");
if (Number==0)
Number=6;
else
Number = (Number*10) + 6; //Pressed twice
}

if (key == '9')
{Serial.println ("Button 9");
if (Number==0)
Number=9;
else
Number = (Number*10) + 9; //Pressed twice
}

if (key == 'A' || key == 'B' || key == 'C' || key == 'D') //Detecting Buttons on Column 4
{
Num1 = Number;
Number =0;
if (key == 'A')
{Serial.println ("Addition"); action = '+';}
if (key == 'B')
{Serial.println ("Subtraction"); action = '-'; }
if (key == 'C')
{Serial.println ("Multiplication"); action = '*';}
if (key == 'D')
{Serial.println ("Devesion"); action = '/';}

delay(100);
}

}

void CalculateResult()
{
if (action=='+')
Number = Num1+Num2;

if (action=='-')
Number = Num1-Num2;

if (action=='*')
Number = Num1*Num2;

if (action=='/')
Number = Num1/Num2;
}

void DisplayResult()
{
lcd.setCursor(0, 0); // set the cursor to column 0, line 0
lcd.print(Num1); lcd.print(action); lcd.print(Num2); // Display the result on LCD Screen
Serial.println();
Serial.println(Num1); Serial.print(action); Serial.print(Num2); // Display the result on Serial Monitor

if (result==true)
//{lcd.print(" ="); lcd.print(Number);} //Display the result
Serial.print(" = "); Serial.print(Number);
lcd.setCursor(0, 1); // set the cursor to column 0, line 1
lcd.print(Number); //Display the result
// delay(1000); change this to suit your needs
}

```








