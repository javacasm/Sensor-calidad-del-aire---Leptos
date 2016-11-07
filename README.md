# Proyecto de estación de medida de calidad del aire

Basado en el  [Proyecto Leptos ](https://imvec.tech/leptos/) pero adaptado para utilizar componentes más baratos y poder hacer accesible desde internet

![2](https://imvec.tech/wp-content/uploads/2016/09/LeptosSteps001-1024x768.jpg)
(Imagen de los componentes del proeycto Leptos)

Se trata de un proyecto interesante y con una importante dimensión social.

Si existieran suficientes nodos de medida y todos ellos accesibles desde internet se podrían trazar mapas detallados de la calidad del aire.

Mi propuesta es usar un hardware más barato y con conexión wifi directa

* ESP8266 (NodeMCU)
* Sensor de gas MQ2
* Sensor PPD42 de partículas en suspensión.
* Sensor BME280 de temperatura humedad y presión.

La placa NodeMCU tiene una entrada analógica que usaríamos para leer el MQ2, 2 entradas digitales para leer el sensor PPD42 y un I2C o SPI

## [Componentes proyecto Leptos](./Componentes_Leptos.md)

## [Componentes proyecto con ESP8266](./Componentes_ESP8266.md)
