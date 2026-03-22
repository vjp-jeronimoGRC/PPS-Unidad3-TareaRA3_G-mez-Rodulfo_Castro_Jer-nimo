
---

Descargamos y descomprimimos el zip de la aplicación con la que vamos a trabajar:

![[Pasted image 20260321172826.png]]

---
### Apartado 5 - 1. Ejecución de la aplicación.

Creación del `Dockerfile`:

![[Pasted image 20260321173212.png]]

Creación del archivo `docker-compose.yml`:

![[Pasted image 20260321173405.png]]

Una vez creados estos dos archivos, construiremos y levantaremos el contenedor:

![[Pasted image 20260321205811.png]]

Podemos observar que se van mostrando logs de los diferentes servicios:

**Springboot**

![[Pasted image 20260321205950.png]]

![[Pasted image 20260321210252.png]]

**Nessus**

![[Pasted image 20260321210031.png]]

![[Pasted image 20260321210331.png]]

**Sonarqube**

![[Pasted image 20260321210116.png]]

![[Pasted image 20260321210357.png]]

---
### Apartado 5 - 2. Escaneo estático.

El primer paso será saber la *IP* donde se encuentra el servidor de `SonarQube`:

![[Pasted image 20260321211327.png]]

Y accedemos al contenedor de la aplicación para realizar el escaneo estático:

![[Pasted image 20260321211426.png]]

Una vez dentro descargamos e instalamos el cliente sonar-scanner:

![[Pasted image 20260321211503.png]]

Ponemos en la configuración de sonar-scanner la ip de nuestro contenedor `SonarQube`.

![[Captura de pantalla 2026-03-21 212146.png]]

Y ponemos el archivo de configuración del proyecto:

![[Pasted image 20260321212359.png]]

Después se ejecuta el escaneo:

![[Pasted image 20260321214948.png]]

![[Pasted image 20260322101050.png]]

---
### Apartado 5 - 3. Análisis de seguridad con DAST.

Una vez levantada la aplicación estamos a disposición de poder escanearla con `Nessus`:

Aquí los pasos:

1. ![[Pasted image 20260322110157.png]]
2. Una vez creado el Escaneo lo lanzaremos:
	![[Pasted image 20260322110846.png]]

3. Visualizamos los resultados del escaneo:
	![[Pasted image 20260322111644.png]]

---
### Apartado 5 - 4. Análisis de dependencias de librerías.

Primero entramos al contenedor:

![[Pasted image 20260322112449.png]]

Ejecutamos el análisis **Maven**:

![[Pasted image 20260322121147.png]]

![[Pasted image 20260322120918.png]]

Podemos observar una gran cantidad de **CVE**:
![[Pasted image 20260322121055.png]]

![[Pasted image 20260322120847.png]]


---
### Apartado 5 - 5. Análisis de los problemas encontrados

| **Vulnerabilidad**                         | **CWE**                                                   | **Consecuencias**                                                                       | **Localización**                                 | **Exploit(s)**                                                                           | **Solución**                                                                                     |
| ------------------------------------------ | --------------------------------------------------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Remote Code Execution (Spring4Shell)**   | **CWE-94:** Inyección de código                           | Permite a un atacante ejecutar comandos arbitrarios en el servidor de forma remota.     | `spring-core-5.0.9.RELEASE.jar` (CVE-2022-22965) | Envío de una petición HTTP POST especialmente diseñada que manipula el _class loader_.   | Actualizar Spring Framework a la versión 5.3.18 o superior.                                      |
| **Deserialización de datos insegura**      | **CWE-502:** Deserialización de datos no confiables       | Ejecución de código arbitrario al procesar archivos JSON maliciosos.                    | `jackson-databind-2.9.6.jar` (CVE-2018-14718)    | Enviar un objeto JSON que apunte a una clase maliciosa que Jackson intentará instanciar. | Actualizar la librería Jackson Databind a la versión 2.13 o superior.                            |
| **AJP Ghostcat Vulnerability**             | **CWE-20:** Validación de entrada incorrecta              | Lectura de archivos arbitrarios del servidor (como archivos de configuración o código). | `tomcat-embed-core-8.5.34.jar` (CVE-2020-1938)   | Conectarse al puerto AJP (8009) y solicitar archivos fuera de la raíz web.               | Actualizar Tomcat a la versión 8.5.51 o desactivar el conector AJP si no se usa.                 |
| **Prototype Pollution (JS)**               | **CWE-1321:** Inyección en prototipos                     | Puede llevar a denegación de servicio (DoS) o ejecución de código en el cliente (XSS).  | `jquery-3.3.1-1.jar` (CVE-2019-11358)            | Manipular el atributo `__proto__` a través de funciones como `$.extend()`.               | Actualizar jQuery a la versión 3.4.0 o superior.                                                 |
| **Inyección de Entidad Externa XML (XXE)** | **CWE-611:** Restricción incorrecta de entidades externas | Lectura de archivos locales del servidor y escaneo de puertos internos.                 | `dom4j-1.6.1.jar` (CVE-2020-10683)               | Enviar un archivo XML con una entidad DTD que apunte a `/etc/passwd`.                    | Actualizar dom4j a la versión 2.1.3 o configurar el parser para deshabilitar entidades externas. |