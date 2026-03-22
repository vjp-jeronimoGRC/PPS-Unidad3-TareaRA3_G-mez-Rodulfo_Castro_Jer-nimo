# Apartado 4 - Errores en la Seguridad y Componentes Vulnerables

## 1. Iniciar entorno de pruebas

Ejecución del script para restaurar a la configuración original:
![Restaurar Entorno](./img/Pasted%20image%2020260219181734.png)

Se inicia el escenario multicontenedor:
![Escenario Docker](./img/Pasted%20image%2020260219182132.png)

---

## 2. Instalación de Apache

Entramos en el contenedor:
![Acceso Contenedor](./img/Pasted%20image%2020260219182452.png)

Hacemos la instalación de `apache2`:
![Instalar Apache](./img/Pasted%20image%2020260219182812.png)

---

## 3. Estructura de directorios de configuración Apache

Visualización de la estructura de configuración:
![Estructura 1](./img/Pasted%20image%2020260219183407.png)
![Estructura 2](./img/Pasted%20image%2020260219183431.png)
![Estructura 3](./img/Pasted%20image%2020260219183455.png)

Vemos los módulos instalados y habilitados:
![Modulos Instalados](./img/Pasted%20image%2020260219184406.png)
![Modulos Habilitados](./img/Pasted%20image%2020260219185134.png)

---

## 4. Sitios Virtuales

Modificamos el archivo `/etc/apache2/sites-available/default.conf`:
![Config Sitio 1](./img/Pasted%20image%2020260219185358.png)
![Config Sitio 2](./img/Pasted%20image%2020260219190716.png)

Verificamos que Apache reconoce el sitio:
![Reconocimiento Sitio](./img/Pasted%20image%2020260219191001.png)

Cambio de propietarios y de los permisos del directorio web:
![Permisos](./img/Pasted%20image%2020260219191754.png)

Visualizamos la página en el navegador:
![Navegador Web](./img/Pasted%20image%2020260219192420.png)

---

## 5. Resolución local de nombres: DNS o fichero /etc/hosts

Cambiamos el nombre del host y consultamos la IP del contenedor:
![Nombre Host](./img/Pasted%20image%2020260219200557.png)
![IP Contenedor](./img/Pasted%20image%2020260219193145.png)

Reiniciamos el servicio Apache:
![Reinicio Apache 1](./img/Pasted%20image%2020260219201007.png)
![Reinicio Apache 2](./img/Pasted%20image%2020260219200713.png)

Resultado en el navegador usando el nombre de host:
![Navegador Host](./img/Pasted%20image%2020260219202128.png)

---

## 6. Creación de un servidor virtual "Hacker"

Creación de la subcarpeta y configuración del sitio:
![Carpeta Hacker](./img/Pasted%20image%2020260219202656.png)
![Index Hacker](./img/Pasted%20image%2020260219202949.png)
![Config Hacker](./img/Pasted%20image%2020260219203509.png)

Recargamos Apache y modificamos el servidor:
![Recarga Apache](./img/Pasted%20image%2020260219203623.png)
![Mod Host](./img/Pasted%20image%2020260219204856.png)

Acceso desde el navegador:
![Web Hacker](./img/Pasted%20image%2020260219205113.png)

---

## 7. Habilitar HTTPS con SSL/TLS

### Paso 1: Crear la clave privada y el certificado
![Crear SSL](./img/Pasted%20image%2020260220093920.png)
![Ver Certificados](./img/Pasted%20image%2020260220094048.png)

### Paso 2: Configurar Apache para usar HTTPS
![Config SSL](./img/Pasted%20image%2020260220094259.png)

### Paso 3: Habilitar SSL y el sitio
![Habilitar SSL](./img/Pasted%20image%2020260220094450.png)

### Paso 4: Configuración de hosts y verificación
![Hosts SSL](./img/Pasted%20image%2020260220095224.png)
![HTTPS OK](./img/Pasted%20image%2020260220095352.png)

---

## 8. Forzar HTTPS en Apache2

### Configuración en default.conf
Modificamos el archivo para redirigir tráfico al puerto 443:
![Forzar HTTPS 1](./img/Pasted%20image%2020260220112110.png)
![Forzar HTTPS 2](./img/Pasted%20image%2020260220112200.png)

### Configuración mediante .htaccess
![Config htaccess 1](./img/Pasted%20image%2020260220113234.png)
![Config htaccess 2](./img/Pasted%20image%2020260220113839.png)

---

## 9. Content Security Policy (CSP)
Implementación de cabeceras de seguridad CSP:
![CSP](./img/Pasted%20image%2020260220114830.png)

---

## 10. HSTS (HTTP Strict Transport Security)
Configuración de seguridad extra recomendada:
![HSTS](./img/Pasted%20image%2020260220115833.png)

---

## 11. Identificación y Corrección de Security Misconfiguration

### Configuraciones inseguras detectadas
![Inseguro](./img/Pasted%20image%2020260220120110.png)

### Corrección de la configuración (Server Signature)
Ocultamos la versión del servidor:
![Server Fix 1](./img/Pasted%20image%2020260220120632.png)
![Server Fix 2](./img/Pasted%20image%2020260220120806.png)

### Ocultar la versión de PHP (php.ini)
![PHP Fix 1](./img/Pasted%20image%2020260220121847.png)
![PHP Fix 2](./img/Pasted%20image%2020260220121812.png)
![PHP Fix 3](./img/Pasted%20image%2020260220122212.png)
![PHP Fix 4](./img/Pasted%20image%2020260220122315.png)

---

## 12. Configuración de mod_security con OWASP CRS

Pruebas previas de Inyección SQL y XSS:
![Test SQLi](./img/Pasted%20image%2020260304121112.png)
![Test XSS](./img/Pasted%20image%2020260304121147.png)

Instalación y configuración de ModSecurity:
![Instalar ModSec](./img/Pasted%20image%2020260304121411.png)
![Config ModSec](./img/Pasted%20image%2020260304121611.png)
![Recarga ModSec](./img/Pasted%20image%2020260304121831.png)
![Carga ModSec](./img/Pasted%20image%2020260304121909.png)

### Incorporación de OWASP Core Rule Set (CRS)
Clonación e inclusión de reglas:
![Clonar CRS](./img/Pasted%20image%2020260304122205.png)
![Incluir CRS](./img/Pasted%20image%2020260304122407.png)
![Verificar CRS](./img/Pasted%20image%2020260304122443.png)
![Config Extra CRS](./img/Pasted%20image%2020260304122538.png)

### Verificación de logs de bloqueo
![Logs ModSec](./img/Pasted%20image%2020260304131422.png)

---

## 13. Restauración Final
Desinstalación de módulos y limpieza para dejar el entorno original:
![Limpieza 1](./img/Pasted%20image%2020260304131710.png)
![Limpieza 2](./img/Pasted%20image%2020260304131728.png)
![Limpieza 3](./img/Pasted%20image%2020260304132011.png)
![Limpieza 4](./img/Pasted%20image%2020260304132049.png)
