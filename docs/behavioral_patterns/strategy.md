# STRATEGY

## ¿QUÉ ES?

Es un patrón comportamental que permite definir una familia de algoritmos en clases separadas, encapsular cada uno y hacer sus objetos intercambiables, lo cual permite cambiar el comportamiento de una clase en tiempo de ejecución, además ayuda a desacoplar las clases que utilizan algoritmos de las implementaciónes de los mismos 

## ¿CASOS DE USO?
- Cuando necesitamos elegir un algoritmo en tiempo de ejecución entre varios.
- Cuando queremos que nuestro código sea extensible, permitiendo añadir nuevos algoritmos fácilmente sin cambiar el código existente.
- Cuando los algoritmos son intercambiables entre sí y necesitamos proporcionar una forma sencilla de cambiar entre ellos.
- Cuando los algoritmos se comportan de forma similar y queremos mantener una única interfaz para todos ellos.

## ¿QUÉ PROBLEMA RESUELVE?

El problema se origina cuando una clase quiere hacer algo específico en muchos sentidos y se quieren crear multiples algoritmos acoplandose a un código complicado que es de dificil mantenimiento, poco o nada extensible 

## SOLUCION
Se toma la clase acoplada con muchos maneras de cumplir una tarea especifica y se extrae cada una de estas maneras o algoritmos en clases separadas llamadas "strategies". La clase originar que se llama "context" debe tener un campo para almacenar una referencia a una de las "strategies"(Es una interfaz común para todos los algoritmos). El contexto delega el trabajo a un objeto de strategies en vez de hacerlo por si mismo. El contexto no es el responsable de seleccionar el algoritmo correcto, en vez de esto el cliente pasa el strategy deseado al contexto, el contexto solo expone un único método para activar el algoritmo encapsulado dentro de la estrategia seleccionada.

## BENEFICIOS
- Se pueden cambiar algoritmos en tiempo de ejecución
- Se puede aislar la implementación de un algoritmo del codigo que lo usa
- Promuevo el principio Open/close al permitir introducir nuevas caracteristicas o estrategias sin cambiar el contexto 

## DESVENTAJAS
- Si hay solo pocos algoritmos que en realidad no cambian mucho en el tiempo no hay necesidad de complicar el código con más clases e interfaces 
- Los clientes deben tener en cuenta las diferentecias entre las implementaciones de strategies para ser capaces de seleccionar el adecuado

## EJEMPLO
Primer escenario: Clase acoplada

```Dart
enum KeyboardType {
  number, currency, phone
}
class Keyboard {
  String value = "";
  KeyboardType type;

  Keyboard(this.type);

  addKey(String key){
    value += key;
  }

  String showValue(){
    switch(type){
      case KeyboardType.number: return value;
      case KeyboardType.currency: return "\$ $value";
      case KeyboardType.phone: return "+ $value";
    }
  }
}
```
Segundo escenario: Se crea una interfaz común a los diferentes metodos y la clase contexto no conoce cual se utilizara

```Dart
abstract class KeyboardFormatterStrategy{
  String format(String value);
}

class KeyboardPro {
  String value = "";
  KeyboardFormatterStrategy formatterStrategy;

  KeyboardPro(this.formatterStrategy);

  addKey(String key){
    value += key;
  }

  String showValue(){
    return formatterStrategy.format(value);
  }
}
// Implementaciones de la interfaz o strategies

class KeyboardPhoneFormatter implements KeyboardFormatterStrategy{
  @override
  String format(String value) {
    return "+ $value";
  }
}

class KeyboardCurrencyFormatter implements KeyboardFormatterStrategy{
  @override
  String format(String value) {
    return "\$ $value";
  }
}
```
Tercer escenario: utilización de interfaces más factory 

```Dart

// Clase Factory: mapa con keys y valores que son las implementaciones de la interfaz común, tiene los metodos para agregar strategies
// y un metodo para obtener el strategie con la key "type"

class KeyboardFormatterStrategyFactory {

  final Map<String, KeyboardFormatterStrategy> _strategies = {
    "phone": KeyboardPhoneFormatter(),
    "currency": KeyboardCurrencyFormatter()
  };
  KeyboardFormatterStrategy? getFormatter(String type){
    return _strategies[type];
  }
  addFormatter(String name, KeyboardFormatterStrategy strategy){
    _strategies[name] = strategy;
  }
}

//Clase contexto
// el metodo getFormatter obtiene la implementación a travez del type y le da formato a value 

class KeyboardPro2 {
  String value = "";
  final KeyboardFormatterStrategyFactory _factory;

  KeyboardPro2(this._factory);

  addKey(String key){
    value += key;
  }

  String showValue({String type = "number"}){
    return _factory.getFormatter(type)?.format(value) ?? "";
  }
}
```
Uso de los casos
```Dart

main(){
  // Primer caso - Rompe con el principio OC
  final keyboard = Keyboard(KeyboardType.phone);
  final value1 = keyboard.showValue();

  // Segundo Caso
  final strategy = KeyboardPhoneFormatter();
  final keyboard2 = KeyboardPro(strategy);
  final value2 = keyboard2.showValue();

  // Tercer Caso
  // Inicia la app - configuro los strategies
  final formattersFactory = KeyboardFormatterStrategyFactory();
  formattersFactory.addFormatter("decimal", KeyboardCurrencyFormatter());

  // Creamos el teclado y obtenemos el valor
  final keyboard3 = KeyboardPro2(formattersFactory);
  final value3 = keyboard3.showValue(type: "decimal");

}
```