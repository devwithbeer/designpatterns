# PATRÓN FACTORY METHOD

## ¿QUÉ ES?

Factory Method es un patron creacional, en el cual se tiene una clase abstracta "objeto" cuya implementación da origen a objetos de la misma "objeto1, objeto2.. etc", sin embargo se delega la creación de estos objetos a otra clase abstracta "factory" cuya implementación "factory1" crea objetos "objeto1", implementación "factory2" crea objetos "objeto2" ... etc.

## CASOS DE USO

- Adaptar una vista en flutter de a cuerdo al sistema operativo 
- Inyección de dependencias

## ¿QUE PROBLEMA RESUELVE?

El problema se presenta cuando el código se acopla directamente a objetos instanciados de una clase abstracta y en algún momento es necesario modificar la cantidad o tipo de estos objetos por lo que hay que cambiar todo el código haciendo esto tedioso y poco o nada extensible 

## SOLUCIÓN

La solución que plantea el patrón es la delegación de la creación de los objetos a otra clase abstracta cuyas implemntaciónes son las que crean objetos concretos de la clase abstracta "objeto", asi la clase abstracta "factory" crea objetos pero no sabe exactamente cual, haciendo que el proceso de creación no dependa especificamente de los objetos 

## BENEFICIOS

- Se evita el acoplamiento entre el creador y los productos concretos 
- Se delega el proceso de creación a una clase concreta facilitando su manejo y soporte practicando el pricipio SOLID Single Responsability
- Principio Open/Close ya que se pueden introducir nuevos tipos de productos sin dañar el código existente
- Proporciona una estructura flexible para la creación de objetos
- Separa la creación de objetos de la lógica de negocios

## DESVENTAJAS

- El patrón se puede hacer complicado si se hacen muchas subclases con estructura complicada
- Puede ser confuso para las personas que no están familiarizadas con este patrón 


## EJEMPLO

```dart

//Producto clase abstracta
abstract class Button{
  onClick();
  Widget render ();
}


//Producto 1 
class AndroidButton implements Button{

  @override
  onClick() {
    // TODO: implement onClick
    throw UnimplementedError();
  }

  @override
  Widget render() => ElevatedButton(onPressed: onClick, child: const Text("ok"));
}


//Producto 2 
class IosButton implements Button{
  @override
  onClick() {
    // TODO: implement onClick
    throw UnimplementedError();
  }

  @override
  Widget render() => CupertinoButton(onPressed: onClick, child: const Text("ok"));
}
```
Delegación de la creación de los objetos a una clase Factory

```dart
//Fabrica
abstract class Dialog{
  Widget render(BuildContext context){
    return Column(children: [
      const Text("dialog"),
      createButton().render()

    ],);
  }
  Button createButton();
}

//Fabrica 1
class IosDialog extends Dialog{

  @override
  Button createButton() => IosButton();


}

//Fabrica 2
class AndroidDialog extends Dialog{
  @override
  Button createButton() => AndroidButton();

}
```
Se crea un objeto de factory al que se le asigna el producto 

```dart
main(){
  final Dialog dialog = AndroidDialog();
  dialog.render(context);
}
```
