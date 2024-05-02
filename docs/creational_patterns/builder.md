# PATRÓN BUILDER 

## ¿QUÉ ES?

Es un diseño de creación el cual permite construir objetos complejos paso a paso, permite crear representaciones diferentes de un objeto usando el mismo código de construcción 

## CASOS DE USO

- Alert dialog android
- Constructores en dart
- Listview Builder flutter


## ¿QUÉ PROBLEMA RESUELVE?

El problema se presenta cuando hay que crear un objeto muy complejo paso a paso o propiedad por propiedad, más aún cuando estas existen en gran cantidad o cuando hay que hacer lógica o calculos para determinar el valor de las mismas. Dando como resultado un constructor enorme que podría ser ilegible y aún peor disperso entre el código del cliente.

?> En lenguajes como java donde no existen parametros opcionales o que tengan valores por defecto, este patrón es de muchísima utilidad, pero en lenguajes como kotlin o dart hay que analizar la lógica de creación para determinar la nacesidad de implementarlo


## SOLUCIÓN

La solución que ofrece el patrón Builder consiste en delegar el proceso de creación del objeto de su misma clase a objetos separados llamados "builder", donde se organiza la construcción paso a paso,  no es necesario cumplir con todos ellos, o en un orden especifico, solamente se llama a los pasos necesesarios para generar un objeto especifico de acuerdo a las necesidades. Así mismo se pueden crear diferentes builders con implementaciónes diferentes para la construcción de objetos personalizados pero con los mismos pasos.

> Usar patrón builder cuando se busque crear diferentes representaciónes de un mismo objeto, centralizando el proceso de construcción.



## BENEFICIOS

- Útil en la creación de objetos complejos con lógica que se puede centralizar dentro del builder haciendo el código reutilizable

- Aplicable si se necesitan crear objetos con valores por defecto a partir de clases externas como clases pertenecientes a librerias 

- Facilita creación de objetos con las propiedades que realmente se necesitan, calculando propiedades a partir de otras o con valores por defecto

- A través de interfaces podemos crear multiples builders para establecer los mismos pasos de creación de un objeto pero con diferente implementación.


## DESVENTAJAS

- Se incrementa la complejidad del código ya que necesita la creación de nuevas clases, metodos y estructuras de organización 

- A menos que el objeto sea muy complejo muchas de sus ventajas como reusabilidad, selección de pasos en la creación del objeto de acuerdo a la necesidad, establecer valores por defecto ya se realian en clases de lenguajes más nuevos y mas desarrollados como kotlin, dart sin la necesidad de establecer un patrón builder.

## EJEMPLO 

CREAR LA DATA CLASE 

```dart
enum PizzaSize { small, medium, large }

class Pizza {
  final bool cheese;
  final bool pepperoni;
  final bool tomato;
  final PizzaSize size;
  final bool pineapple;
  final int portions;

  Pizza(this.size, this.cheese, this.pepperoni, this.pineapple, this.tomato,
      this.portions);

  Pizza copy(
      {bool? cheese,
      bool? pepperoni,
      bool? tomato,
      PizzaSize? size,
      bool? pineapple,
      int? portions}) {
    return Pizza(
      size ?? this.size,
      cheese ?? this.cheese,
      pepperoni ?? this.pepperoni,
      pineapple ?? this.pineapple,
      tomato ?? this.tomato,
      portions ?? this.portions,
    );
  }

  static PizzaBuilder builder() => PizzaBuilder();
}
```

CREACION DE LA CLASE BUILDER 

```dart
class PizzaBuilder {
  late Pizza _pizza;

  PizzaBuilder() {
    _pizza = Pizza(
      PizzaSize.small,
      false,
      false,
      false,
      false,
      _getPortionsBySize(PizzaSize.small),
    );
  }

  int _getPortionsBySize(PizzaSize size) {
    switch (size) {
      case PizzaSize.small:
        return 4;
      case PizzaSize.medium:
        return 8;
      case PizzaSize.large:
        return 12;
    }
  }

  PizzaBuilder setCheese(bool cheese) {
    _pizza = _pizza.copy(cheese: cheese);
    return this;
  }
  PizzaBuilder setPepperoni(bool pepperoni) {
    _pizza = _pizza.copy(pepperoni: pepperoni);
    return this;
  }
  PizzaBuilder setTomato(bool tomato) {
    _pizza = _pizza.copy(tomato: tomato);
    return this;
  }
  PizzaBuilder setSize(PizzaSize size) {
    _pizza = _pizza.copy(size: size, portions: _getPortionsBySize(size));
    return this;
  }
  PizzaBuilder setPineapple(bool pineapple) {
    _pizza = _pizza.copy(pineapple: pineapple);
    return this;
  }
  Pizza build() {
    return _pizza;
  }
}


```

CREACIÓN DEL OBJETO A PARTIR DEL BUILDER

```dart
main(){

    final newPizza =
    Pizza.builder()
        .setCheese(true)
        .setPepperoni(true)
        .setPineapple(true)
        .setTomato(true)
        .setSize(PizzaSize.medium)
        .build();
}
````