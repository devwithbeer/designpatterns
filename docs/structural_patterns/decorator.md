# DECORATOR

## ¿QUÉ ES?

Es un patrón de diseño estructural que permite añadir comportamientos o funcionalidades  a objetos existentes colocando estos en otros envolventes. Es decir proporciona una forma de envolver un objeto para proporcionar un comportamiento adicional sin alterar la estructura del objeto original.   

## CASOS DE USO
- Personalizar objetos de interfas de usuario 
- Implementar nuevas funcionalidades a objetos de la capa de dominio

## ¿QUÉ PROBLEMA RESUELVE?

El patrón decorator permite añadir comportamiento adicional a un objeto de manera dinámica sin tener que alterar su código fuente subyacente. Esto permite a los desarrolladores añadir características a los objetos existentes en tiempo de ejecución, lo que es útil para la reutilización de código.

## SOLUCION

Extender una clase sería la solución que viene a la mente cuando se plantea alterar el comportamiento de un objeto, pero la herencia tiene dos problemas a tener en cuenta: La herencia es estatica es decir no se puede alterar el comportamiento de un objeto en tiempo de ejecuión solo reemplaza un objeto completo por otro creado en una subclase diferente. Y las subclases en la mayoria de lenguajes solo pueden tener una clase padre, es decir una subclase no puede heredar comportamientos de multiples clases al mismo tiempo. 

Por lo anterior la solución es utilizar Agregación o Composición sobre la herencia. Las dos funcionan igual: Un objeto hace referencia a otro y le delega un trabajo, asi un objeto puede usar el comportamiento de varias clases teniendo referencia a multiples objetos 

## BENEFICIOS 
- Se puede extender el comportamiento de un objeto sin hacer nuevas subclases 
- Se puede agregar o remover responsabilidades de un objeto en tiempo de ejecución 
- Se puede cambiar algunos comportamientos envolviendo un objeto en multiples decorators

## DESVENTAJAS
- Es dificil remover un decorator especifico en un stack
- Es dificil implementar decorators ya que su comportamiento no depende del orden del stack 

## EJEMPLO 

Clase base

```Dart
class Notifier {
  
   sendNotification(){
     print("Notificado por Email");
   }
  }
```
Decorator
```Dart
class SMSNotifier extends Notifier {
  
  Notifier _notifier;
  
  SMSNotifier(this._notifier);
  
  @override
  sendNotification(){
    _notifier.sendNotification();
    print("Notificado por SMS");
    
  }
}

class WSNotifier extends Notifier {
  
  Notifier _notifier;
  
  WSNotifier(this._notifier);
  
  @override
  sendNotification(){
    _notifier.sendNotification();
    print("Notificado por WS");
    
  }
}
```
Uso
```Dart
final base = Notifier();

final sms = SMSNotifier(base);

final total = WSNotifier(sms);

main(){
  print("BASE");
  base.sendNotification();
  
  print("SMS");
  sms.sendNotification();
  
  print("TOTAL");
  total.sendNotification();
  
}
```

