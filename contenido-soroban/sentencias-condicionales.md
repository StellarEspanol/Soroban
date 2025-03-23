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



Nos vamos a una ruta donde queramos crear nuestro proyecto y ponemos el siguiente comando:

```
stellar contract init  conditionalStatements --name ConditionalStatements
```

Borramos el contrato generado en  lib.rs y ponemos el siguiente código

```rust
#![no_std]

use soroban_sdk::{contract, contractimpl, Env, Symbol};

#[contract]
pub struct MyContract;

#[contractimpl]
impl MyContract {
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
        match role.to_string().as_str() {
            "Admin" => Symbol::new(&env, "Acceso total"),
            "User"  => Symbol::new(&env, "Acceso limitado"),
            "Guest" => Symbol::new(&env, "Solo lectura"),
            _       => Symbol::new(&env, "Rol no reconocido"),
        }
    }
}
```



Borramos el contrato generado en  lib.rs y ponemos el siguiente código
