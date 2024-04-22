# FACADE

## ¿QUÉ ES?

El patrón Facade es un patrón de diseño estructural que proporciona una interfaz simple para interactuar con una API o cualquier otro conjunto de clases complejo

## CASOS DE USO
- Uso de librerias 

## ¿QUÉ PROBLEMA RESUELVE?
Cuando se usa una estructura compleja de clases como una libreria existe un problema al adaptarla a nuestro código ya que se puede hacer muy complejo la instancia de todas esas clases.

## SOLUCION
Facade proporciona una interfaz simple a través de la cual se utilizan solo los metodos o clases que realmente necesitamos de la librería compleja para así adaptarla a nuestra aplicación 

## BENEFICIOS
- Permnite aislar el código de una libreria compleja sin necesidad de instanciar cada componente de la misma 

## DESVENTAJAS 
- Puede conertirse en un objeto "Dios" el cual este acoplado a muchas o todas las clases de una aplicación 

## EJEMPLO

Libreria con multiples clases y metodos 

```Dart

class Firebase{
  configFirebase(){}
}
class GoogleCloudPlatform{
  configGoogleCloudPlatform(){}
}
class GraphQL{
  configGraphQL(){}
}
```

Facade que presenta una interfaz para acceder a esa libreria 

```Dart

//La fachada facilita el uso de las libreria 

class Facade {
  late Firebase _firebase;
  late GoogleCloudPlatform _gcp;
  late GraphQL _graphql;

  Facade(){
    _firebase = Firebase();
    _gcp = GoogleCloudPlatform();
    _graphql = GraphQL();
  }

  makeDeliverApp(){
    _firebase.configFirebase();
    _gcp.configGoogleCloudPlatform();
    _graphql.configGraphQL():
  }
}

void main(){
  Facade facade = Facade();
  facade.makeDeliverApp();
}
```
