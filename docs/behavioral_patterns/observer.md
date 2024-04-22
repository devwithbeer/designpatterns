# OBSERVER

## ¿QUÉ ES?

Es un patrón de diseño comportamental que permite definit un mecanismo de subscripción para notificar a multiples objetos acerca de cualquier evento que le suceda al objeto que están observando 

## ¿CASOS DE USO?

- Observar cambios desde bases de datos 
- Observar estados de interfaz de usuario

## ¿QUÉ PROBLEMA RESUELVE?

El problema que resuelve rádica en la necesidad de verificar la presencia de cambios en un objeto determinado o cambios en los datos de un servicio sin que se deba establecer un tiempo determinado de verficiación. Se debe establecer un metodo de escucha ante los cambios de estado por lo que se establecería una relación uno a muchos. 

## SOLUCION

El objeto del cual se tiene interes usualmente se llama SUJETO, pero este objeto tambien notificara sobre sus cambios de estado por lo que se denomina también PUBLISHER. Todos los objetos que hacen seguimiento a los cambios en el estado del publisher se denominan SUBSCRIBERS

EL patrón observer sugiere que se agregue un mecanismo de subscripción a la clase Publisher, asi los objetos pueden subscribirse o desubscribirse de un evento de stream que venga del Publisher. Este mecanismo consiste en un campo array para almacenar la lista de referencias de subscriptores y algunos metodos públicos que permitan agregar y remover los subscriptores. Entonces cuando un evento de interes le pasa al Publisher, este verifica sus subscriptores y llama al metodo de notificación específico.

Es crucial que todos los subscriptores implementen la misma interfaz y que el Publisher se comunique con ellos a traveés de esa interfaz. Además esta debe declarar el metodo de notificación con un conjunto de parametros que el Publisher puede usar para pasar datos junto a la notificación.

## BENEFICIOS 
- Favorece el principio Open/Close: se puede instroducir nuevos subscriptores sin tener que cambiar el código del Publisher 
- Se puede establecer relaciones entre objetos en tiempo de ejecución 

## DESVENTAJAS
- Los subscriptores se notifican en orden aleatorio

## EJEMPLO

```Dart
typedef TemperatureListener = Function(double);

class Temperature {
  double _temperature = 0;
  set temperature(double value){
    _temperature = value;
    for (var listener in listeners) {
      listener(value);
    }
  }

  Set<TemperatureListener> listeners = {};

  addTemperatureListener(TemperatureListener listener){
    listeners.add(listener);
  }

  removeTemperatureListener(TemperatureListener listener){
    listeners.remove(listener);
  }
}

void main(){

  final temperature = Temperature();
  temperature.temperature = 1;

  temperature.addTemperatureListener((value){
    print(value);
  });

  temperature.temperature = 10;
  temperature.temperature = 17;

}
```