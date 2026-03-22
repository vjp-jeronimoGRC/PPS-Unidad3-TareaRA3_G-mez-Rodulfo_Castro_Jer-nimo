# Apartado 3 - Actividad de Autenticación y Gestión de Sesiones

### Archivo auxiliar mostrar_sesion.php

![mostrar_sesion](./img/Pasted%20image%2020260307105931.png)

---
### Código vulnerable

![vulnerable 1](./img/Pasted%20image%2020260307110256.png)
![vulnerable 2](./img/Pasted%20image%2020260307110419.png)

A través del método GET el formulario envía como: `http://localhost/sesion.php?user=admin`

![GET Login](./img/Pasted%20image%2020260307110512.png)

---

### Explotación de Session Hijacking

**Pasos para obtener las Cookies en el navegador**

![Herramientas desarrollador](./img/Pasted%20image%2020260307111903.png)

Obtención de la cookie de sesión:

![Cookie PHPSESSID](./img/Pasted%20image%2020260307112023.png)

**Ataque detallado: Session Hijacking**

1. **El usuario legítimo inicia sesión**
    - El usuario accede a la web y pasa su nombre de usuario en la URL: [http://localhost/session.php?user=admin](http://localhost/session.php?user=admin)
    - El servidor crea una sesión y almacena la variable: `$_SESSION['user'] = 'admin';`
    - El navegador almacena la cookie de sesión: `PHPSESSID=98817c3a024d55f13e6196cda01543f1;`
    - Ahora, cada vez que el usuario haga una solicitud, el navegador enviará la cookie.

2. **El atacante roba la cookie de sesión**
    - Método escogido: **Captura de tráfico (MITM)**.

Captura de tráfico:
![Wireshark/MITM](./img/Pasted%20image%2020260307112559.png)

Encontramos entre los datos de la petición la cookie de sesión del usuario:
![Cookie robada](./img/Pasted%20image%2020260307113007.png)

**Suplantación de identidad:**
- Editar cookies en el navegador (F12):
![Editar cookies](./img/Pasted%20image%2020260307113749.png)

- Ir a **Application > Storage > Cookies**:
![Storage Cookies](./img/Pasted%20image%2020260307114004.png)

- Modificar PHPSESSID y reemplazarlo por el valor robado:
![Inyectar Cookie](./img/Pasted%20image%2020260307114258.png)

- Enviar el Session ID en una solicitud y acceder como la víctima:
![Acceso suplantado](./img/Pasted%20image%2020260307122831.png)

---
## Mitigación de problemas

### Prevenir vulnerabilidades XSS
Usar `htmlspecialchars()` siempre que se muestre información del usuario:
![XSS Fix](./img/Pasted%20image%2020260307123014.png)

### Prevenir vulnerabilidades Session Fixation
Regenerando el ID de sesión en cada inicio de sesión y sanitizando la entrada:
![Session Fixation Fix](./img/Pasted%20image%2020260307123304.png)

### Configurar la cookie de sesión de forma segura
- Tiempo de expiración determinado.
- Flag `HttpOnly` para anular ejecución de JavaScript.
- No permitir sesión en URL, solo cookies.
- Restricción de dominio.

![Cookie Segura](./img/Pasted%20image%2020260307123851.png)

---

### Lista blanca de servidores
Modificación para bloquear el acceso si el host no está permitido:
![WhiteList Host](./img/Pasted%20image%2020260307124051.png)

---
### Validar la IP y User-Agent del usuario
Creación del archivo con todas las modificaciones de seguridad:
![Validacion 1](./img/Pasted%20image%2020260307124342.png)
![Validacion 2](./img/Pasted%20image%2020260307125031.png)
![Validacion 3](./img/Pasted%20image%2020260307125156.png)

---
## Mejoras de seguridad adicionales: HTTPS con SSL/TLS

Crear certificados dentro del contenedor PHP:
![Certificados](./img/Pasted%20image%2020260307130156.png)

Configuración de Apache para la conexión **HTTPS**:
![Config Apache SSL](./img/Pasted%20image%2020260307130434.png)

Habilitar módulo SSL y añadir el sitio en `/etc/hosts`:
![Módulos Apache](./img/Pasted%20image%2020260307130754.png)
![Hosts File](./img/Pasted%20image%2020260307132355.png)

Prueba de seguridad y verificación del sitio seguro:
![Sitio Seguro 1](./img/Pasted%20image%2020260307131947.png)
![Sitio Seguro 2](./img/Pasted%20image%2020260307132450.png)

---
### Implementación de sesion2.php
Fichero con mejoras finales:
- Redirigir HTTP a HTTPS.
- Configuración de SameSite estricta.
- Control de intentos fallidos.
- Cierre seguro de sesión (Logout).

![sesion2 cod1](./img/Pasted%20image%2020260307133525.png)
![sesion2 cod2](./img/Pasted%20image%2020260307134148.png)

Mensaje de sesión cerrada correctamente:
![Logout OK](./img/Pasted%20image%2020260307134205.png)
