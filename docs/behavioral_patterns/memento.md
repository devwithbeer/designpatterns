# MEMENTO

## ¿QUÉ ES?
Es un patrón de diseño comportamental que permite salvar y restaurar estados previos de un objeto sin revelera los detalles de la su implementación, en otras palabras el patrón Memento permite capturar y almacenar el estado de un objeto de manera que pueda restaurarse posteriormente a ese mismo estado. Esto significa que el patrón Memento crea una copia del estado interno de un objeto en una instancia separada de memoria. Esta copia de memoria se llama memento. El patrón Memento se utiliza para permitir a los objetos revertir a un estado anterior sin violar el principio de encapsulación.

## ¿CASOS DE USO?
- Cuando se quiere almacenar estados de objetos para ser capaces de restaurarlos como en el caso de una función UNDO o en transacciónes cuando se quiere retroceder en un error de operación 
- Estados de la vista que requieran tener histórico como un editor de texto o editor gráfico 

## ¿QUÉ PROBLEMA RESUELVE?

El patrón Memento busca la posibilidad de que los objetos puedan revertir a un estado anterior sin violar el principio de encapsulación. Esto significa que los objetos internos de una clase no pueden ser accedidos directamente por ninguna otra clase.

 Esto ayuda a preservar la integridad del objeto y evita que otros objetos alteren su estado. El patrón Memento también permite que los objetos sean restaurados a un estado anterior sin tener que guardar toda la información sobre el estado. Esto significa que se pueden ahorrar muchos recursos al no tener que guardar toda la información sobre el estado de un objeto.

## SOLUCION
Memento permite a un objeto restaurar su estado anterior sin violar el principio de encapsulación lo cual se logra almacenando los estados internos del objeto como un objeto separado llamado "memento". El objeto memento se puede usar para restaurar el estado interno del objeto original. Este patrón delega la creación de snapshots de estado al propietario real de ese estado: el objeto de originator, por lo tanto en vez de crear copias del estado desde "afuera" la clase editor por si misma hace los snapshots ya que tiene acceso completo a su propio estado. Los mementos se almacenan en otras clases llamadas Caretakers las cuales no pueden alterar los estados dentro de los mementos, solo Originator tiene acceso a todos los campos dentro de memento permitiendole restaurar el estado previo a voluntad. 


## BENEFICIOS
- Se pueden crear copias de los estados sin violar el principio de encapsulación 
- Se puede simplificar el código del originator ya que permite al caretaker mantener la historia de los estados de originator

## DESVENTAJAS
- Consumo excesivo de RAM si los clientes crean mementos muy seguido 
- Caretakers deben seguir el ciclo de vida de originators para destruir mementos obsoletos 

## EJEMPLO
```Dart

//Clase que contiene los parametros del estado

import 'dart:ui';

class State{
  String name;
  int age;
  State(this.name, this.age);
}

//Clase Memento
class Memento{
  State state;
  Memento(this.state);
}

//Clase originator, define metodos para guardar estados en mementos y otro para
//restaurar estados a partir de mementos
// mantiene estados importantes que pueden cambiar en el tiempo

class Originator{
  State state;
  Originator(this.state);

  Memento makeMemento(){
    return Memento(State(state.name, state.age));
  }

  restore(Memento memento){
    state.name = memento.state.name;
    state.age = memento.state.age;
  }
}

//Clase Caretaker o cuidador, no depende de la clase memento

class Caretaker{
  Originator originator;
  Caretaker(this.originator);

  final List<Memento> _mementos = [];


  Memento getMemento(int index){
    return  _mementos[index];
  }

  saveMemento(Memento memento){
    _mementos.add(memento);
  }

  undo(){
  final lastMemento =   _mementos.removeLast();
  originator.restore(lastMemento);
  }
}

//Testing

void main(){

  final originator = Originator(State("Dev", 35));
  final caretaker = Caretaker(originator);

  Memento memento = originator.makeMemento();
  caretaker.saveMemento(memento);
  caretaker.undo();
}
```