# Tipos de Datos Estructurados

### 🧱 Tipos de Datos Estructurados

Los tipos de datos estructurados son construcciones que te permiten organizar y agrupar datos relacionados bajo una misma entidad. Son fundamentales para modelar conceptos complejos en tus contratos inteligentes.

* **Structs**: Estructuras que agrupan campos con nombre y tipos diferentes.
* **Enums**: Tipos que pueden contener uno de varios valores posibles predefinidos.
* **Tuplas**: Colecciones ordenadas y de tamaño fijo que pueden contener elementos de diferentes tipos.
* **Arrays**: Colecciones de elementos del mismo tipo con longitud fija.

#### 1. Structs 🏗️

Las **structs** en Rust te permiten agrupar campos con nombres y tipos diferentes. Es como crear tu propio "molde" para agrupar datos relacionados.

```rust
rustCopiarEditarstruct Persona {
    nombre: String,
    edad: u32,
}

fn main() {
    let persona = Persona {
        nombre: String::from("Juan"),
        edad: 30,
    };
    println!("{} tiene {} años 👤", persona.nombre, persona.edad);
}
```

***

#### 2. Enums 🎨

Los **enums** definen un tipo que puede ser uno de varios valores predefinidos. Son muy útiles para representar estados o variantes.

```rust
rustCopiarEditarenum Estado {
    Activo,
    Inactivo,
    Desconocido,
}

fn main() {
    let estado = Estado::Activo;
    match estado {
        Estado::Activo => println!("¡Está activo! 😊"),
        Estado::Inactivo => println!("Está inactivo. 😴"),
        Estado::Desconocido => println!("Estado desconocido. 🤔"),
    }
}
```

***

#### 3. Tuplas 🔗

Las **tuplas** son colecciones ordenadas de tamaño fijo que pueden contener elementos de diferentes tipos. ¡Son perfectas para agrupar datos relacionados sin necesidad de nombres!

```rust
rustCopiarEditarfn main() {
    let persona: (&str, u32) = ("Ana", 25);
    println!("{} tiene {} años 👩", persona.0, persona.1);
}
```

***

#### 4. Arrays 📚

Los **arrays** son colecciones de elementos del mismo tipo con una longitud fija. Se usan cuando conoces la cantidad exacta de elementos y estos son del mismo tipo.

```rust
rustCopiarEditarfn main() {
    let numeros: [i32; 5] = [1, 2, 3, 4, 5];
    println!("El primer número es: {} ", numeros[0]);
}
```
