# SHINYEI PPD42 sensor de particulas en suspensión

[Dónde Comprarlo](https://es.aliexpress.com/item/SHINYEI-dust-sensor-PPD42NS-PPD4NS-PPD42NJ-dust-sensor-with-cable/32305336628.html?spm=2114.13010608.0.0.BrP51G&detailNewVersion=&categoryId=523)

El valor de sensor se puede leer como  PWM (se puede leer desde un pin digital!! ) ([How to read PWM signal](http://www.benripley.com/diy/arduino/three-ways-to-read-a-pwm-signal-with-arduino/))

![](https://ae01.alicdn.com/kf/HTB1MSx0HpXXXXcIXVXXq6xXFXXX3/220667657/HTB1MSx0HpXXXXcIXVXXq6xXFXXX3.jpg?size=157456&height=750&width=1000&hash=430f15c763a3fbbfffd49bbbfc82cf10)

## Descripción (de la página del vendedor)

Características:

1. Salida PWM
2. Compacto y ligero
3. Fácil de instalar
4. Sólo necesita un tipo de alimentación
5. Barato

Aplicaciones:

1. Purificadores de aire
2. Aire acondicionados
3. Instrumentos de medida de calidad del aire


Datos a tener en cuenta:

1. Usa medidas ópticas es capaz de detectar partículas de 1 micra o más
2. Utiliza dos salidas distintes para distinta sensibilidad
3. 5 VDC power supply
4. Detecta hasta 8000pcs / 283ml

La medición se hace contando el tiempo que las partículas ocupan determinada posición (donde se corta un haz de luz infrarroja) LOW Pulse Occupancy time (LPO time). LPO time es proporcional a la concentración PM.

Nota: El sensor usa un método de conteo, no de peso como otros sensores, por tanto la unidad es  pcs/L o pcs/0.01cf.


## Pinout

### ¡¡Cuidado que el cable que traen algunas placas induce a error por los colores!!

En esta imagen del sensor que vende seeedstudio con su conector Groove queda mucho más claro

![a](https://statics3.seeedstudio.com/images/product/Dustsensor.jpg)

1. GND
2. Output P2
3. Vcc: 5V 90mA
4. Output P1
5. Input (T1), threshold for P2


## Forma y cuidados de uso



* Mantener el sensor en la posición indicada hacia arriba.
* Se necesitan 3 minutos de calentamiento antes de la primera medida.
* ¡¡ NO TOCAR LOS POTENCIÓMETROS!!! YA VIENEN CONFIGURADOS DE FÁBRICA


## Ejemplo de código


1. Conectamos el sensor a la entrada D8

2. Este es el código del ejemplo

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


Obetenemos 3 resultados: lowpulseoccupancy, ratio and concentration:

* "lowpulseoccupancy" representa el Low Pulse Occupancy Time(LPO Time) detectado en  30s. Su unidad son los microsegundos.
* "ratio" refleja en qué nivel LPO Time se llegó al tiempo de espera.
* "concentration" es una cifra con sentido físico. Se calcula a partir de la gráfica del fabricante para LPO time.

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
* [Otro proyecto que lo usa](http://irq5.io/2013/07/24/testing-the-shinyei-ppd42ns/)
* [Proyecto con ESP8266](http://arduinoairpollution.altervista.org/progetto/)
* [Proyecto](http://www.takingspace.org/make-your-own-aircasting-particle-monitor/)
