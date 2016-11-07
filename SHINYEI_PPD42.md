# SHINYEI PPD42 sensor de particulas en suspensión

[Dónde Comprarlo](https://es.aliexpress.com/item/SHINYEI-dust-sensor-PPD42NS-PPD4NS-PPD42NJ-dust-sensor-with-cable/32305336628.html?spm=2114.13010608.0.0.BrP51G&detailNewVersion=&categoryId=523)

El valor de sensor se puede leer como  PWM (se puede leer como digital!! ) ([How to read PWM signal](http://www.benripley.com/diy/arduino/three-ways-to-read-a-pwm-signal-with-arduino/))

![](https://ae01.alicdn.com/kf/HTB1MSx0HpXXXXcIXVXXq6xXFXXX3/220667657/HTB1MSx0HpXXXXcIXVXXq6xXFXXX3.jpg?size=157456&height=750&width=1000&hash=430f15c763a3fbbfffd49bbbfc82cf10)

## Descripción (de la página del vendedor)

SHINYEI dust sensor PPD42NS PPD4NS PPD42NJ dust sensor with cable

Main features:

1. PWM mode output;
2. compact, light weight;
3. easy to install;
4. a single power supply;
5. low prices.

Main applications:

1. air purifiers and air cleaners;
2. air-conditioning;
3. air quality monitoring instrument;
4. air conditioners and other related products.

The main parameters:

1. the optical principles, capable of detecting dust particles of 1 micron or more;
2. two output modes, solve different sensitivity requirements, clean environment Vout outputs a high level signal (4V);
3. 5 VDC power supply
4. detecting particles range: up to 8000pcs / 283ml (1um more particles)

This Dust Sensor gives a good indication of the air quality in an environment by measuring the dust concentration. The Particulate Matter level (PM level) in the air is measured by counting the Low Pulse Occupancy time (LPO time) in given time unit. LPO time is proportional to PM concentration. This sensor can provide reliable data for air purifier systems; it is responsive to PM of diameter 1μm.

Note: This sensor uses counting method to measure dust concentration, not weighing method, and the unit is pcs/L or pcs/0.01cf.

Note: Please pay attention to the warnings listed here.

## Pinout

### ¡¡Cuidado que el cable que traen algunas placas induce a error por los colores!!

En esta imagen del sensor que vende seeedstudio con su conector Groove queda mucho más claro

![a](https://statics3.seeedstudio.com/images/product/Dustsensor.jpg)

1. GND
2. OUTPUT P2
3. Vcc: 5V 90mA
4. OUTPUT P1
5. INPUT (T1), threshold for P2


## Forma y cuidados de uso

### ¡¡ NO TOCAR LOS POTENCIÓMETROS!!! YA VIENEN CONFIGURADOS DE FÁBRICA

* Please keep it upright.
* 3 min preheat time is required while using for the first time.
* Arbitrary operation may cause unexpected damage.
* Following widgets (red rectangle marked) is used only for the factory setting. Please DO NOT change the default configuration.

## Ejemplo de código

Demos

Here is a demo to show how to obtain PM concentration data from this Grove - Dust Sensor.

1. Plug the dust sensor into digital port D8 on the Grove - Base Shield. It can only be D8 because the operation of this sensor involves sampling. This function only can be achieved by D8, the input capturing pin of ATmega328P, on Arduino/Seeeduino.
Also, you can connect Grove - Dust sensor to Arduino UNO without Base Shield:
Arduino UNO	Dust Sensor

  * 5V	Red wire
  * GND	Black wire
  * D8	Yellow wire

2. Copy and paste the demo code below to a new Arduino sketch.

        /*
        Grove - Dust Sensor Demo v1.0
         Interface to Shinyei Model PPD42NS Particle Sensor
         Program by Christopher Nafis
         Written April 2012

         http://www.seeedstudio.com/depot/grove-dust-sensor-p-1050.html
         http://www.sca-shinyei.com/pdf/PPD42NS.pdf

         JST Pin 1 (Black Wire)  => //Arduino GND
         JST Pin 3 (Red wire)    => //Arduino 5VDC
         JST Pin 4 (Yellow wire) => //Arduino Digital Pin 8

         */

        int pin = 8;
        unsigned long duration;
        unsigned long starttime;
        unsigned long sampletime_ms = 2000; //sampe 30s
        unsigned long lowpulseoccupancy = 0;
        float ratio = 0;
        float concentration = 0;

        void setup() {
          Serial.begin(9600);
          pinMode(8,INPUT);
          starttime = millis();//get the current time;
        }

        void loop() {
          duration = pulseIn(pin, LOW);
          lowpulseoccupancy = lowpulseoccupancy+duration;

          if ((millis()-starttime) >= sampletime_ms)//if the sampel time = = 30s
          {
            ratio = lowpulseoccupancy/(sampletime_ms*10.0);  // Integer percentage 0>100
            concentration = 1.1*pow(ratio,3)-3.8*pow(ratio,2)+520*ratio+0.62; // using spec sheet curve
            Serial.print("concentration = ");
            Serial.print(concentration);
            Serial.println(" pcs/0.01cf");
            Serial.println("\n");
            lowpulseoccupancy = 0;
            starttime = millis();
          }
        }


The result above consists of three parts: lowpulseoccupancy, ratio and concentration:

* "lowpulseoccupancy" represents the Low Pulse Occupancy Time(LPO Time) detected in given 30s. Its unit is microseconds.
* "ratio" reflects on which level LPO Time takes up the whole sample time.
* "concentration" is a figure that has a physical meaning. It is calculated from the characteristic graph below by using the LPO time.

![Characteristics.jpg ](http://wiki.seeedstudio.com/images/thumb/1/1f/Characteristics.jpg/600px-Characteristics.jpg)

## ¿Cómo funciona?

![inside](http://aqicn.org/aqicn/view/images/sensors/Shinyei-PPD42NS-inside.jpg)

Esencialmente se calienta el aire y se mide la cantidad de partículas que se mueven

![Cómo funciona](http://arduinoairpollution.altervista.org/wp-content/uploads/2016/03/Shinyei-PPD42NS-How-it-works.png)

## Documentación

* [Documentación seeedstudio](http://wiki.seeedstudio.com/wiki/Grove_-_Dust_sensor)
* [DataSheet](http://www.seeedstudio.com/wiki/images/4/4c/Grove_-_Dust_sensor.pdf)
* [Cómo funciona](http://takingspace.org/wp-content/uploads/ShinyeiPPD42NS_Deconstruction_TracyAllen.pdf)
* [Wiki sobre el sensor](http://wiki.timelab.org/wiki/PPD42NS)
* [Datasheet del fabricante](http://wiki.timelab.org/images/f/f9/PPD42NS.pdf)
* [Proyecto que lo usa](https://hackaday.io/project/11339-particulate-matter-sensor-network)
