# Apartado 5 - Escaneo estático y dinámico de aplicación web

Descargamos y descomprimimos el zip de la aplicación con la que vamos a trabajar:

![Descarga App](./img/Pasted%20image%2020260321172826.png)

---
### 5.1. Ejecución de la aplicación

Creación del `Dockerfile` para el entorno:
![Dockerfile](./img/Pasted%20image%2020260321173212.png)

Creación del archivo `docker-compose.yml`:
![Docker Compose](./img/Pasted%20image%2020260321173405.png)

Una vez creados estos dos archivos, construiremos y levantaremos el contenedor:
![Levantar Contenedores](./img/Pasted%20image%2020260321205811.png)

Podemos observar los logs de los diferentes servicios desplegados:

**Springboot**
![Log Springboot 1](./img/Pasted%20image%2020260321205950.png)
![Log Springboot 2](./img/Pasted%20image%2020260321210252.png)

**Nessus**
![Log Nessus 1](./img/Pasted%20image%2020260321210031.png)
![Log Nessus 2](./img/Pasted%20image%2020260321210331.png)

**Sonarqube**
![Log Sonar 1](./img/Pasted%20image%2020260321210116.png)
![Log Sonar 2](./img/Pasted%20image%2020260321210357.png)

---
### 5.2. Escaneo estático (SAST)

El primer paso es identificar la IP del servidor de `SonarQube`:
![IP Sonar](./img/Pasted%20image%20202603211327.png)

Accedemos al contenedor de la aplicación para realizar el escaneo:
![Acceso Contenedor](./img/Pasted%20image%2020260321211426.png)

Instalamos el cliente `sonar-scanner`:
![Install Scanner](./img/Pasted%20image%2020260321211503.png)

Configuramos la IP del contenedor en `sonar-scanner.properties`:
![Config IP Sonar](./img/Captura%20de%20pantalla%202026-03-21%20212146.png)

Configuramos el archivo del proyecto y ejecutamos el escaneo:
![Config Proyecto](./img/Pasted%20image%2020260321212359.png)
![Ejecución Escaneo 1](./img/Pasted%20image%2020260321214948.png)
![Ejecución Escaneo 2](./img/Pasted%20image%2020260322101050.png)

---
### 5.3. Análisis de seguridad dinámico (DAST)

Configuración y lanzamiento del escaneo con `Nessus`:

1. **Configuración del escaneo**:
   ![Nessus Config](./img/Pasted%20image%2020260322110157.png)

2. **Lanzamiento del escaneo**:
   ![Nessus Launch](./img/Pasted%20image%2020260322110846.png)

3. **Resultados del análisis**:
   ![Nessus Results](./img/Pasted%20image%2020260322111644.png)

---
### 5.4. Análisis de dependencias (SCA)

Entramos al contenedor y ejecutamos el análisis de dependencias de **Maven**:
![Acceso SCA](./img/Pasted%20image%2020260322112449.png)
![Maven Analysis 1](./img/Pasted%20image%2020260322121147.png)
![Maven Analysis 2](./img/Pasted%20image%2020260322120918.png)

Identificación de vulnerabilidades críticas (**CVE**):
![CVE Detectados 1](./img/Pasted%20image%2020260322121055.png)
![CVE Detectados 2](./img/Pasted%20image%2020260322120847.png)

---
### 5.5. Análisis de los problemas encontrados

### Apartado 5 - 5. Análisis de los problemas encontrados

| **Vulnerabilidad**                         | **CWE**                                                   | **Consecuencias**                                                                       | **Localización**                                 | **Exploit(s)**                                                                           | **Solución**                                                                                     |
| ------------------------------------------ | --------------------------------------------------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Remote Code Execution (Spring4Shell)**   | **CWE-94:** Inyección de código                           | Permite a un atacante ejecutar comandos arbitrarios en el servidor de forma remota.     | `spring-core-5.0.9.RELEASE.jar` (CVE-2022-22965) | Envío de una petición HTTP POST especialmente diseñada que manipula el _class loader_.   | Actualizar Spring Framework a la versión 5.3.18 o superior.                                      |
| **Deserialización de datos insegura**      | **CWE-502:** Deserialización de datos no confiables       | Ejecución de código arbitrario al procesar archivos JSON maliciosos.                    | `jackson-databind-2.9.6.jar` (CVE-2018-14718)    | Enviar un objeto JSON que apunte a una clase maliciosa que Jackson intentará instanciar. | Actualizar la librería Jackson Databind a la versión 2.13 o superior.                            |
| **AJP Ghostcat Vulnerability**             | **CWE-20:** Validación de entrada incorrecta              | Lectura de archivos arbitrarios del servidor (como archivos de configuración o código). | `tomcat-embed-core-8.5.34.jar` (CVE-2020-1938)   | Conectarse al puerto AJP (8009) y solicitar archivos fuera de la raíz web.               | Actualizar Tomcat a la versión 8.5.51 o desactivar el conector AJP si no se usa.                 |
| **Prototype Pollution (JS)**               | **CWE-1321:** Inyección en prototipos                     | Puede llevar a denegación de servicio (DoS) o ejecución de código en el cliente (XSS).  | `jquery-3.3.1-1.jar` (CVE-2019-11358)            | Manipular el atributo `__proto__` a través de funciones como `$.extend()`.               | Actualizar jQuery a la versión 3.4.0 o superior.                                                 |
| **Inyección de Entidad Externa XML (XXE)** | **CWE-611:** Restricción incorrecta de entidades externas | Lectura de archivos locales del servidor y escaneo de puertos internos.                 | `dom4j-1.6.1.jar` (CVE-2020-10683)               | Enviar un archivo XML con una entidad DTD que apunte a `/etc/passwd`.                    | Actualizar dom4j a la versión 2.1.3 o configurar el parser para deshabilitar entidades externas. |                                                                                                                                                                         

