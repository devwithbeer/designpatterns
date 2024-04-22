# CHAIN OF RESPONSABILITY

## ¿QUÉ ES?
Es un patrón de diseño comportamental que permite pasar una solicitud a lo largo de una cadena de controladores "handlers", una vez un handler recibe una solicitud este decide si procesarla o pasarla al siguiente handler en la cadena. Esto permite a una aplicación encadenar objetos para procesar una solicitud sin conocer la identidad de los objetos en la cadena. Esto también permite a los objetos cambiar dinámicamente la cadena de responsabilidad.

## ¿CASOS DE USO?
- Servicio web con midleware
- Procesos secuenciales

## ¿QUÉ PROBLEMA RESUELVE?
El problema surge cuando se buscan implementar pasos de ejecución como por ejemplo en verificación antes de ingresar a un sistema, los pasos se pueden extender o incrementar de tal manera que el codigo se vuelve ilegible, no extensible y dificil de mantener.


## SOLUCION
El patrón busca transformar comportamientos o algortimos particulares en objetos independientes llamados "HANDLERS", cada eslabón de la cadena se extrae a su propia clase con un único metodo que realiza la función (como un check por ejemplo), la solicitud junto a la data se pasas a este metodo como argumento. El patrón sugiere que se unan estos eslabones como una cadena, cada handler que se une a esta tiene un campo para almacenar una referencia al siguiente handler en la cadena, asi además de procesar la solicitud la pasan mas adelante en la cadena. La solicitud para a lo largo de toda la cadena hasta que todos los handlers tienen la oportunidad de procesarla. La mejor parte del proceso es que un eslabon o handler puede decidir no pasar mas allá la solicitud y detener cualquier proceso siguiente. Es crucial que todos los handlers implementen la misma interfaz, cada handler concreto solo se preocupa por el siguiente y por tener el metodo "execute", de esta forma se puede componer una cadena en tiempo de ejecución usando varios handlers sin acoplar el código a sus implementaciones. 

## BENEFICIOS 
- Se puede controlar el orden de manejo de las solicitudes
- Promueve el principio de Single Responsability, se puede desacoplar clases que invoquen operaciones de clases que implementen operaciones
- Promueve el principio Open/Close ya que se pueden introducir nuevos handlers sin romper el código existente 

## DESVENTAJAS
- Algunas solicitudes pueden terminar sin manejo si no se implementan todas las opciones 

## EJEMPLO
```Dart
enum NavigationHandlerResult {
  handled, unhandled
}

abstract class NavigationHandler {
  NavigationHandlerResult handleNavigation(String url);
}

class EmailNavigationHandler implements NavigationHandler {
  @override
  NavigationHandlerResult handleNavigation(String url) {
    if(url.contains("email")){
      // open email app
      return NavigationHandlerResult.handled;
    }

    return NavigationHandlerResult.unhandled;
  }
}

class PhoneNavigationHandler implements NavigationHandler {
  @override
  NavigationHandlerResult handleNavigation(String url) {
    if(url.contains("phone")){
      // open email app
      return NavigationHandlerResult.handled;
    }

    return NavigationHandlerResult.unhandled;
  }
}

class NavigationCoordinator {

  final List<NavigationHandler> _handlers = [
    PhoneNavigationHandler(),
    EmailNavigationHandler()
  ];

  addNavigationHandler(NavigationHandler handler){
    _handlers.add(handler);
  }

  openScreen(String url){
    bool isNavigationHandled = false;
    
    for(NavigationHandler handler in _handlers){
        if(handler.handleNavigation(url) == NavigationHandlerResult.handled){
          isNavigationHandled = true;
          break;
        }
    }
    
    if(!isNavigationHandled){
      // Open Not Found screen
    }
  }
}
````