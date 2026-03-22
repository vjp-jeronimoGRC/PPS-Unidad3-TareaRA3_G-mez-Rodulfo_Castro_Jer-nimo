
---

## Iniciar entorno de pruebas

Iniciamos el escenario multicontenedor:

![[Pasted image 20260302114224.png]]

### Creación de base de datos

Opción escogida -> **1**:

![[Pasted image 20260302114352.png]]

![[Pasted image 20260302115147.png]]

![[Pasted image 20260302115758.png]]

![[Pasted image 20260302115920.png]]

---

## Crear página web en Apache

![[Pasted image 20260302120608.png]]

![[Pasted image 20260302125657.png]]

---

## Explotación de Inyección SQL

- **Bypass de autenticación**

![[Pasted image 20260302131330.png]]
![[Pasted image 20260302131423.png]]

---
## Mitigación de vulnerabilidad

![[Pasted image 20260302133209.png]]

![[Pasted image 20260302133248.png]]

![[Pasted image 20260302133407.png]]

Realiza un **escapado de caracteres**. Utiliza la función `addslashes()` para añadir barras invertidas (`\`) antes de caracteres especiales como la comilla simple (`'`). 

El atacante introduce `'` y PHP lo convierte en `\'`. La base de datos interpreta esto como parte del texto, no como fin de cadena, rompiendo la inyección.

#### Código mejorado uso consultas parametrizadas

**login3.php**

![[Pasted image 20260302133713.png]]

Separa la estructura de la consulta SQL (`SELECT ... WHERE...`) de los datos introducidos por el usuario (`bind_param`). La BD recibe primero la estructura y luego los datos. Los datos **nunca** se ejecutan como código SQL, por lo que el payload `' OR '1'='1'` se trata solo como texto literal.

**login4.php**
 
![[Pasted image 20260302134011.png]]

Fuerza a que los datos de entrada sean de un tipo específico (por ejemplo, validar que un ID sea un número entero con `intval()`). Si el atacante introduce un texto o una comilla, la función lo convierte a `0` o lo elimina, impidiendo que el  payload SQL llegue a la base de datos.