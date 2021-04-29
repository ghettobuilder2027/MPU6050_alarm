/*
 * SImple bike alarm
 * Calculate a jitt value based on the 6 axes
 */
#include "Wire.h"
#include <MPU6050_light.h> // https://github.com/rfetick/MPU6050_light

MPU6050 mpu(Wire);

long timer = 0;
// Angle values
float gx,gy,gz,hx,hy,hz,dx,dy,dz;
// Acceleration values
float ax,ay,az,bx,by,bz,cx,cy,cz;
// jitt is the overall movement
float jitt;

// warning movemement
bool warning = false;
bool warning2 = false;


// Alarm of any kind will go here
void beep() {
  if (!warning) {
      warning = true;
      // light beep
      
  } else if (!warning2){
    warning2 = true ;
    // bigger beep
  } else {
    // Super alarm 
    warning = warning2 = false;
  }
  
  
}

void setup() {
  gx=gy=gz=hx=hy=hz=dx=dy=dz=0;
  ax=ay=az=bx=by=bz=cx=cy=cz=0;
  Serial.begin(115200);

  //Initiate the MPU
  Wire.begin();
  byte status = mpu.begin();
  Serial.print(F("MPU6050 status: "));
  Serial.println(status);
  while(status!=0){ } // stop everything if could not connect to MPU6050
  
  Serial.println(F("Calculating offsets, do not move MPU6050"));
  delay(1000);
  mpu.calcOffsets(true,true); // gyro and accelero
  Serial.println("Done!\n");
  
}

void loop() {
  mpu.update();
  
  if(millis() - timer > 200){ // print data every second
    gx = hx;
    gy = hy;
    gz = hz;
    hx = mpu.getAngleX();
    hy = mpu.getAngleY();
    hz = mpu.getAngleZ();
    dx= hx-gx;
    dy=hy-gy;
    dz=hz-gz;
    bx=ax;
    by=ay;
    bz=az;
    ax=mpu.getAccX();
    ay=mpu.getAccY();
    az=mpu.getAccZ();
    cx=ax-bx;
    cy=ay-by;
    cz=az-bz;

    /*
    Serial.print("GX: ");Serial.print(dx);
    Serial.print("\tGY: ");Serial.print(dy);
    Serial.print("\tGZ: ");Serial.print(dz);
    Serial.print("\tAX: ");Serial.print(cx);
    Serial.print("\tAY: ");Serial.print(cy);
    Serial.print("\tAZ: ");Serial.println(cz);
    */
    timer = millis();
    if (timer>800){
      jitt = abs(dx)+abs(dy)+abs(dz)+abs(cx)+abs(cy)+abs(cz);
      Serial.println(jitt);
      if (jitt>1.5) {
        beep();
      }
    }
  }

}
