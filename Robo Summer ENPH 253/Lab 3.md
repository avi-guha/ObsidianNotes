Purpose of this lab is to understand optical isolators, and develop an H-Bridge circuit.
Below is the code I will use to adjust the potentiometer and create a variable speed/direction controller


``` cpp;

// Read a potentiometer on an ADC pin and print the measured voltage

#include <Arduino.h>

#include "driver/ledc.h"

#include "driver/adc.h"

  

#define pwmChannel1 1

#define pwmOutA 32 // GPIO pin for PWM output

#define pwmChannel2 2 // PWM channel number

#define pwmOutB 33 // PWM channel number for the second output

  

int pot_read = 0; // ADC pin for the potentiometer

int tempRead = 0; // Temporary variable for mapped value

void setup() {

  Serial.begin(115200);

  delay(100);

  adc1_config_width(ADC_WIDTH_BIT_12); // Set ADC width to 12 bits

  adc1_config_channel_atten(ADC1_CHANNEL_0, ADC_ATTEN_DB_12); // Set attenuation for channel 0 (GPIO 36)

  ledcSetup(pwmChannel1, 1000, 12); // Setup PWM on channel 1 with a frequency of 1000 Hz

  ledcAttachPin(pwmOutA, pwmChannel1); // Attach PWM output to pin 4

  ledcAttachPin(pwmOutB, pwmChannel2); // Attach PWM output to pin 32

}

  

void loop() {

  // 1) read raw (0…4095)

  pot_read = adc1_get_raw(ADC1_CHANNEL_0);

  if(pot_read / 4095.0 > 0.5) {

    tempRead = pot_read;

    tempRead = map(tempRead, (4095/2), 4095, 0, 4095);

    ledcWrite(pwmChannel1, pot_read);

    Serial.print("Potentiometer reading: ");

    Serial.println(pot_read);

    Serial.print("Mapped value for PWM Channel 1: ");

    Serial.println(tempRead);

  }

  else {

    tempRead = pot_read;

    tempRead = map(tempRead, 0, (4095/2), 4095, 0);

    ledcWrite(pwmChannel2, pot_read);

    Serial.print("Potentiometer reading: ");

    Serial.println(pot_read);

    Serial.print("Mapped value for PWM Channel 2: ");

    Serial.println(tempRead);

  }

  delay(500);

}
```
