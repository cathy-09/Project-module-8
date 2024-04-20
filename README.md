#Project - Scientific Calculator
==========================================================================================================================================

Темата на проектната работа е научен калкулатор. Той представлява калкулатор, който използва Arduino платка и LCD дисплей. Разработената вградена система, позволява на потребителя да извършва различни математически операции и да види визуализиращият се резултат на LCD екрана. Според това, кой от бутоните е натиснат от потребителя, програмата го разпознава като дадена математическа операция или число, и извършва съответните действия с въведените данни. Включени са различни операции като събиране, изваждане, умножение, деление, изчисляване на корен квадратен, тригонометричните функции – синус и косинус и повдигане на дадено число на избраната степен. Програмата включва една обработка на грешка и тя е проверка за деление на нула и извеждане на съобщения на LCD екрана.

Проектът се осъществява с помощта на симулационната платформа TinkerCad. Създадена е както работеща симулация и функционира перфектно.

Целта на проекта е да предостави удобен и лесен за използване калкулатор, който може да бъде изграден с достъпни електронни компоненти и да се използва за различни математически задачи.

След като обмислихме идеята и разпределихме задачите, всеки член от екипа взе участие в разработването на всяка част от проекта. Създадохме симулация в TinkerCad. След успешното създаване на симулацията, преминахме към дебъгване на проекта и създаването на електрическа и блок схема и таблица с компоненти. Накрая, написахме документацията и подготвихме презентацията за нашия проект.

## Authors

- [@Petio0](https://github.com/Petio0)
- [@cathy-09](https://github.com/cathy-09)
- [@Lemon1069](https://www.github.com/Lemon1069)
- [@CozDoog101](https://www.github.com/CozDoog101)

## Montage scheme

![image](https://github.com/cathy-09/Project-module-8/assets/158329994/a34c0a2f-6007-4ed7-951b-aa9c06709cd9)

## Usage/Examples

```c++
#include <LiquidCrystal.h>
#include <math.h>

LiquidCrystal lcd(13, 12, A2, A3, A4, A5);

unsigned long int firstArg = 0, secondArg = 0;
double result = 0;
double ans = 0;
bool calculatable = false;
String operation = "";

void setup()
{
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.print("Scientific-Calc");
  delay(1000);
  lcd.clear();
}

void loop() {
  int valueA0 = analogRead(A0);
  int valueA1 = analogRead(A1);
  if (valueA0 != 1023 || valueA1 != 1023) {
  	detectButtons();
  }
  if (calculatable) {
  	calculateResult();
  }
  displayResult();
  delay(500);
  lcd.clear();
}

void detectButtons() {
  int valueA0 = analogRead(A0);
  int valueA1 = analogRead(A1);
  switch (valueA1) { //switch on the larger keypad (basic operations)
    case 0: // number 0
	{
    	if (result == 0) {
        result = 0;
    	} else {
        	result *= 10;
    	}
	}
	break;
	case 50: // number 1
	{
    	if (result == 0) {
        result = 1;
    	} else {
        result = result * 10 + 1;
    	}
	}
	break;
	case 100: // number 2
	{
    	if (result == 0) {
        result = 2;
    	} else {
        	result = result * 10 + 2;
    	}
	}
	break;
	case 150: // number 3
	{
    	if (result == 0) {
        result = 3;
    	} else {
    	    result = result * 10 + 3;
    	}
	}
    break;
	case 200: // number 4
	{
    	if (result == 0) {
        result = 4;
    	} else {
        	result = result * 10 + 4;
    	}
	}
	break;
	case 250: // number 5
	{
    	if (result == 0) {
        result = 5;
    	} else {
        	result = result * 10 + 5;
    	}
	}
	break;
	case 300: // number 6
	{
    	if (result == 0) {
        result = 6;
    	} else {
        	result = result * 10 + 6;
    	}
	}
	break;
	case 350: // number 7
	{
    	if (result == 0) {
        result = 7;
    	} else {
        	result = result * 10 + 7;
    	}
	}
	break;
	case 400: // number 8
	{
    	if (result == 0) {
        result = 8;
    	} else {
        	result = result * 10 + 8;
    	}
	}
	break;
	case 450: // number 9
	{
    	if (result == 0) {
        result = 9;
    	} else {
        	result = result * 10 + 9;
    	}
	}
	break;
    case 500: //=
    {
    	secondArg = result;
    	calculatable = true;
    }
    	break;
    case 550: //multiplication
    {
    	operation = "*";
    	firstArg = result;
    	result = 0;
    }
    	break;
    case 600: //division
    {
    	operation = "/";
    	firstArg = result;
    	result = 0;
    }
    	break;
    case 650: //subtraction
    {
    	operation = "-";
    	firstArg = result;
    	result = 0;
    }
    	break;
    case 700: //addition
    {
    	operation = "+";
    	firstArg = result;
    	result = 0;
    }
    	break;
  }
  
  switch (valueA0) { //switch on smaller keypad (complicated operations)
  	case 0: //ans
    	result = ans;
    	break;
    case 200: //log10
    	operation = "log10";
    	break;
    case 300: //cos
    	operation = "cos";
    	break;
    case 400: //sin
    	operation = "sin";
    	break;
    case 500: //x^y
    	operation = "^";
    	firstArg = result;
    	result = 0;
    	break;
    case 600: //sqrt
    	operation = "sqrt";
    	break;
    case 700: //clr
    	operation = "";
    	firstArg = 0;
    	secondArg = 0;
    	ans = result;
    	result = 0;
    	calculatable = false;
    	break;
  }
  
  delay(500);
}

void calculateResult() {
  result = 0;
  if (operation.equals("+")) {
  	result = firstArg + secondArg;
  }
  if (operation.equals("-")) {
  	result = firstArg - secondArg;
  }
  if (operation.equals("*")) {
  	result = firstArg * secondArg;
  }
  if (operation.equals("/")) {
    if (secondArg == 0) {
      	lcd.setCursor(0, 1);
    	lcd.print("Can't divide by 0");
    }
  	result = (float)firstArg / secondArg;
  }
  if (operation.equals("log10")) {
  	result = log10(secondArg);
  }
  if (operation.equals("sin")) {
  	result = sin(secondArg);
  }
  if (operation.equals("cos")) {
  	result = cos(secondArg);
  }
  if (operation.equals("^")) {
  	result = pow(firstArg, secondArg);
  }
  if (operation.equals("sqrt")) {
  	result = sqrt(secondArg);
  }
  //resultText = new String(result);
}

void displayResult() {
  lcd.setCursor(0, 0);
  if (firstArg != 0) {
    lcd.print(firstArg);
  }
  else {
    lcd.print(result, 0);
  }
  if (!operation.equals("")) {
  	lcd.print(operation);
  }
  else {
    lcd.print(" ");
  }
  if (secondArg != 0) {
    lcd.print(secondArg);
  }
  else if (firstArg != 0) {
    lcd.print(result, 0);
  }
  if (calculatable) {
    if (firstArg == 0) {
      lcd.clear();
      lcd.print(operation);
      lcd.print(secondArg);
    }
    lcd.print(" = ");
    lcd.print(result, 2);
  }
}
```

## TinkerCad Link

[TinkerCad](https://www.tinkercad.com/things/7LO7A2wiD7R-copy-of-project-module-8/editel?sharecode=GNh6WFG-WGh9ctIyL7fgSFLxO-9XOmSvVyPHAaL6WXg)
