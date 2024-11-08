//Master

#include <Wire.h>

void setup() { 
    Wire.begin(); // Start I2C as master
    pinMode(2, INPUT);      // Button
    pinMode(7, OUTPUT);     // Green 
    pinMode(6, OUTPUT);     // Red 

    // Start with pedestrian red
    digitalWrite(7, LOW); 
    digitalWrite(6, HIGH); 
}

void loop() { 
    if (digitalRead(2) == HIGH) { 
        // Pedestrian button pressed
        digitalWrite(7, LOW); //Green OFF
        digitalWrite(6, HIGH); //Red ON

        // Send button press signal to Slave
        Wire.beginTransmission(8); // Address of the slave Arduino
        Wire.write('B');           // Send 'B' for button press
        Wire.endTransmission();

        delay(2000); 
        digitalWrite(7, HIGH);    
        digitalWrite(6, LOW);     
        delay(6000); 
      
        // Reset to initial state
        digitalWrite(7, LOW);     
        digitalWrite(6, HIGH);    
    }
}

//___________________________________________________________________________________________________________________________

//Slave

#include <Wire.h>

enum TrafficState { GREEN, YELLOW, RED_WITH_PEDESTRIAN_GREEN, YELLOW_AFTER_RED };

TrafficState currentState = GREEN;

void setup() { 
    Wire.begin(8); // Start I2C as slave with address 8
    Wire.onReceive(Receive); // Register Receive Signal

    pinMode(13, OUTPUT); // Red 
    pinMode(12, OUTPUT); // Yellow 
    pinMode(11, OUTPUT); // Green 
    // Start green 
    digitalWrite(13, LOW); 
    digitalWrite(12, LOW); 
    digitalWrite(11, HIGH);
}

void loop() { 
    switch (currentState) {
        case GREEN:
            // remains Green until a signal is received from The Master
            break;

        case YELLOW:
            digitalWrite(11, LOW);    
            digitalWrite(12, HIGH);   
            delay(2000);              
            currentState = RED_WITH_PEDESTRIAN_GREEN;
            break;

        case RED_WITH_PEDESTRIAN_GREEN:
            digitalWrite(12, LOW);   
            digitalWrite(13, HIGH);   
            delay(6000);              
            currentState = YELLOW_AFTER_RED; 
            break;

        case YELLOW_AFTER_RED:
           
            digitalWrite(13, LOW);    
            digitalWrite(12, HIGH);   
            delay(2000);             
            digitalWrite(12, LOW);    
            digitalWrite(11, HIGH);  
            currentState = GREEN;     
            break;
    }
}
//Buton
void Receive(int bytes) { 
    while (Wire.available()) { 
        char c = Wire.read(); 
        if (c == 'B') { 
            // Button pressed signal
            currentState = YELLOW; // Transition to yellow when button is pressed
        }
    }
}
