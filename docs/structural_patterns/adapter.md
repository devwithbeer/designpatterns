  # ADAPTER

## ¿QUÉ ES?

Es un patrón de diseño estructural que permite colaborar a objetos con diferentes o incompatibles interfaces, permite interactuar a dos clases 

## CASOS DE USO
- Adapter de ReciclerView - Datos a views
- Adapter entre capas - Entidades a modelos, modelos a presentación enum - texto

## ¿QUÉ PROBLEMA RESUELVE?

El problema se presenta cuando un código está adaptado a un modelo específico y se decide cambiar el mismo o que se relacióne este con otros que tengan propiedades o tipos diferentes

## SOLUCIÓN

El patrón adapter permite relacionar dos clases diferentes sin necesidad de cambiar todo el código ya que funciona como un intermediario que convierte un modelo a otro haciendolo extensible 

## BENEFICIOS
- Facilita el principio de responsabilidad única ya que se aisla el código de conversión de la lógica de negocios 

- Facilida el principio Open/Close, ya que se pueden introducir nuevos adapters sin dañar o modificar el código base 

## DESVENTAJAS

- La complejidad del código aumenta ya que es necesario un entramado de interfases o clases 

## EJEMPLO

```Dart
class Model {
  String name;
  String address;
  Model(this.name, this.address);
}
```
ADAPTER
```Dart
class ModelToJson{

 static Map<String, dynamic> toJson(Model model){
    return {
      "name": model.name,
      "address": model.address
    };
  }
}
```
```Dart
Model example = Model("Sofia", "USA 005201");
final json = ModelToJson.toJson(example);

```