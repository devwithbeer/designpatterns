# SINGLETON

## ¿QUÉ ES?

El patrón singleton es un patrón creacional que permite asegurar que una clase tenga solamente una instancia durante la vida de la aplicación, mientras se provee acceso global a esta instancia.

## CASOS DE USO

- Inyección de dependencias
- Control de acceso a un recurso compartido por ejemplo base de datos o un archivo

## ¿QUÉ PROBLEMAS RESUELVE?

Resuelve dos problemas a la vez violando el principio de responsabilidad única 

1. Asegura que una clase solo tenga una instancia en el caso de que sea requerida.
2. Otorga un punto de acceso global a la instancia: a pesar de generar un acceso global, el patrón también proteje la instancia de ser reescrita por otro código

## SOLUCION

Todas las implementaciones de un Singleton tienen 2 pasos en común

1. Hacer un constructor privado que impide crear nuevas instancias 
2. Crear un metodo o variable estatica que actua como constructor, estos llaman al constructor privado para crear un objeto y guardarlo en un campo / variable estatica

## BENEFICIOS

- Se asegura que una clase tenga solamente una única instancia 
- Se otorga acceso global a esa instancia 
- El objeto singleton se inicializa solo cuando se solicita por primera vez

## DESVENTAJAS

- Viola el principio de responsabilidad única ya que resuelve dos problemas al tiempo
- Puede enmascarar mal diseño, por ejemplo los componentes saben demasiado unos de otros 
- Se requiere un manejo especial si hay multiples threads/ hilos, para que no se creen multiples instancias
- Presenta problemas para la realización de testing, ya que muchas pruebas pueden acceder al mismo objeto 

## EJEMPLO 

```dart
class Singleton{
  static late Singleton instance = Singleton._();

  Singleton._();

  int sumar (int a, int b){
    return a + b;
  }
}

// final singleton = Singleton(); No se puede construir directamente un objeto de la clase con constructor privado

final resultado = Singleton.instance.sumar(5, 3);
```