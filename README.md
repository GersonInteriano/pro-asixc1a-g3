<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/75f17801-4b98-4781-934b-30af8fb42f61" />
# sujeta a cambios


# Proyecto Transversal: InnovateTech - Infraestructura Híbrida

## Tabla de Contenidos

1. [Introducción y Contexto del Proyecto](#1-introduccion-y-contexto-del-proyecto)
2. [Propuesta de CPD Local (Infraestructura Física)](#2-propuesta-de-cpd-local-infraestructura-fisica)
   * [2.1. Ubicación y Acondicionamiento](#21-ubicacion-y-acondicionamiento)
   * [2.2. Diseño de Racks y Organización](#22-diseno-de-racks-y-organizacion)
   * [2.3. Infraestructura Eléctrica (SAI)](#23-infraestructura-electrica-sai)
   * [2.4. Seguridad Física y PRL](#24-seguridad-fisica-y-prl)
3. [Despliegue en el Núvol (AWS)](#3-despliegue-en-el-nuvol-aws)
   * [3.1. Arquitectura de Red (VPC)](#31-arquitectura-de-red-vpc)
   * [3.2. Instancias EC2](#32-instancias-ec2)
   * [3.3. Gestión de Accesos (SSH Keys)](#33-gestion-de-accesos-ssh-keys)
   * [3.4. Automatización con Ansible](#34-automatizacion-con-ansible)
4. [Implantación de Servicios Multimedia](#4-implantacion-de-servicios-multimedia)
   * [4.1. Servicio de Streaming de Audio](#41-servicio-de-streaming-de-audio)
   * [4.2. Servicio de Streaming de Vídeo](#42-servicio-de-streaming-de-video)
   * [4.3. Videoconferencia (Jitsi Meet)](#43-videoconferencia-jitsi-meet)
5. [Servicios de Red y Gestión de Identidad](#5-servicios-de-red-y-gestion-de-identidad)
   * [5.1. Directorio Activo (AD/LDAP)](#51-directorio-activo-adldap)
   * [5.2. SFTP Seguro e Integración](#52-sftp-seguro-e-integracion)
   * [5.3. Centralización de Logs](#53-centralizacion-de-logs)
6. [Diseño e Implementación de la Base de Datos](#6-diseno-e-implementacion-de-la-base-de-datos)
   * [6.1. Diseño Conceptual (E/R)](#61-diseno-conceptual-er)
   * [6.2. Diseño Lógico (Relacional)](#62-diseno-logico-relacional)
   * [6.3. Script de Creación de Usuarios](#63-script-de-creacion-de-usuarios)
   * [6.4. Programación (Triggers, Events y Auditoría)](#64-programacion-triggers-events-y-auditoria)
7. [Comprobaciones de Rendimiento y Seguridad](#7-comprobaciones-de-rendimiento-y-seguridad)
8. [Digitalización y Sostenibilidad](#8-digitalizacion-y-sostenibilidad)
9. [Conclusiones](#9-conclusiones)
10. [Anexos y Entregables](#10-anexos-y-entregables)

---

## 1. Introducción y Contexto del Proyecto <a name="1-introduccion-y-contexto-del-proyecto"></a>

El presente proyecto tiene como finalidad diseñar e implementar una infraestructura tecnológica robusta para InnovateTech, una empresa en expansión dedicada a la provisión de servicios digitales. El núcleo de la propuesta es un modelo híbrido que combina la seguridad y el control de un CPD local con la escalabilidad y alta disponibilidad de la nube de Amazon Web Services (AWS).

InnovateTech experimenta un crecimiento acelerado en sus ventas online y una demanda crítica de soporte técnico. Para atender estas necesidades, el proyecto se enfoca en desplegar:

**Gestión de Identidad:** Control centralizado de usuarios mediante LDAP.

**Servicios Multimedia:** Plataformas de streaming de audio y vídeo, además de videoconferencia (Jitsi).

**Persistencia de Datos:** Implementación de una base de datos relacional para la gestión operativa y auditoría.

**Automatización y Monitorización:** Despliegue de procesos automatizados mediante Ansible y centralización de registros de eventos.

## Valores Técnicos Fundamentales

La solución se fundamenta en tres pilares esenciales:

**Seguridad:** Implementación de protocolos cifrados, gestión rigurosa de roles y segmentación de red para garantizar la integridad y confidencialidad.

**Sostenibilidad:** Diseño eficiente del hardware local con el objetivo de minimizar el consumo energético y evaluar la huella ecológica.

**Escalabilidad:** Arquitectura en la nube diseñada para adaptarse dinámicamente a picos de demanda en servicios multimedia.

[⬆ Volver al índice](#-tabla-de-contenidos)

## 2. Propuesta de CPD Local (Infraestructura Física) <a name="2-propuesta-de-cpd-local-infraestructura-fisica"></a>

El Centro de Procesamiento de Datos (CPD) local se ha concebido como el centro neurálgico de administración y conectividad de InnovateTech. Su diseño físico prioriza la integridad del hardware y la continuidad del servicio.

### 2.1. Ubicación y Acondicionamiento <a name="21-ubicacion-y-acondicionamiento"></a>

La sala técnica se ha acondicionado siguiendo normativas de seguridad y eficiencia:

<img width="8192" height="5517" alt="image" src="https://github.com/user-attachments/assets/27725aa7-95fe-4cb1-b4dc-702155227373" />
https://mermaid.ai/d/1d8e0a02-9697-4f68-99f1-cd7f8237895b
<br><br>
_Enlace al plano logico_





- **Entorno Físico:** Ubicación en una sala interior, sin ventanas y con muros de resistencia al fuego. Se utiliza suelo técnico elevado (30 cm) para la canalización oculta de cables y falso techo para la extracción de aire caliente.

- **Climatización de Precisión:** Se implementa un sistema de aire acondicionado industrial manteniendo la temperatura constante entre 20°C y 22°C. El diseño utiliza la metodología de pasillos fríos y calientes para evitar puntos de calor en los equipos.

- **Seguridad y Prevención:**

  - **Detección de Incendios:** Sensores ópticos de humo y temperatura.

  - **Extinción:** Sistema automático mediante gas inerte (Novec o CO2), que sofoca el fuego sin dañar los componentes electrónicos ni dejar residuos.
  
  - **Control de Acceso:** Cerradura electrónica con registro de entrada para personal autorizado.

### 2.2. Diseño de Racks y Organización <a name="22-diseno-de-racks-y-organizacion"></a>

Para una gestión eficiente, la infraestructura se divide en dos armarios Rack de 42U, separando las funciones de red de las de administración:

### Rack 1: Networking y Seguridad (Infraestructura de Red)

- Contiene los elementos que garantizan la comunicación interna y el enlace con AWS:
- Patch Panels: Gestión del cableado estructurado que llega desde los puestos de trabajo.
- Router de Borde y Firewall: Encargados de la seguridad perimetral y de mantener el túnel VPN con la nube.
- Switch Core: Dispositivo de alta velocidad (Capa 3) para la distribución de VLANs.
- SAI (Sistema de Alimentación Ininterrumpida): Ubicado en la parte inferior para proporcionar estabilidad eléctrica y autonomía en caso de fallo de suministro.

### Rack 2: Gestión y Administración (Servicios Locales)

- Orientado al soporte administrativo y la protección de datos local:
- Servidor de Administración Local: Controlador de dominio secundario y gestión de políticas internas.
- Almacenamiento NAS: Nodo dedicado a las copias de seguridad de la base de datos y logs de AWS, garantizando una redundancia fuera de la nube.
- Consola KVM: Para la administración física de los servidores sin necesidad de periféricos individuales.
- Servidor de Monitorización: Supervisión en tiempo real de la temperatura, consumo y estado de los servicios.

### 2.3. Infraestructura Eléctrica (SAI) <a name="23-infraestructura-electrica-sai"></a>
*(Contenido aquí...)*

### 2.4. Seguridad Física y PRL <a name="24-seguridad-fisica-y-prl"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 3. Despliegue en el Núvol (AWS) <a name="3-despliegue-en-el-nuvol-aws"></a>

### 3.1. Arquitectura de Red (VPC) <a name="31-arquitectura-de-red-vpc"></a>
*(Contenido aquí...)*

### 3.2. Instancias EC2 <a name="32-instancias-ec2"></a>
*(Contenido aquí...)*

### 3.3. Gestión de Accesos (SSH Keys) <a name="33-gestion-de-accesos-ssh-keys"></a>
*(Contenido aquí...)*

### 3.4. Automatización con Ansible <a name="34-automatizacion-con-ansible"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 4. Implantación de Servicios Multimedia <a name="4-implantacion-de-servicios-multimedia"></a>

### 4.1. Servicio de Streaming de Audio <a name="41-servicio-de-streaming-de-audio"></a>
*(Contenido aquí...)*

### 4.2. Servicio de Streaming de Vídeo <a name="42-servicio-de-streaming-de-video"></a>
*(Contenido aquí...)*

### 4.3. Videoconferencia (Jitsi Meet) <a name="43-videoconferencia-jitsi-meet"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 5. Servicios de Red y Gestión de Identidad <a name="5-servicios-de-red-y-gestion-de-identidad"></a>

### 5.1. Directorio Activo (AD/LDAP) <a name="51-directorio-activo-adldap"></a>
*(Contenido aquí...)*

### 5.2. SFTP Seguro e Integración <a name="52-sftp-seguro-e-integracion"></a>
*(Contenido aquí...)*

### 5.3. Centralización de Logs <a name="53-centralizacion-de-logs"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 6. Diseño e Implementación de la Base de Datos <a name="6-diseno-e-implementacion-de-la-base-de-datos"></a>

### 6.1. Diseño Conceptual (E/R) <a name="61-diseno-conceptual-er"></a>
*(Contenido aquí...)*

### 6.2. Diseño Lógico (Relacional) <a name="62-diseno-logico-relacional"></a>
*(Contenido aquí...)*

### 6.3. Instalación y Securización de MariaDB <a name="63-instalacion-y-securizacion-de-mariadb"></a>
*(Contenido aquí...)*

#### Evidencias de Configuración de Red (Instancia EC2)

Para permitir que el motor de base de datos MariaDB acepte conexiones externas provenientes de los servidores de aplicaciones de nuestros compañeros (dentro de la misma VPC o mediante accesos autorizados), modificamos la directiva de escucha por defecto.
<br><br>
<img width="938" height="246" alt="image" src="https://github.com/user-attachments/assets/82b11735-8ecf-47b3-a249-38a446847d54" />
<br><br>

- Captura del archivo `/etc/mysql/mariadb.conf.d/50-server.cnf` donde se aprecia la modificación de la directiva `bind-address`. Al establecer el valor en `0.0.0.0`, obligamos al servicio a escuchar en todas las interfaces de red disponibles en la instancia EC2, superando la restricción local (`127.0.0.1`) que viene configurada de fábrica.
<br><br>

<img width="938" height="706" alt="image" src="https://github.com/user-attachments/assets/245f85ef-15a1-481c-b2a2-cc4b50bcce23" />
<br><br>

- Evidencia del reinicio del demonio del SGBD mediante `sudo systemctl restart mariadb`. La captura muestra el comando `sudo systemctl status mariadb` con el flag `active (running)` en verde, confirmando que el cambio sintáctico es correcto y que el motor de base de datos ha levantado el servicio sin errores en el puerto estándar 3306.


### 6.4. Script de Creación de Usuarios <a name="64-script-de-creacion-de-usuarios"></a>
*(Contenido aquí...)*

### 6.5. Programación (Triggers, Events y Auditoría) <a name="65-programacion-triggers-events-y-auditoria"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 7. Comprobaciones de Rendimiento y Seguridad <a name="7-comprobaciones-de-rendimiento-y-seguridad"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 8. Digitalización y Sostenibilidad <a name="8-digitalizacion-y-sostenibilidad"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 9. Conclusiones <a name="9-conclusiones"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 10. Anexos y Entregables <a name="10-anexos-y-entregables"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)
