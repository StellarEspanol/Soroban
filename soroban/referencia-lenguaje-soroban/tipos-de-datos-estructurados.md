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

**Contratos inteligentes ejemplo:**

En esta ocasión vamos a crear un contrato independiente por cada tipo de dato de la siguiente manera.

Abrimos la consola en la ruta donde deseamos crear el proyecto y ejecutamos.

```bash
stellar contract init structured_data_types --name structured_data_types
```

&#x20;Borramos todo el código y ponemos lo siguiente:

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, contracttype, Address, Env, String, };

#[contracttype]
#[derive(Clone, Debug, Eq, PartialEq)]
pub enum TaskStatus {
    Open = 0,
    InProgress = 1,
    Completed = 2,
}

#[contracttype]
#[derive(Clone, Debug, Eq, PartialEq)]
pub struct Task {
    pub id: u32,
    pub description: String,
    pub status: TaskStatus,
    pub assignee: Address,
}

#[contract]
pub struct SimpleContract;
#[contractimpl]
impl SimpleContract {
    // Función que busca una fruta en un array y devuelve su posición o -1 si no la encuentra.
    pub fn find_fruit(env: Env,fruit: String) -> i32 {
        let fruits: [String; 5] = [
            String::from_str(&env,"manzana"),
            String::from_str(&env,"banana"),
            String::from_str(&env,"naranja"),
            String::from_str(&env,"uva"),
            String::from_str(&env,"fresa"),
        ];

        for (index, f) in fruits.iter().enumerate() {
            if f == &fruit {
                // Compara la fruta dada con cada elemento del array.
                return index as i32; // Devuelve la posición (índice) si la encuentra.
            }
        }

        -1 // Devuelve -1 si la fruta no está en el array.
    }

 
    // Función que crea una tarea y devuelve el struct
   pub fn create_task(env: Env, id: u32, description: String, assignee: Address) -> Task {
        Task {
            id,
            description,
            status: TaskStatus::Open,
            assignee,
        }
    }
 
     // Función que devuelve una tupla con información
    pub fn get_info(env: Env) -> (String, i32) {
        (String::from_str(&env,"Ejemplo Simple"), 123)
    }

    // Función que devuelve la descripción del estado de la tarea
    pub fn get_status_description(env: Env,status: TaskStatus) -> String {
        match status {
            TaskStatus::Open => String::from_str(&env,"Abierta"),
            TaskStatus::InProgress => String::from_str(&env,"En Progreso"),
            TaskStatus::Completed => String::from_str(&env,"Completada"),
        }
    }
}
```

\


```
// Some code
```
