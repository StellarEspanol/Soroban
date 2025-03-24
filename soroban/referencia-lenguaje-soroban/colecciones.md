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
