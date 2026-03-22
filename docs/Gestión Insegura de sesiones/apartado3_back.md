
---
### Archivo auxiliar mostrar_sesion.php

![[Pasted image 20260307105931.png]]

---
### Código vulnerable

![[Pasted image 20260307110256.png]]

![[Pasted image 20260307110419.png]]

A través del método GET el formulario envía como *http://localhost/sesion.php?user=admin

![[Pasted image 20260307110512.png]]

---

### Explotación de Session Hijacking

**Pasos para obtener las `Cookies` en el navegador**

![[Pasted image 20260307111903.png]]

Obtención de la cookie de sesión:

![[Pasted image 20260307112023.png]]

**Ataque detallado: Session Hijacking**

1. El usuario legítimo inicia sesión

    1. El usuario accede a la web y pasa su nombre de usuario en la URL: [http://localhost/session.php?user=admin](http://localhost/session.php?user=admin)
    
    2. El servidor crea una sesión y almacena la variable: `$_SESSION['user'] = 'admin';`
    
    3. El navegador almacena la cookie de session: `PHPSESSID=98817c3a024d55f13e6196cda01543f1;`
    
    4. Ahora, cada vez que el usuario haga una solicitud, el navegador enviará la cookie: `Cookie: PHPSESSID=98817c3a024d55f13e6196cda01543f1`

2.  El atacante roba la cookie de sesion:

Método escogido:  **Captura de tráfico (MITM)**

Captura de tráfico:

![[Pasted image 20260307112559.png]]

Encontramos entre los datos de la petición la cookie de sesión del usuario:

![[Pasted image 20260307113007.png]]

Una vez adquirida la Cookie de sesión:

- Editar cookies en el navegador: Abrir las herramientas de desarrollador (F12 en Chrome):

![[Pasted image 20260307113749.png]]

- Ir a Application > Storage > Cookies.

![[Pasted image 20260307114004.png]]

- Modificar PHPSESSID y reemplazarlo por el valor robado.
 ![[Pasted image 20260307114258.png]]

- Enviar el Session ID en una solicitud.

![[Pasted image 20260307122831.png]]


---
## Mitigación de problemas


## **Prevenir vulnerabilidades `XSS`**

Usar `htmlspecialchars()` siempre que se muestre información del usuario:

![[Pasted image 20260307123014.png]]

## **Prevenir vulnerabilidades `Session Fixation`**

Lo podemos hacer regenerando el ID de sesión en cada inicio de sesión, además guarda en la sesión el valor recibido por `GET['user']`, sanitizándolo para evitar ataques XSS (Cross-Site Scripting).

![[Pasted image 20260307123304.png]]

## **Configurar la cookie de sesión de forma segura y tiempo de expiración de sesión**

- La sesión sólo permanece abierta un tiempo determinado.
- Anulamos ejecución de JavaScript
- No permitimos sesion introducida directamente en URL, sólo a través de las cookies
- Sólo funciona en el sitio especificado "pps.edu" no localhost ni ningún otro dominio.

![[Pasted image 20260307123851.png]]

---

## Crear una lista blanca de servidores para bloquear acceso desde hosts no permitidos

Modificación por si el host actual no está en la lista no nos deja iniciar la sesión.

![[Pasted image 20260307124051.png]]

---
## **Validar la IP y User-Agent del usuario**

Creación del archivo con todas las modificaciones:

![[Pasted image 20260307124342.png]]

![[Pasted image 20260307125031.png]]

![[Pasted image 20260307125156.png]]


---

## Mejoras de seguridad adicionales

### Habilitar HTTPS con SSL/TLS (Apache)

Crear certificados dentro del contenedor php:

![[Pasted image 20260307130156.png]]

Creación del documento para la conexión **HTTPS**

![[Pasted image 20260307130434.png]]

Habilitar módulo `SSL` y deshabilitar el que teníamos hasta ahora `default`. Añadir el sitio en `/etc/hosts`

![[Pasted image 20260307130754.png]]

![[Pasted image 20260307132355.png]]

Prueba de seguridad:

![[Pasted image 20260307131947.png]]

Observamos que nos verifica la seguridad del sitio.

![[Pasted image 20260307132450.png]]

---

A continuación voy a crear un archivo llamado `sesion2.php` en este nuevo fichero he añadido:

- Redirigir HTTP a HTTPS
- Configuración de SameSite más estricta
- Control de intentos fallidos (prevención fuerza bruta)
- Cierre seguro de sesión
- Agregar un botón de ``logout`` en HTML.

![[Pasted image 20260307133525.png]]

![[Pasted image 20260307134148.png]]


Se muestra el mensaje de sesión cerrado:

![[Pasted image 20260307134205.png]]



