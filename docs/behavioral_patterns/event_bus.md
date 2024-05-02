# Event Bus

## ¿QUÉ ES?
El patrón de diseño Event Bus proporciona un mecanismo para la comunicación entre componentes de software sin la necesidad de conocer los detalles de la implementación. Esto se logra mediante la publicación de eventos en un bus (también llamado bus de mensajería). Los componentes pueden publicar, subscribirse y recibir los eventos publicados. Esto permite a los componentes comunicarse entre sí de forma asíncrona, sin que los componentes estén acoplados entre sí. En otras palabras un bus de eventos es un mecanismo mediante el cual los componentes pueden emitir eventos sin saber quién los va a recibir y recibir eventos sin saber quién los emite.

## ¿CASOS DE USO?
- Comunir distintos componentes en toda la aplicación

## ¿QUÉ PROBLEMA RESUELVE?
Al igual que observer este patrón interviene en la necesidad de verificar la presencia de cambios en un objeto determinado o cambios en los datos de un servicio o estados sin que se deba establecer un tiempo determinado de verficiación

## SOLUCION
El patrón Event Bus permite a los objetos suscribirse a ciertos eventos del Bus, de modo que cuando un evento es publicado en el Bus, todos los objetos suscritos reciben la notificación. Funciona como una línea de autobús, permitiendo que los componentes se conecten y se comuniquen entre sí sin tener que estar conectados directamente. Los componentes publican eventos al event bus y los otros componentes que se suscriben a ese evento recibirán una notificación. Los eventos pueden incluir cualquier cosa desde cambios en el estado de una aplicación hasta la finalización de una tarea. Esto permite a los componentes estar desacoplados y trabajar de manera independiente, lo que hace que la aplicación sea más escalable y mantenible.

## BENEFICIOS 
- Un componente puede emitir un evento escuchado por otro no relacionado jerarquicamente 
- Mayor desacople de los componentes, en particular en lo relativo a eventos

## DESVENTAJAS
- Una mala documentación de una app que creció mucho con este patrón puede hacer que un nuevo desarrollador tenga una mala curva de aprendizaje al no estar familiarizado con el código 
- Es dificil seguir el código ya que muchas veces no estan explicito 
- Es dificil testear el event bus y no se relacióna bien con los test unitarios

## EJEMPLO
```Dart

typedef Subscription = Function(dynamic);

class EventBus {

  final Map<String, Set<Subscription>> _subscriptions = {};

  subscribe(String topic, Subscription subscription) {
    final subscriptionsByTopic = _subscriptions[topic];
    if (subscriptionsByTopic == null){
      _subscriptions[topic] = { subscription };
      return;
    }
    subscriptionsByTopic.add(subscription);
  }

  unsubscribe(String topic, Subscription subscription) {
    final subscriptionsByTopic = _subscriptions[topic];
    subscriptionsByTopic?.remove(subscription);
  }

  dispatch(String topic, dynamic data){
    final subscriptionsByTopic = _subscriptions[topic];
    subscriptionsByTopic?.forEach((subscription) {
      subscription(data);
    });
  }
}

void main(){

  final eventBus = EventBus();

  eventBus.subscribe("notificaciones", (data){
    print(data);
  });

  eventBus.subscribe("internet_connection", (data){
    print(data);
  });

  eventBus.subscribe("notificaciones", (data){
    print(data);
  });

  eventBus.dispatch("notificaciones", 123);
}
```