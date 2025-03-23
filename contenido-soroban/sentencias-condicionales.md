# Sentencias condicionales

### 📚 **Teoría sobre `if`, `else` y `match` en Rust**

Rust ofrece dos estructuras de control clave para la toma de decisiones: **if-else** y **match**.

#### ✅ **`if-else`**

Se usa para **tomar decisiones en función de una condición booleana**.

📌 **Sintaxis básica:**

```rust
rustCopiarEditarif condicion {
    // Código si la condición es verdadera
} else {
    // Código si la condición es falsa
}
```

📌 **Ejemplo en Rust:**

```rust
rustCopiarEditarlet num = 10;
if num > 0 {
    println!("El número es positivo");
} else {
    println!("El número es negativo");
}
```

📌 **Cuándo usar `if-else`**

* Cuando se necesita verificar una condición **booleana única**.
* Cuando solo hay **dos opciones** (`true` o `false`).

***

#### ✅ **`match`**

Se usa para **comparar un valor con múltiples posibles casos**, similar a `switch` en otros lenguajes.

📌 **Sintaxis básica:**

```rust
rustCopiarEditarmatch variable {
    valor1 => { /* Código para valor1 */ },
    valor2 => { /* Código para valor2 */ },
    _      => { /* Código por defecto */ },
}
```

📌 **Ejemplo en Rust:**

```rust
rustCopiarEditarlet rol = "Admin";
match rol {
    "Admin" => println!("Acceso total"),
    "User"  => println!("Acceso limitado"),
    "Guest" => println!("Solo lectura"),
    _       => println!("Rol no reconocido"),
}
```

📌 **Cuándo usar `match`**

* Cuando hay **varias condiciones posibles**.
* Cuando se trabaja con **enum, strings o patrones complejos**.



### Código en Soroban



Nos vamos a una ruta donde queramos crear nuestro proyecto y ponemos el siguiente comando:

```
stellar contract init  conditionalStatements --name ConditionalStatements
```

Borramos el contrato generado en  lib.rs y ponemos el siguiente código

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, symbol_short, Env, Symbol};
#[contract]
pub struct ConditionalSt;

#[contractimpl]
impl ConditionalSt {
    // Función que usa if-else para determinar si un número es positivo o negativo.
    pub fn check_number(env: Env, num: i32) -> Symbol {
        if num >= 0 {
            Symbol::new(&env, "Numero positivo")
        } else {
            Symbol::new(&env, "Numero negativo")
        }
    }
    // Función que usa match para asignar permisos según el rol del usuario.
    pub fn check_role(env: Env, role: Symbol) -> Symbol {
        let admin: Symbol = symbol_short!("Admin");
        let user: Symbol = symbol_short!("User");
        let guest: Symbol = symbol_short!("Guest");

        match role {
            admin => Symbol::new(&env, "Acceso total"),
            user => Symbol::new(&env, "Acceso limitado"),
            guest => Symbol::new(&env, "Solo lectura"),
            _ => Symbol::new(&env, "Rol no reconocido"),
        }
    }
}
```

### 📌 **Explicación general del código**

* Usa `#![no_std]`, lo que significa que **no usa la biblioteca estándar de Rust**.
* Importa módulos de `soroban_sdk`, necesarios para escribir **contratos inteligentes en Soroban**.
* Define una **estructura de contrato** llamada `ConditionalSt`.
* Implementa dos funciones clave:
  1. `check_number()`: Usa `if-else` para verificar si un número es **positivo o negativo**.
  2. `check_role()`: Usa `match` para **asignar permisos** según el rol de un usuario.

***

### 🛠 **Explicación de las funciones**

Todas las funciones reciben `env: Env`, que es obligatorio en Soroban.

#### 1️⃣ **`check_number(env: Env, num: i32) -> Symbol`**

✅ **Determina si un número es positivo o negativo.**

📌 **Paso a paso:**

1. Recibe un número entero con signo (`i32`).
2. Usa una **estructura condicional `if-else`**:
   * Si `num >= 0`, retorna `"Numero positivo"`.
   * Si `num < 0`, retorna `"Numero negativo"`.
3. Retorna el resultado como un **símbolo (`Symbol`)**.

📌 **Ejemplo de uso:**

```rust
check_number(10)   // Devuelve "Numero positivo"
check_number(-5)   // Devuelve "Numero negativo"
```

***

#### 2️⃣ **`check_role(env: Env, role: Symbol) -> Symbol`**

✅ **Asigna permisos según el rol del usuario.**

📌 **Paso a paso:**

1. Define **tres roles** como `Symbol`:
   * `Admin` → `"Acceso total"`
   * `User` → `"Acceso limitado"`
   * `Guest` → `"Solo lectura"`
2. Usa `match` para **verificar el rol** y asignar permisos.
3. Si el rol no es reconocido, devuelve `"Rol no reconocido"`.

📌 **Ejemplo de uso:**

```rust
check_role("Admin")   // Devuelve "Acceso total"
check_role("User")    // Devuelve "Acceso limitado"
check_role("Otro")    // Devuelve "Rol no reconocido"
```

**Compilación del contrato**

Ejecutamos lo siguiente:

```
Stellar contract build
```

**Despliegue del contrato**

Para Mac y Linux el salto de línea es con el carácter " **\\**" y en Windows con el carácter " **´** "

Reemplaze el simbolo \* por el respectivo carácter de salto de linea a su sistema operativo.

```
stellar contract deploy *
  --wasm target/wasm32-unknown-unknown/release/ConditionalStatements.wasm *
  --source developer *
  --network testnet *
  --alias primitivedata
```
