---
description: >-
  Tipos de datos que no necesitan ninguna importación, se manejan primitivamente
  en el contrato
---

# Tipos de datos primitivos

Nos vamos a una ruta donde queramos crear nuestro proyecto y ponemos el siguiente comando:

```
stellar contract init  primitivedata --name primitivedata
```

Borramos el contrato generado en  lib.rs y ponemos el siguiente código

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, Env};

#[contract]
pub struct DataTypesContract;
#[contractimpl]
impl DataTypesContract {
    // Suma dos números de 32 bits sin signo (u32).
    pub fn add_u32(_env: Env, a: u32, b: u32) -> u32 {
        a + b
    }
    // Suma dos números de 32 bits con signo (i32).
    pub fn add_i32(_env: Env, a: i32, b: i32) -> i32 {
        a + b
    }
    // Suma dos números de 64 bits sin signo (u64).
    pub fn add_u64(_env: Env, a: u64, b: u64) -> u64 {
        a + b
    }
    // Suma dos números de 64 bits sin signo (u64).
    pub fn add_i64(_env: Env, a: u64, b: u64) -> u64 {
        a + b
    }
    // Suma dos números de 128 bits sin signo (u128).
    pub fn add_u128(_env: Env, a: u128, b: u128) -> u128 {
        a + b
    }
    // Suma dos números de 128 bits con signo (i128).
    pub fn add_i128(_env: Env, a: i128, b: i128) -> i128 {
        a + b
    }
    // Invierte un valor booleano.
    pub fn negate_bool(_env: Env, flag: bool) -> bool {
        !flag
    }

}

```

#### 📌 **Explicación general**

* Usa **#!\[no\_std]**, lo que indica que no usa la biblioteca estándar de Rust.
* Importa módulos de **soroban\_sdk**, necesarios para ejecutar el contrato en Soroban.
* Define un contrato llamado `DataTypesContract`.
* Implementa varias funciones que realizan **suma de números** con diferentes tipos (`u32`, `i32`, `u64`, `i64`, `u128`, `i128`).
* Incluye una función para **negar un booleano**.

***

### 🛠 **Explicación de las funciones**

Todas las funciones reciben `_env: Env`, aunque no lo utilizan directamente. Es un requisito en Soroban para la ejecución de contratos.

#### 1️⃣ **Funciones de suma**

Cada una de estas funciones **suma dos números** del mismo tipo y retorna el resultado.

| **Función** | **Tipo de datos**            | **Rango de valores**                                                                                       |
| ----------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `add_u32`   | `u32` (sin signo, 32 bits)   | 0 a 4,294,967,295                                                                                          |
| `add_i32`   | `i32` (con signo, 32 bits)   | -2,147,483,648 a 2,147,483,647                                                                             |
| `add_u64`   | `u64` (sin signo, 64 bits)   | 0 a 18,446,744,073,709,551,615                                                                             |
| `add_i64`   | `i64` (con signo, 64 bits)   | -9,223,372,036,854,775,808 a 9,223,372,036,854,775,807                                                     |
| `add_u128`  | `u128` (sin signo, 128 bits) | 0 a 340,282,366,920,938,463,463,374,607,431,768,211,455                                                    |
| `add_i128`  | `i128` (con signo, 128 bits) | -170,141,183,460,469,231,731,687,303,715,884,105,728 a 170,141,183,460,469,231,731,687,303,715,884,105,727 |

📌 **Ejemplo de uso:**\
Si llamamos a `add_u32(5, 10)`, devuelve `15`.\
Si llamamos a `add_i64(-20, 50)`, devuelve `30`.

***

#### 2️⃣ **`negate_bool(_env: Env, flag: bool) -> bool`**

✅ **Invierte un valor booleano (`true` ↔ `false`).**

📌 **Paso a paso:**

1. Recibe un valor booleano (`flag`).
2. Usa el operador lógico `!` para invertirlo.
3. Retorna el valor opuesto.

📌 **Ejemplo de uso:**\
Si llamamos `negate_bool(true)`, devuelve `false`.\
Si llamamos `negate_bool(false)`, devuelve `true`.

***

📌 **Resumen**

Este contrato define funciones matemáticas y lógicas en Soroban:

* **Funciones de suma** (`add_u32`, `add_i32`, `add_u64`, `add_i64`, `add_u128`, `add_i128`): Permiten sumar números de diferentes tamaños y signos.
* **Función booleana** (`negate_bool`): Invierte el valor de un booleano.

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
  --wasm target/wasm32-unknown-unknown/release/primitivedata.wasm *
  --source developer *
  --network testnet *
  --alias primitivedata
```

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

**Pruebas del contrato**

Para **Linux y Mac** el salto de línea de la instrucción es con el carácter " \ " para **Windows** con el carácter " \` "

**Función add\_u32**

```
stellar contract invoke *
--id CCBJB6CU7VTVAO4OCYZJ75XPQMJ4AC2CBML3H2PXL2WQ55TGADD7PHPL *
--source developer *
--network testnet *
-- *
add_u32 *
--a 123 *
--b 123
```

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

**Función negate\_bool**

```
stellar contract invoke *
--id CCBJB6CU7VTVAO4OCYZJ75XPQMJ4AC2CBML3H2PXL2WQ55TGADD7PHPL *
--source developer *
--network testnet *
-- *
negate_bool *
--flag true
```

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
