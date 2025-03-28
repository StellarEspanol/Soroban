# Colecciones

### 📚 Colecciones

Las colecciones son estructuras de datos que te permiten almacenar y manipular múltiples valores. En Soroban, están optimizadas para el entorno blockchain y difieren de las implementaciones estándar de Rust.

* **Vec (soroban\_sdk::vec::Vec)**: Implementación propia de Soroban para vectores dinámicos.
* **Map (soroban\_sdk::map::Map)**: Implementación propia de Soroban para colecciones de pares clave-valor.

#### 1. Vec en Soroban 🌟

El **Vec** en Soroban es una colección dinámica que permite almacenar una secuencia de elementos de un mismo tipo. Al igual que en Rust, su tamaño puede crecer de forma dinámica. En el contexto de Soroban se trabaja con él a través del entorno (env), lo que facilita su integración en contratos inteligentes.

```rust
use soroban_sdk::{Env, Vec};

pub fn ejemplo_vec(env: &Env) {
    let mut numeros = Vec::new(env); // Crea un Vec vacío
    numeros.push(10);
    numeros.push(20);
    // Puedes registrar el contenido usando el log del entorno
    env.log().info("Contenido del Vec:", numeros);
}
```

***

#### 2. Map en Soroban 🔄

El **Map** en Soroban es una colección de pares clave-valor, muy útil para asociar claves únicas a valores, similar a un diccionario. También requiere el entorno (env) para su creación y manipulación, lo que lo hace ideal para el manejo de datos en contratos inteligentes.

```rust
use soroban_sdk::{Env, Map};

pub fn ejemplo_map(env: &Env) {
    let mut mi_mapa = Map::new(env); // Crea un Map vacío
    mi_mapa.set(1, "uno");
    mi_mapa.set(2, "dos");
    // Registra el contenido del Map
    env.log().info("Contenido del Map:", mi_mapa);
}
```

**Contratos inteligentes ejemplo:**

Abrimos la consola en la ruta donde deseamos crear el proyecto y ejecutamos.

```bash
stellar contract init collections --name vecmap
```

Borramos todo el código y ponemos lo siguiente:

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, symbol_short, Env, Map, String, Symbol, Vec};

#[contract]
pub struct VecMapContract;

#[contractimpl]
impl VecMapContract {
    const VEC_KEY: Symbol = symbol_short!("vec");
    const MAP_KEY: Symbol = symbol_short!("map");

    fn get_vec(env: &Env) -> Vec<String> {
        env.storage()
            .instance()
            .get(&Self::VEC_KEY)
            .unwrap_or(Vec::new(&env))
    }

    fn set_vec(env: &Env, vec: &Vec<String>) {
        env.storage().instance().set(&Self::VEC_KEY, vec);
    }

    // Funciones para manejo de Vec
    pub fn add_vec(env: Env, value: String) -> u32 {
        let mut my_vec: Vec<String> = Self::get_vec(&env);
        my_vec.push_back(value);
        Self::set_vec(&env, &my_vec);
        my_vec.len()
    }

    //Obtenemos un elemento del vector
    pub fn get_vec_element(env: Env, index: u32) -> Option<String> {
        let my_vec: Vec<String> = Self::get_vec(&env);
        my_vec.get(index)
    }

    pub fn remove_vec_element(env: Env, index: u32) -> Option<()> {
        let mut my_vec = Self::get_vec(&env);
        // Verificamos primero si el índice es válido
        if my_vec.get(index).is_some() {
            let element: Option<()> = my_vec.remove(index);
            Self::set_vec(&env, &my_vec);
            Some(())
        } else {
            None
        }
    }

    fn get_map(env: &Env) -> Map<Symbol, String> {
        env.storage()
            .instance()
            .get(&Self::MAP_KEY)
            .unwrap_or(Map::new(env))
    }

    fn set_map(env: &Env, map: &Map<Symbol, String>) {
        env.storage().instance().set(&Self::MAP_KEY, map);
    }

    // Funciones para manejo de Map
    
    // Agregamos un elemento al map
    pub fn add_map_entry(env: Env, key: Symbol, value: String) {
        let mut my_map = Self::get_map(&env);
        my_map.set(key, value);
        Self::set_map(&env, &my_map);
    }
    //Obtener algun valor del map
    pub fn get_map_value(env: Env, key: Symbol) -> Option<String> {
        let my_map = Self::get_map(&env);
        my_map.get(key)
    }

    // Eliminar un elemento del map
    pub fn remove_map_entry(env: Env, key: Symbol) -> Option<()> {
        let env_ref = &env;
        let mut my_map = Self::get_map(env_ref);
        if my_map.remove(key).is_some() {
            Self::set_map(&env, &my_map);
            Some(()) // Éxito
        } else {
            None     // La clave no existía
        }
    }
}
```

## Explicación del contrato

**Estructuras y Tipos del Contrato**

1. **Atributo `#![no_std]`**\
   Indica que el contrato no utiliza la biblioteca estándar de Rust, común en entornos embebidos o contratos inteligentes para reducir dependencias y adaptarse a restricciones de recursos.
2. **Macros de Contrato**\
   `#[contract]` y `#[contractimpl]`\
   Estas macros, proporcionadas por el _soroban\_sdk_, se utilizan para:

* **`#[contract]`**: Definir la estructura principal del contrato.
* **`#[contractimpl]`**: Implementar la lógica de negocio del contrato.\
  Para más detalles, consulta la documentación de [contratos Soroban](https://soroban.stellar.org/docs/developers/writing-contracts).

3. **Constantes `VEC_KEY` y `MAP_KEY`**\
   Definen claves simbólicas utilizadas para identificar y acceder a las estructuras de datos `Vec` y `Map` en el almacenamiento del contrato.

***

🛠 **Funciones del Contrato**

1️⃣ **Funciones Auxiliares para `Vec`**

* **`fn get_vec(env: &Env) -> Vec<String>`**\
  Recupera el vector almacenado en el contrato. Si no existe, retorna un vector vacío.
* **`fn set_vec(env: &Env, vec: &Vec<String>)`**\
  Almacena el vector proporcionado en el contrato.

2️⃣ **Funciones Públicas para `Vec`**

* **`pub fn add_vec(env: Env, value: String) -> u32`**\
  Agrega un elemento al final del vector y devuelve la nueva longitud del mismo.
* **`pub fn get_vec_element(env: Env, index: u32) -> Option<String>`**\
  Recupera el elemento en la posición especificada del vector. Devuelve `None` si el índice es inválido.
* **`pub fn remove_vec_element(env: Env, index: u32) -> Option<()>`**\
  Elimina el elemento en la posición especificada del vector. Devuelve `Some(())` si la operación es exitosa, o `None` si el índice es inválido.

3️⃣ **Funciones Auxiliares para `Map`**

* **`fn get_map(env: &Env) -> Map<Symbol, String>`**\
  Recupera el mapa almacenado en el contrato. Si no existe, retorna un mapa vacío.
* **`fn set_map(env: &Env, map: &Map<Symbol, String>)`**\
  Almacena el mapa proporcionado en el contrato.

4️⃣ **Funciones Públicas para `Map`**

* **`pub fn add_map_entry(env: Env, key: Symbol, value: String)`**\
  Agrega una entrada al mapa con la clave y valor proporcionados.
* **`pub fn get_map_value(env: Env, key: Symbol) -> Option<String>`**\
  Recupera el valor asociado a la clave especificada en el mapa. Devuelve `None` si la clave no existe.
* **`pub fn remove_map_entry(env: Env, key: Symbol) -> Option<()>`**\
  Elimina la entrada correspondiente a la clave especificada en el mapa. Devuelve `Some(())` si la operación es exitosa, o `None` si la clave no existía.

***

📌 **Resumen General**\
Este contrato inteligente, desarrollado con el _soroban\_sdk_, demuestra cómo manejar estructuras de datos dinámicas como `Vec` y `Map` en un entorno sin la biblioteca estándar de Rust. Proporciona funciones para agregar, recuperar y eliminar elementos de estas estructuras, almacenándolas de manera persistente en el contrato. Esta implementación es útil para gestionar colecciones de datos en contratos inteligentes en la blockchain de Stellar.

