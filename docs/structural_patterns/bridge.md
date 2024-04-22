# BRIDGE

## ¿QUÉ ES?
El patrón de diseño Bridge se refiere a un patrón de estructura de software que divide la lógica de una aplicación en dos componentes separados, generalmente una interfaz y una implementación. Esta abstracción permite a los dos componentes cambiar de forma independiente sin afectarse mutuamente. Esto significa que una aplicación puede usar una misma interfaz para trabajar con diferentes implementaciones sin necesidad de cambiar código. El patrón bridge se usa a menudo para crear una conexión entre dos sistemas o para abstraer una lógica de una aplicación de su interfaz.

## CASOS DE USO
- Se puede usar en casos en los que una aplicación necesita cambiar lógicamente en tiempo de ejecución sin afectar a la interfaz. Esto se puede lograr proporcionando una abstracción entre la lógica de la aplicación y la interfaz de usuario. También se puede usar para permitir a una aplicación trabajar con diferentes implementaciones de algún componente sin necesidad de cambiar el código. Por ejemplo, se puede usar para conectar una aplicación a diferentes bases de datos sin necesidad de cambiar el código.

## ¿QUÉ PROBLEMA RESUELVE?

Resuelve el problema de la separación entre la lógica de una aplicación y su interfaz. Esto permite a una aplicación cambiar lógicamente sin afectar a la interfaz de usuario. También permite a una aplicación trabajar con diferentes implementaciones de algunos componentes sin necesidad de cambiar código. Esto permite a una aplicación ser más flexible y escalable.

## SOLUCIÓN
Se realiza la abstracción o **Interfase** la cual no hace un trabajo por si misma, esta delega el trabajo a la **Implementación** también llamada plataforma. Estos terminos no hacen referencia a clases abstractas o interfases de los lenguajes de programación, en una aplicación real la abstracción puede ser representada por una interfaz de usuario y la implementación es la API o el código del sistema subyacente 

# BENEFICIOS
- Crear clases y apps plataforma-independiente
- El código del cliente trabaja con alto nivel de abstracción 
- Promueve el principio Open/Close ya que permite agregar nuevas abstracciónes e implementaciónes independientes de cada una 
- Promueve el principio de responsabilidad única ya que permite enfocar lógica de alto nivel en la abstracción y en las plataformas detalles de implementación 

# DESVENTAJAS 
- Puede hacer el código más complicado en clases altamente acopladas

# EJEMPLO

ABSTRACCIÓN
Define operaciones de alto nivel basadas en operaciones primitivas, la abastracción define la interfaz de "control" hace referencia a un objeto de "implementación" y le delega todo el trabajo

```Dart

//Clase de abstracción
class RemoteControl{
  Device device;
  RemoteControl(this.device);

  volumeDown(){
    device.setVolume(device.getVolume() - 10);
  }

  volumeUp(){
    device.setVolume(device.getVolume() + 10);
  }

  channelDown(){
    device.setChannel(device.getChannel() - 1);
  }

  channelUp(){
    device.setChannel(device.getChannel() + 1);
  }

}
```
IMPLEMENTACIÓN, declara metodos comunes a todas las clases de implementación concretas, puede ser diferente a la clase de abstracción, provee solo operaciones primitivas

```Dart

//Clase para implementación
class Device{
  isEnabled(){}
  enable(){}
  disable(){}
  getVolume(){}
  setVolume(int percent){}
  getChannel(){}
  setChannel(int channel){}
}
```
```Dart

//Se pueden extender clases de la jerarquía de la abstracción
//independientes de la clase DEVICE
class AdvanceRemoteControl extends RemoteControl{
  AdvanceRemoteControl(super.device);

  mute(){
    device.setVolume(0);
  }
}
```
```Dart
//Todos los Devices siguen la misma interfaz, estos son las implementaciones
//concretas

class Tv implements Device{
  @override
  disable() {}
  @override
  enable() {}
  @override
  getChannel() {}
  @override
  getVolume() {}
  @override
  isEnabled() {}
  @override
  setChannel(int channel) {}
  @override
  setVolume(int percent) {}
}
```
```Dart

//En alguna parte del código se podría utilizar lo siguiente

  Tv tv = Tv();
  RemoteControl remote = RemoteControl(tv);
```