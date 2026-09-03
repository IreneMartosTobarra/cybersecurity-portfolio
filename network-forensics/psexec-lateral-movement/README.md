Investigación de movimiento lateral mediante PsExec

##  Descripción del proyecto

Investigación práctica de **Network Forensics** centrada en la detección y análisis de movimiento lateral mediante **PsExec** dentro de una red Windows.

El análisis se realizó a partir de una captura de tráfico de red utilizando **Wireshark**, con el objetivo de identificar las comunicaciones entre los sistemas implicados, analizar la autenticación, detectar la transferencia del ejecutable utilizado por PsExec e identificar los recursos compartidos empleados durante la actividad.

Este proyecto está basado en un laboratorio práctico de **CyberDefenders** y documenta mi propio proceso de investigación y los conocimientos adquiridos durante su resolución.


##  Objetivos

Los principales objetivos de la investigación fueron:

* Identificar el sistema desde el que se inició la actividad del atacante.
* Identificar los equipos involucrados en el movimiento lateral.
* Analizar las comunicaciones SMB entre los sistemas.
* Examinar el proceso de autenticación mediante NTLMSSP.
* Identificar el ejecutable de servicio utilizado por PsExec.
* Identificar los recursos compartidos administrativos utilizados durante el ataque.
* Detectar evidencias de movimiento lateral adicional.
* Relacionar los hallazgos con técnicas de MITRE ATT&CK.


##  Herramientas y tecnologías

 Herramienta                                                  
 **Wireshark**             Análisis del tráfico de red                            
 **SMB / SMB2**            Análisis de comunicaciones y transferencia de archivos 
 **NTLMSSP**               Análisis de autenticación                             
 **PsExec**                Investigación de ejecución remota                      
 **MITRE ATT&CK**          Clasificación de las técnicas utilizadas               


#  Metodología de investigación

## 1. Análisis inicial del tráfico SMB

La investigación comenzó analizando el tráfico **SMB/SMB2** presente en la captura.

Se utilizaron filtros de Wireshark para localizar las comunicaciones relacionadas con SMB y determinar qué sistemas estaban intercambiando tráfico.

También se revisaron las conversaciones IPv4 para identificar las principales comunicaciones entre los equipos.


## 2. Identificación de los sistemas implicados

A partir del tráfico SMB y de los mensajes de autenticación, se analizaron las direcciones IP y los nombres de los equipos implicados.

Los mensajes **NTLMSSP Challenge** permitieron obtener información sobre los nombres de los sistemas objetivo.

Esto permitió reconstruir parcialmente el recorrido del atacante dentro de la red.

## 3. Análisis de la autenticación NTLM

Se analizaron los mensajes **NTLMSSP Authenticate** para estudiar la actividad de autenticación asociada al movimiento lateral.

El análisis permitió identificar información relacionada con la cuenta utilizada durante la autenticación y comprender cómo se produjo el acceso remoto entre los sistemas.


## 4. Identificación del ejecutable de PsExec

Una de las partes principales de la investigación consistió en determinar qué archivo fue transferido al sistema objetivo para permitir la ejecución remota.

Para ello se utilizó la funcionalidad de Wireshark:

**Archivo → Exportar objetos → SMB**

La revisión de los objetos transferidos permitió identificar el ejecutable asociado al servicio utilizado por PsExec.

Este hallazgo constituye un indicador importante de la utilización de **PsExec** durante el incidente.


## 5. Análisis de los recursos compartidos SMB

Se analizaron las conexiones **SMB Tree Connect** para identificar los recursos compartidos utilizados durante la actividad.

Los recursos administrativos de Windows son especialmente relevantes en este tipo de investigación, ya que pueden ser utilizados para transferir archivos y realizar acciones remotas con privilegios administrativos.


## 6. Detección de movimiento lateral

Después de identificar la primera comunicación y el sistema inicialmente comprometido, se analizaron las comunicaciones posteriores para determinar si el atacante intentó acceder a otros sistemas.

La correlación entre:

* tráfico SMB,
* autenticación NTLM,
* transferencia de archivos,
* recursos compartidos administrativos,
* y ejecución remota,

permitió identificar evidencias compatibles con **movimiento lateral mediante PsExec**.


#  Hallazgos principales

Durante la investigación se identificaron diferentes indicadores asociados a actividad de PsExec:

* Comunicación SMB entre diferentes sistemas Windows.
* Actividad de autenticación mediante NTLMSSP.
* Uso de credenciales para autenticación remota.
* Transferencia de un ejecutable relacionado con el servicio de PsExec.
* Uso de recursos compartidos administrativos de Windows.
* Evidencias de ejecución remota.
* Actividad posterior compatible con movimiento lateral hacia otro sistema.

La combinación de estos indicadores permitió reconstruir parte de la actividad del atacante y comprender cómo se produjo el movimiento lateral dentro de la red.



# Oportunidades de detección

Desde el punto de vista de un analista SOC, esta investigación permite identificar varios comportamientos que podrían utilizarse para crear detecciones.

### 1. Tráfico SMB inusual

Monitorizar conexiones SMB entre equipos que normalmente no mantienen comunicación entre sí.

### 2. Acceso a recursos administrativos

Investigar accesos inesperados a recursos compartidos administrativos, especialmente cuando proceden de estaciones de trabajo.

### 3. Creación y ejecución remota de servicios

Monitorizar la creación de nuevos servicios y su ejecución desde sistemas remotos.

### 4. Transferencia de ejecutables mediante SMB

Investigar archivos ejecutables transferidos mediante SMB antes de una actividad de ejecución remota.

### 5. Autenticación NTLM sospechosa

Correlacionar eventos de autenticación NTLM con conexiones SMB y actividad de movimiento lateral.

### 6. Correlación de múltiples indicadores

Un único indicador puede no ser suficiente para determinar un ataque. Sin embargo, la combinación de autenticación, SMB, transferencia de archivos y ejecución remota puede proporcionar una evidencia mucho más sólida.


#  Habilidades demostradas

A través de esta investigación he practicado:

* **Network Forensics**
* **Análisis de tráfico de red**
* **Wireshark**
* **SMB / SMB2**
* **NTLMSSP**
* **Análisis de autenticación**
* **Análisis de recursos compartidos Windows**
* **Investigación de PsExec**
* **Detección de movimiento lateral**
* **Análisis de incidentes**
* **MITRE ATT&CK**
* **Identificación de indicadores de compromiso**


#  Conclusión

Esta investigación permitió analizar desde una perspectiva de defensa cómo una herramienta legítima de administración remota como **PsExec** puede ser utilizada para realizar movimiento lateral dentro de una red.

El análisis de tráfico de red proporciona información valiosa para un analista de seguridad, especialmente cuando se correlacionan diferentes evidencias como autenticación, comunicaciones SMB, transferencia de archivos y ejecución remota.

Este laboratorio me permitió practicar un flujo de investigación similar al que puede realizar un analista **SOC / Blue Team** al investigar actividad sospechosa dentro de una red.



##  Disclaimer

Este proyecto está basado en un laboratorio de entrenamiento de **CyberDefenders**.

No se incluyen en este repositorio los archivos originales del laboratorio, las respuestas del desafío ni contenido propietario de la plataforma.

El objetivo de este repositorio es documentar mi propio proceso de aprendizaje, metodología de análisis y conocimientos adquiridos durante la investigación.
