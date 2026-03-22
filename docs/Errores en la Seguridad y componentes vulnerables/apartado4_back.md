
---

## Iniciar entorno de pruebas

Ejecución del script para restaurar a la configuración original

![[Pasted image 20260219181734.png]]

Se inicia el escenario multicontenedor:

![[Pasted image 20260219182132.png]]

---

## 2. Instalación de Apache

Entramos en el contenedor:

![[Pasted image 20260219182452.png]]

Hacemos la instalación de `apache`:

![[Pasted image 20260219182812.png]]


---

## 3. Estructura de directorios de configuración Apache

![[Pasted image 20260219183407.png]]![[Pasted image 20260219183431.png]]

![[Pasted image 20260219183455.png]]

Vemos los módulos instalados:

![[Pasted image 20260219184406.png]]

Visualizamos los módulos habilitados:

![[Pasted image 20260219185134.png]]

---
## 4. Sitios Virtuales

Modificamos el el archivo`/etc/apache2/sites-available/default.conf`:

![[Pasted image 20260219185358.png]]
![[Pasted image 20260219190716.png]]

Podemos observar que el sitio ya lo reconoce Apache:

![[Pasted image 20260219191001.png]]

Cambio de propietarios y de los permisos:

![[Pasted image 20260219191754.png]]


Visualizamos la página:

![[Pasted image 20260219192420.png]]


---

## 5. Resolución local de nombres: dns o fichero **/etc/hosts**

Cambiamos el nombre del host:

![[Pasted image 20260219200557.png]]

Consultamos la dirección IP del contenedor:

![[Pasted image 20260219193145.png]]

Reiniciamos el servicio Apache:

![[Pasted image 20260219201007.png]]

![[Pasted image 20260219200713.png]]

Salida en el navegador:

![[Pasted image 20260219202128.png]]

---

## 6. Creación de un servidor virtual **Hacker**

Creación de la subcarpeta *hacker*:

![[Pasted image 20260219202656.png]]

![[Pasted image 20260219202949.png]]

Creación del archivo de configuración del sitio:

![[Pasted image 20260219203509.png]]

Recargamos el servicio apache:

![[Pasted image 20260219203623.png]]

Modificación de la dirección del servidor:

![[Pasted image 20260219204856.png]]

Accedemos por el navegador:

![[Pasted image 20260219205113.png]]


---

## 7. Cómo habilitar HTTPS con SSL/TLS en Servidor Apache


## **Paso 1: Crear la clave privada y el certificado**

![[Pasted image 20260220093920.png]]

Visualizamos la creación del certificado y la clave pública:

![[Pasted image 20260220094048.png]]

## **Paso 2.Configurar Apache para usar HTTPS**

![[Pasted image 20260220094259.png]]

## **Paso 3: Habilitar SSL y el sitio:**

![[Pasted image 20260220094450.png]]

## Paso 4: Poner **dirección en /etc/hosts o habilitar puerto 443**

![[Pasted image 20260220095224.png]]

![[Pasted image 20260220095352.png]]

---

## 8.Forzar HTTPS en Apache2 (default.conf y .htaccess)

Opción escogida:

**Mediante la configuración de apache en la configuración del sitio**

#### 1. Configuración en `default.conf` (archivo de configuración de Apache)

Modificamos el archivo `default.conf`:

![[Pasted image 20260220112110.png]]

Reiniciamos el Apache:

![[Pasted image 20260220112200.png]]

#### 2. Configuración en `.htaccess`

Modificamos el `default.conf`:

![[Pasted image 20260220113234.png]]

Creación del archivo `.htaccess`:

![[Pasted image 20260220113839.png]]

---

## 9. Implementación y Evaluación de Content Security Policy (CSP)

![[Pasted image 20260220114830.png]]

---

## 10. Nota de seguridad extra: HSTS (opcional pero recomendado)

![[Pasted image 20260220115833.png]]

---

## 11. Identificación y Corrección de Security Misconfiguration

#### Configuraciones inseguras

![[Pasted image 20260220120110.png]]

### Corregir la configuración del servidor Apache

![[Pasted image 20260220120632.png]]

No aparece la versión del servidor Apache.

![[Pasted image 20260220120806.png]]

#### Ocultar la versión de PHP (php.ini)

![[Pasted image 20260220121847.png]]

![[Pasted image 20260220121812.png]]

![[Pasted image 20260220122212.png]]

![[Pasted image 20260220122315.png]]


---

## 10. Configuración de `mod_security` con reglas OWASP CRS en Apache.

**Inyección SQL**

![[Pasted image 20260304121112.png]]

**XSS**

![[Pasted image 20260304121147.png]]


Cortafuegos `mod_security` 

![[Pasted image 20260304121411.png]]

Fichero de configuración de `modsecurity.conf`

![[Pasted image 20260304121611.png]]

Guarda y recarga el servicio Apache:

![[Pasted image 20260304121831.png]]

Verifica que `mod_security` esté cargado:

![[Pasted image 20260304121909.png]]

---
### Descargar OWASP ModSecurity Core Rule Set (CRS)

Para incorporar las reglas CRS de OWASP a `mod_security` clonamos el repositorio y copiamos el archivo de configuración.

![[Pasted image 20260304122205.png]]

### Incluir las reglas OWASP en la configuración

![[Pasted image 20260304122407.png]]

Para comprobar si están añadidas las reglas de modsecurity-crs, puedes hacer:

![[Pasted image 20260304122443.png]]

**IMPORTANTE¡¡ Solo en el caso de que no te aparezcan cargados los módulos**, edita el archivo de configuración de Apache para que cargue las reglas. Puedes hacer esto en un archivo `.conf` dentro de `/etc/apache2/conf-available/`:

![[Pasted image 20260304122538.png]]

Luego, habilita el archivo de configuración y reinicia el servicio:

---
### Ver logs de ModSecurity



![[Pasted image 20260304131422.png]]

---

## Volver a dejar todo "niquelao"

Desinstalamos Modsecurity y elimanos las reglas de ModSecurity:

![[Pasted image 20260304131710.png]]

![[Pasted image 20260304131728.png]]

**GUARDANDO LOS CAMBIOS**

![[Pasted image 20260304132011.png]]

![[Pasted image 20260304132049.png]]