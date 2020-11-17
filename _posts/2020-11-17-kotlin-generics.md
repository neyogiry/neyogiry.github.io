---
layout: post
title: "Genéricos en Kotlin"
permalink: /kotlin-generics/
---

¿Alguna vez has escuchado sobre las clases genéricas?, ¿qué son y para qué sirven?
En este post vamos a explicar un poco sobre los genéricos en Kotlin y como podemos aprovecharlo para tener un código más flexible y reutilizable.

Vamos a tomar una situación simple como ejemplo.
Imagina que eres el encargado de administrar un centro de cuidado y atención de algunas diferentes especies de animales. Por el momento vamos a tener en cuenta a los animales y su alimentación. Empezemos con una clase `Dog` para el animal y `Feeding` para su alimentación. Creemos las entidades para ello. Puedes usar [Kotlin Playground](https://play.kotlinlang.org/) para validar el código.

```markdown
data class Dog(val id: Int, val name: String)
class Feeding(val dog: Dog, val water: Double, val food: Double)
```

Water y food son la cantidad de agua y comida que el animal necesita. Con nuestras entidades definidas podemos hacer uso de ellas de la siguiente manera:

```markdown
val dog = Dog(id = 0, name = "Doglin")
val feeding = Feeding(dog = dog, water = 2.0, food = 4.0)
```

Ello funciona bien. Pero al trabajar con muchos animales necesitamos ver la alimentación de cada uno de ellos. ¿Deberíamos crear un `CatFeeding` si necesitamos manejar la alimentación de un gato? Esto no es conveniente porque nos llevaría a tener código repetitivo.

### Parámetro de tipo
Kotlin, al igual que Java, permite que las clases tengan parámetros de tipo. Eso ayuda a mejorar la flexibilidad y reutilización de código. Veamos como quedaría nuestro código:

```markdown
class Feeding<T>(val animal: T, val water: Double, val food: Double)

val dog: Dog = Dog(id = 1, name = "Stu")
val cat: Cat = Cat(id = 4, name = "Mina")
val dogFeeding: Feeding<Dog> = Feeding(animal = dog, water = 2.0, food = 4.0)
val catFeeding: Feeding<Cat> = Feeding(animal = cat, water = 1.5, food = 3.0)
```

Hemos creado una clase `Feeding` de tipo `T`, un tipo genérico, donde podremos pasarle el tipo de animal con el que deseamos trabajar. Le indicamos la clase con la que vamos a trabajar y luego le pasamos un objeto de esa misma clase como parámetro. Genial, ya sabemos como utilizar tipo genérico como parámetro en una clase.

Pero, ¿qué tan seguro es?. Veamos si alguien hace lo siguiente:

```markdown
val stringFeeding: Feeding<String> = Feeding(animal = "Este animal es una cadena", water = 1.5, food = 3.0)
```

Esto es sintácticamente válido, pero no tiene sentido.
    
### Jerarquía de clases
Dos de los principales fundamentos de la programación orientada a objetos, la herencia y el polimorfismo, adquieren un papel protagonista al establecer una jerarquía de clases. Esto es particularmente importante para evitar el uso indebido del código como se describe anteriormente.
    
Imaginemos que las clases de `Dog`, `Cat`, etc heredan de una clase `Animal`. Entonces podemos hacer algo como esto:

```markdown
class Feeding<T : Animal>(val animal: T, val water: Double, val food: Double)
```

Esto nos asegura que la clase `Feeding` va a recibir un parámetro que hereda de la clase `Animal`. 

Si has llegado hasta aquí espero que te haya sido de ayuda este post. Más adelante hablaremos sobre **variance**, **covariant** y **contravariance**. Nos vemos la próxima.
    
