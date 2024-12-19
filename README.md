# Jetson_Arduino
Preview
----
# Breadboard
![image](https://github.com/user-attachments/assets/f3e61c03-0169-4026-92e8-f36ecb902e5b)
1. GND Connection
   - Ensure proper grounding for your circuit by connecting GND appropriately.
2. Power Rails
   - Red and Blue Lines: These represent the power rails (typically 5V).
      - Power flows horizontally along these lines.
   - Mini Breadboards: Mini versions do not have dedicated red and blue power rails.
3. Signal Rows
   - The center rows are isolated until connected by jumper wires.
   - When a jumper wire is connected to a signal row, the entire horizontal row becomes electrically connected.
4. Jumper Wires
   - Exposed Ends: Use male jumper wires for connections where pins are visible.
   - No Exposed Ends: Use female jumper wires for connecting components without exposed pins.

# Arduino Uno
![image](https://github.com/user-attachments/assets/7e0adcea-5b2c-4e76-8d54-3e161bdf2747)
### Jetson Nano
- Provides voltage and current for components.
- Used to process analog values with Arduino Uno.
### Arduino Uno Rev3
- Official board from Arduino.cc.
- Key features:
   - Analog Input: Reads analog values.
   - Communication Pins:
      - Pins 4, 5: General communication.
      - Pins 0, 1: Serial communication.
   - Digital Pins: 0–13 for various uses.
   - Power Pins: VIN (external power), GND (x2), 5V, 3.3V.
 
## Arduino down on Jetson Nano sd card
```
sudo apt-get update
sudo apt-get install arduino
```

4 week
======

# Arduino Setup via Terminal

## **Light Sensor and LED Control**  
- **LED_BUILTIN**: Defined as an output (commonly connected to pin **13**).  
- **void loop**:  
  - The computer processes binary (0s and 1s), so the LED state is defined as **HIGH** or **LOW**.  
- **Delay Function**:  
  - `delay(100)` controls the blink speed. Increasing the value slows down the blinking.
 ![KakaoTalk_20241130_222648357](https://github.com/user-attachments/assets/19430c6d-ddc3-4c61-9fd2-e9b4e340a673)
Here’s an updated summary incorporating the LED pin connection:

## **LED Pin Connection**  
- Connect the **long leg** of the LED to **pin 12** and the **short leg** to **GND**.  

## **Code Setup**  
- Define the LED pin:  
  ```cpp
  int led = 12;
  ```  
- Set it as an output:  
  ```cpp
  pinMode(led, OUTPUT);
  ```
# Arduino Setup and Dust Sensor Output
## **Setup**   
- Set port: **COM1** or **COM2**.  
## **Wiring**  
- Black: **GND**  
- Red: **5V**  
- Yellow: Data (e.g., pin **2**)  
![KakaoTalk_20241130_222648357_01](https://github.com/user-attachments/assets/b54b6693-fa36-4522-a86f-008717a1dff6)
## **Dust Sensor Output**  
1. **a: LPO Time** - Total low pulse signal time during measurement.  
2. **b: Ratio** - Proportion of LPO time to total sampling time.  
3. **c: Concentration** - Calculated particle concentration (**μg/m³**).
```
int pin = 8;
unsigned long duration;
unsigned long starttime;
unsigned long sampletime_ms = 30000;  // 30초 동안 샘플링
unsigned long lowpulseoccupancy = 0;
float ratio = 0;
float concentration = 0;

void setup()
{
    Serial.begin(9600);
    pinMode(pin, INPUT);
    starttime = millis();
    Serial.println("미세먼지 측정을 시작합니다...");
    Serial.println("==============================");
}

void loop()
{
    duration = pulseIn(pin, LOW);
    lowpulseoccupancy = lowpulseoccupancy + duration;

    if ((millis()-starttime) > sampletime_ms)  // 30초마다 측정
    {
        ratio = lowpulseoccupancy/(sampletime_ms*10.0);
        concentration = 1.1*pow(ratio,3)-3.8*pow(ratio,2)+520*ratio+0.62; // ug/m3 단위

        Serial.println("==============================");
        Serial.print("미세먼지 농도: ");
        Serial.print(concentration);
        Serial.println(" ug/m3");

        // 대기질 상태 표시
        Serial.print("대기질 상태: ");
        if(concentration <= 30) {
            Serial.println("좋음");
        }
        else if(concentration <= 80) {
            Serial.println("보통");
        }
        else if(concentration <= 150) {
            Serial.println("나쁨");
        }
        else {
            Serial.println("매우 나쁨");
        }

        Serial.println("------------------------------");
        Serial.print("측정 시간: ");
        Serial.print(millis()/1000);
        Serial.println("초");
        Serial.println("==============================\n");

        // 다음 측정을 위한 초기화
        lowpulseoccupancy = 0;
        starttime = millis();
    }
}
```
![KakaoTalk_20241130_222648357_02](https://github.com/user-attachments/assets/a898ed96-d3b1-40bc-b6ea-ae7c0b64653e)
