<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/75f17801-4b98-4781-934b-30af8fb42f61" />

# Proyecto Transversal: InnovateTech — Infraestructura Híbrida
**CFGS Administració de Sistemes Informàtics en Xarxa · Curs 25/26 · Institut Tecnològic de Barcelona**

---

https://docs.google.com/presentation/d/1WFo9iezIU0LWEfw7Di2PuHeG-EueYokx/edit?usp=sharing&ouid=103478355393836279819&rtpof=true&sd=true
## Tabla de Contenidos

1. [Introducción y Contexto del Proyecto](#1-introduccion-y-contexto-del-proyecto)
2. [Propuesta de CPD Local (Infraestructura Física)](#2-propuesta-de-cpd-local-infraestructura-fisica)
   - [2.1. Ubicación y Acondicionamiento](#21-ubicacion-y-acondicionamiento)
   - [2.2. Diseño de Racks y Organización](#22-diseno-de-racks-y-organizacion)
   - [2.3. Infraestructura Eléctrica (SAI)](#23-infraestructura-electrica-sai)
   - [2.4. Seguridad Física y PRL](#24-seguridad-fisica-y-prl)
3. [Despliegue en el Núvol (AWS)](#3-despliegue-en-el-nuvol-aws)
   - [3.1. Arquitectura de Red (VPC)](#31-arquitectura-de-red-vpc)
   - [3.2. Instancias EC2](#32-instancias-ec2)
   - [3.3. Gestión de Accesos (SSH Keys)](#33-gestion-de-accesos-ssh-keys)
   - [3.4. IPs Elàstiques](#34-ips-elastiques)
   - [3.5. Security Group — Firewall](#35-security-group--firewall)
   - [3.6. Usuari admintech](#36-usuari-admintech)
   - [3.7. Directorio Activo (OpenLDAP)](#37-directorio-activo-openldap)
   - [3.8. SFTP Seguro e Integración LDAP](#38-sftp-seguro-e-integracion-ldap)
   - [3.9. Centralización de Logs (Rsyslog)](#39-centralizacion-de-logs-rsyslog)
   - [3.10. Automatización con Ansible](#310-automatizacion-con-ansible)
4. [Implantación de Servicios Multimedia](#4-implantacion-de-servicios-multimedia)
   - [4.1. Servicio de Streaming de Audio](#41-servicio-de-streaming-de-audio)
   - [4.2. Servicio de Streaming de Vídeo](#42-servicio-de-streaming-de-video)
   - [4.3. Videoconferencia (Jitsi Meet)](#43-videoconferencia-jitsi-meet)
5. [Aplicación Web Corporativa](#5-aplicacion-web-corporativa)
6. [Diseño e Implementación de la Base de Datos](#6-diseno-e-implementacion-de-la-base-de-datos)
   - [6.1. Diseño Conceptual (E/R)](#61-diseno-conceptual-er)
   - [6.2. Diseño Lógico (Relacional)](#62-diseno-logico-relacional)
   - [6.3. Instalación y Securización de MariaDB](#63-instalacion-y-securizacion-de-mariadb)
   - [6.4. Script de Creación de Usuarios](#64-script-de-creacion-de-usuarios)
   - [6.5. Roles y Permisos](#65-roles-y-permisos)
   - [6.6. Triggers, Events y Auditoría](#66-triggers-events-y-auditoria)
   - [6.7. Incidencias y Soluciones BD](#67-incidencias-y-soluciones-bd)
7. [Comprobaciones de Rendimiento de Red](#7-comprobaciones-de-rendimiento-de-red)
8. [Seguridad logica monitorización y operaciones](#8-seguridad-logica-monitorizacion-y-operaciones)
9. [Digitalización y Sostenibilidad](#9-digitalizacion-y-sostenibilidad)
10. [Conclusiones](#10-conclusiones)
11. [Anexos y Entregables](#11-anexos-y-entregables)

---

## 1. Introducción y Contexto del Proyecto

El presente proyecto tiene como finalidad diseñar e implementar una infraestructura tecnológica para **InnovateTech**, una empresa en expansión dedicada a la provisión de servicios digitales. El núcleo de la propuesta es un modelo híbrido en el que los servicios principales de producción se ejecutan sobre AWS, mientras que el CPD local mantiene funciones de conectividad, seguridad, monitorización y respaldo.

InnovateTech experimenta un crecimiento acelerado en sus ventas online y una demanda crítica de soporte técnico. Para atender estas necesidades, el proyecto se enfoca en desplegar:

- **Gestión de Identidad:** Control centralizado de usuarios mediante LDAP.
- **Servicios Multimedia:** Plataformas de streaming de audio y vídeo, además de videoconferencia (Jitsi).
- **Persistencia de Datos:** Implementación de una base de datos relacional para la gestión operativa y auditoría.
- **Automatización y Monitorización:** Despliegue de procesos automatizados mediante Ansible y centralización de registros de eventos.

### Valores Técnicos Fundamentales

- **Seguridad:** Implementación de protocolos cifrados, gestión rigurosa de roles y segmentación de red.
- **Sostenibilidad:** Diseño eficiente del hardware local con el objetivo de minimizar el consumo energético.
- **Escalabilidad:** Arquitectura en la nube diseñada para adaptarse dinámicamente a picos de demanda.

[⬆ Volver al índice](#tabla-de-contenidos)

---

## 2. Propuesta de CPD Local (Infraestructura Física)

El Centro de Procesamiento de Datos (CPD) actúa como nodo de conectividad híbrida, respaldo y administración de la infraestructura desplegada en AWS.

### 2.1. Ubicación y Acondicionamiento

La sala técnica se ha acondicionado siguiendo normativas de seguridad y eficiencia:

<img width="8192" height="3097" alt="image" src="https://github.com/user-attachments/assets/3e784b52-bdbe-4efc-beb0-6219a25ca03a" />


[Enlace al plano lógico](https://mermaid.ai/d/1d8e0a02-9697-4f68-99f1-cd7f8237895b)

La infraestructura local de InnovateTech se ubica en una sala técnica dedicada, con acceso restringido únicamente al personal autorizado.

Dado que la mayor parte de los servicios críticos se encuentran desplegados en AWS, el CPD local tiene funciones principalmente administrativas, de respaldo y monitorización, por lo que no requiere un diseño de alta densidad térmica propio de grandes centros de datos.

### Características de acondicionamiento

La infraestructura física se ubica en una sala técnica interior con ventilación controlada y acceso restringido. Debido al reducido consumo térmico del CPD, no se requiere climatización de precisión dedicada. La temperatura se mantiene mediante el sistema de climatización general del edificio, configurado entre 24 °C y 26 °C.

- Sala interior protegida frente a humedad y polvo.
- Sistema de ventilación y climatización del edificio suficiente para mantener temperaturas estables entre 24 °C y 26 °C.
- Organización del cableado mediante canaletas y gestión vertical en rack.
- Sistema de alimentación protegido mediante SAI para evitar pérdidas de servicio ante microcortes eléctricos.
- Control de acceso mediante cerradura electrónica o llave física restringida.
- Protección contra incendios

### La protección contra incendios se basa en:

- Detectores ópticos de humo.
- Extintores específicos para equipos eléctricos (CO₂).
- Sistema de extinción localizado mediante aerosol condensado integrado en rack.

Esta solución reduce costes y complejidad respecto a sistemas de inundación total por gas inerte, siendo adecuada para una infraestructura de tamaño reducido.

### 2.2 Diseño de racks y organización

La infraestructura física del CPD local se organiza en dos racks independientes con el objetivo de separar las funciones de red y seguridad de los sistemas de administración y almacenamiento. Esta distribución facilita el mantenimiento, mejora la organización del cableado y simplifica futuras ampliaciones.

Dado que la mayor parte de los servicios corporativos se ejecutan en AWS, el CPD local mantiene únicamente servicios auxiliares, de conectividad y respaldo, reduciendo considerablemente la complejidad de la infraestructura física.

---

#### Rack 1 — Networking y Seguridad

Este rack concentra los dispositivos encargados de la conectividad interna, segmentación de red y enlace seguro con la infraestructura desplegada en AWS.

Equipamiento principal:

* **Patch panels** para la gestión del cableado estructurado.
* **Firewall perimetral** encargado de la seguridad de red y establecimiento del túnel VPN híbrido con AWS.
* **Switch Core Layer 3** para la distribución de VLANs y segmentación interna.
* **Router/SD-WAN** para gestión de conectividad WAN.
* **SAI** instalado en la parte inferior del rack para garantizar estabilidad eléctrica y autonomía ante cortes de suministro.

---

#### Rack 2 — Gestión y Administración

Este rack alberga los sistemas de soporte administrativo y almacenamiento local de respaldo.

Equipamiento principal:

* **Servidor de administración local** destinado a tareas internas de gestión y servicios auxiliares.
* **NAS corporativo** para almacenamiento de copias de seguridad y registros exportados desde AWS.
* **Servidor de monitorización** para supervisión de estado de servicios, consumo y eventos.
* **Consola KVM** para administración física de los equipos.
* **Electrónica auxiliar y organización de cableado**.



---

# 2.3 Infraestructura eléctrica y sistema SAI

## Descripción general

El CPD local adopta un modelo híbrido ligero orientado principalmente a tareas de:

* conectividad,
* administración,
* monitorización,
* seguridad,
* almacenamiento de copias de seguridad.

Los servicios críticos de producción, incluyendo aplicaciones web, streaming, bases de datos y servicios multimedia, se ejecutan sobre infraestructura AWS dentro de la VPC corporativa.

Esta arquitectura reduce significativamente:

* el consumo energético,
* la generación térmica,
* las necesidades de refrigeración,
* y la complejidad operativa del CPD local.

---

## Infraestructura eléctrica

Aunque el volumen de carga es reducido, la instalación mantiene criterios básicos de redundancia y continuidad de servicio.

### Alimentación eléctrica

La infraestructura dispone de:

* acometida eléctrica protegida,
* cuadro eléctrico dedicado,
* distribución mediante PDUs en rack,
* y sistemas SAI independientes para equipos críticos.

La separación de cargas permite aislar los sistemas de red de los sistemas de administración y almacenamiento.

---

## Equipamiento local y consumo estimado

### Rack 1 — Networking y Seguridad

| Equipo                 | Modelo                          | Consumo estimado |
| ---------------------- | ------------------------------- | ---------------- |
| Firewall principal     | FortiGate 60F                   | 18 W             |
| Firewall secundario HA | FortiGate 60F                   | 18 W             |
| Switch Core Layer 3    | Aruba CX 6100 24G 4SFP+         | 45 W             |
| Router / SD-WAN        | Ubiquiti EdgeRouter 4           | 11 W             |
| Electrónica auxiliar   | Patch panels, SFP+, ventilación | 20 W             |

**Consumo total Rack 1:**
112 W

---

### Rack 2 — Gestión y Administración

| Equipo                     | Modelo                | Consumo estimado |
| -------------------------- | --------------------- | ---------------- |
| Servidor de administración | Dell PowerEdge R250   | 110 W            |
| NAS corporativo            | QNAP TS-453D          | 35 W             |
| Servidor de monitorización | Intel NUC / appliance | 25 W             |
| Consola KVM y periféricos  | —                     | 10 W             |

**Consumo total Rack 2:**
180 W

---

## Potencia total estimada

| Rack   | Consumo |
| ------ | ------- |
| Rack 1 | 112 W   |
| Rack 2 | 180 W   |

**Consumo operativo total estimado:**
292 W

Aplicando un margen de crecimiento y seguridad del 25%:

[
P_{total}=292 \cdot 1.25 \approx 365\ W
]

**Potencia final de diseño:**
365 W

---

## Sistema SAI seleccionado

Para garantizar continuidad de servicio ante microcortes o fallos eléctricos, se propone el siguiente sistema SAI:

### Modelo recomendado

* **APC Smart-UPS SMTL1500RMI3UC**
* Tecnología Lithium-Ion
* Formato Rack 2U
* Potencia máxima:

  * 1500 VA
  * 1350 W
* Gestión remota mediante SNMP

El sistema proporciona autonomía suficiente para:

* apagado controlado de equipos,
* continuidad de conectividad,
* mantenimiento temporal de servicios administrativos.

---

## Autonomía estimada

Con una carga real aproximada de 365 W, el sistema SAI proporciona una autonomía estimada superior a 20 minutos, cumpliendo ampliamente los requisitos operativos del CPD.

Debido al bajo consumo derivado de la arquitectura híbrida con AWS, la autonomía disponible resulta considerablemente superior a la habitual en CPDs tradicionales.

---

## Consideraciones de eficiencia energética

La reducción de servicios locales permite minimizar:

* consumo eléctrico,
* disipación térmica,
* necesidad de refrigeración dedicada,
* y costes de mantenimiento.

La climatización general del edificio resulta suficiente para mantener condiciones operativas estables entre 24 °C y 26 °C, sin necesidad de sistemas industriales de refrigeración de precisión.

---

## Conclusión técnica

La adopción de una arquitectura híbrida basada en AWS permite simplificar considerablemente el CPD local manteniendo un nivel adecuado de:

* disponibilidad,
* seguridad,
* redundancia,
* y capacidad de administración.

El diseño final prioriza:

* eficiencia energética,
* reducción de costes,
* simplicidad operativa,
* y escalabilidad futura.

La infraestructura eléctrica y el sistema SAI seleccionados cubren adecuadamente las necesidades reales del entorno, proporcionando continuidad de servicio y protección frente a incidencias eléctricas.

[⬆ Volver al índice](#tabla-de-contenidos)

---
## 2.4 Seguridad Física

### Enfoque de diseño

La seguridad física del CPD local tiene como objetivo proteger los equipos encargados de la conectividad con AWS, la administración de la infraestructura y el almacenamiento de copias de seguridad.

Dado que los servicios críticos de producción se encuentran desplegados en AWS, las medidas adoptadas se han dimensionado para una infraestructura local de reducido tamaño, priorizando la protección de los activos físicos sin introducir complejidad o costes innecesarios.

---

### Control de acceso físico

El acceso a la sala técnica está restringido exclusivamente al personal autorizado de administración de sistemas.

Para reforzar la seguridad de los equipos, los racks disponen de:

* Cerradura de seguridad en puertas frontal y trasera.
* Control y registro de acceso mediante llave o sistema electrónico de apertura.
* Política de acceso basada en roles, limitando la intervención física a personal autorizado.

Estas medidas proporcionan un nivel adecuado de protección y trazabilidad para una infraestructura de tamaño reducido.

---

### Videovigilancia

La supervisión visual del entorno se realiza mediante una cámara IP instalada en la sala técnica con visión directa sobre los racks.

Las grabaciones se almacenan en el NAS corporativo, aprovechando la infraestructura existente y evitando la necesidad de sistemas de grabación dedicados.

Esta solución permite:

* Registrar accesos e intervenciones.
* Verificar incidencias de forma remota.
* Reducir costes de implantación y mantenimiento.

---

### Prevención y detección de incendios

La protección frente a incendios se basa en medidas preventivas y sistemas de detección temprana adaptados al tamaño del CPD.

#### Medidas preventivas

* Uso de cableado certificado LSZH (Low Smoke Zero Halogen).
* Correcta organización del cableado para evitar acumulaciones de calor.
* Supervisión de temperatura y humedad mediante sensores ambientales integrados en el sistema de monitorización.
* Ventilación adecuada de los racks para garantizar la correcta disipación térmica.

#### Sistemas de detección

* Detector óptico de humo instalado en la sala técnica.
* Sensores ambientales de temperatura y humedad conectados al sistema de monitorización.

Las alarmas generadas por estos dispositivos son enviadas automáticamente a la plataforma de monitorización para su gestión y seguimiento.

---

### Extinción de incendios

Debido al reducido tamaño de la infraestructura y a la baja carga térmica existente, no se considera necesaria la instalación de sistemas de extinción por inundación total mediante gas inerte.

La protección se realiza mediante:

* Extintor de CO₂ situado junto al acceso de la sala técnica.
* Procedimientos de actuación definidos para incidencias eléctricas o conatos de incendio.

Esta solución resulta adecuada para un entorno con dos racks y un número limitado de equipos electrónicos.

---

### Iluminación de emergencia y evacuación

La sala técnica dispone de iluminación de emergencia conforme a las medidas de seguridad generales del edificio.

Asimismo, la ubicación de los racks garantiza:

* Acceso seguro para tareas de mantenimiento.
* Ausencia de obstáculos en recorridos de evacuación.
* Espacio suficiente para la intervención del personal técnico.

---

### Integración con la monitorización

Los sensores ambientales y dispositivos de infraestructura se integran con la plataforma de monitorización corporativa.

La solución permite supervisar:

* Temperatura.
* Humedad.
* Estado de los SAI.
* Eventos de infraestructura.
* Disponibilidad de los servicios locales.

La centralización de estas alertas facilita una respuesta rápida ante cualquier incidencia y mejora la capacidad de gestión operativa.

---

### Resumen de medidas adoptadas

| Ámbito                 | Solución                                       |
| ---------------------- | ---------------------------------------------- |
| Control de acceso      | Racks con cerradura y acceso restringido       |
| Videovigilancia        | Cámara IP con almacenamiento en NAS            |
| Detección de incendios | Detector óptico de humo y sensores ambientales |
| Extinción de incendios | Extintor de CO₂                                |
| Monitorización         | Integración con plataforma centralizada        |
| Evacuación             | Iluminación de emergencia y accesos despejados |

Las medidas implementadas proporcionan un nivel de seguridad adecuado para una infraestructura híbrida donde los servicios críticos residen en AWS, manteniendo un equilibrio entre protección, simplicidad operativa y eficiencia económica.

## 3. Despliegue en el Núvol (AWS)

### Introducció

Tota la infraestructura de serveis d'InnovateTech s'ha desplegat al núvol AWS, regió `us-east-1` (N. Virginia). La solució inclou gestió centralitzada d'usuaris via LDAP, servidor web corporatiu amb PHP, transferència segura de fitxers per departament via SFTP, centralització de logs de totes les màquines, base de dades MariaDB i automatització completa amb Ansible.

### 3.1. Arquitectura de Red (VPC)

Una **VPC (Virtual Private Cloud)** és la xarxa privada virtual dins d'AWS que aïlla els recursos. És equivalent a tenir una xarxa local pròpia al núvol. Sense VPC, les instàncies no es poden comunicar entre elles de forma segura.

- **Nom:** `innovatetech-vpc`
- **CIDR:** `10.0.0.0/16`
- **Subxarxa pública:** 1
- **Internet Gateway:** creat i associat automàticament
- **NAT Gateway:** cap (redueix costos)
- **DNS hostnames:** activat

> 📸 **CAPTURA:** Diagrama de la VPC a la consola AWS.
> <img width="1662" height="766" alt="image" src="https://github.com/user-attachments/assets/6c61f9ae-6328-4e81-9c25-0b21634f03da" />

### 3.2. Instancias EC2

5 instàncies EC2 amb Ubuntu Server 24.04 LTS, t2.micro (1 vCPU, 1 GB RAM). Es va escollir Ubuntu 24.04 per la seva estabilitat i àmplia documentació. El tipus t2.micro és l'opció gratuïta de les comptes AWS Academy.

| Instància | Storage | Servei |
|-----------|---------|--------|
| innovatetech-web | 8 GiB | NGINX + PHP + SFTP |
| innovatetech-ldap | 8 GiB | OpenLDAP |
| innovatetech-logs | 8 GiB | Rsyslog + Ansible |
| innovatetech-db | 8 GiB | MariaDB |


La comunicació interna entre màquines es fa sempre per **IP privada**, que és permanent. Les IPs elàstiques s'utilitzen per a l'accés extern.

> 📸 **CAPTURA:** Llistat de les instàncies EC2 en estat running.
> <img width="1179" height="335" alt="image" src="https://github.com/user-attachments/assets/1160b450-a360-4d0f-bde8-62e9e2580be7" />

### 3.3. Gestión de Accesos (SSH Keys)

Per connectar-se a les instàncies EC2 de forma segura s'utilitza autenticació per clau pública/privada.

- **Nom:** `innovatetech-key`
- **Tipus:** RSA
- **Format:** `.pem`

**Permisos a Windows:**
```powershell
icacls "C:\Users\gamer\Downloads\innovatetech-key.pem" /inheritance:r
icacls "C:\Users\gamer\Downloads\innovatetech-key.pem" /grant:r "gamer:R"
```

**Permisos a Linux/Ubuntu:**
```bash
chmod 400 ~/Baixades/innovatetech-key.pem
```

> 📸 **CAPTURA:** Creació del Key Pair a la consola AWS.
> <img width="1675" height="202" alt="image" src="https://github.com/user-attachments/assets/0150a8fc-4c46-411a-a0ad-a0cd78e428d0" />

### 3.4. IPs Elàstiques

Les comptes d'AWS Academy canvien les IPs públiques a cada reinici del laboratori. Per solucionar-ho s'han assignat **IPs elàstiques** (estàtiques) a les màquines principals.

| Màquina | IP Privada (permanent) | IP Elàstica (permanent) |
|---------|----------------------|------------------------|
| innovatetech-ldap | 10.0.6.122 | 100.28.104.126 |
| innovatetech-db | 10.0.0.208 | 100.50.111.243 |
| innovatetech-logs | 10.0.9.98 | 3.208.185.55 |
| innovatetech-web | 10.0.7.135 | 35.171.63.1 |

> 📸 **CAPTURA:** Llistat d'IPs elàstiques a la consola AWS.
> <img width="1680" height="274" alt="image" src="https://github.com/user-attachments/assets/ce13248e-32cc-460e-a3eb-6398ed8ec02e" />

### 3.5. Security Group — Firewall

El Security Group és el firewall virtual d'AWS. S'ha configurat seguint el principi de **mínim privilegi**: els ports sensibles només són accessibles des de la xarxa interna `10.0.0.0/16`.

**Nom:** `innovatetech-sg`

| Port | Protocol | Origen | Servei |
|------|----------|--------|--------|
| 22 | TCP | 0.0.0.0/0 | SSH |
| 80 | TCP | 0.0.0.0/0 | HTTP |
| 443 | TCP | 0.0.0.0/0 | HTTPS |
| 389 | TCP | 10.0.0.0/16 | LDAP (intern) |
| 636 | TCP | 10.0.0.0/16 | LDAPS (intern) |
| 3306 | TCP | 10.0.0.0/16 | MariaDB (intern) |
| 514 | TCP/UDP | 10.0.0.0/16 | Syslog (intern) |
| 1935 | TCP | 0.0.0.0/0 | RTMP streaming |
| 8000 | TCP | 0.0.0.0/0 | Icecast àudio |
| 10000 | TCP/UDP | 0.0.0.0/0 | Jitsi Meet |

> 📸 **CAPTURA:** Regles d'entrada del Security Group.
> <img width="1629" height="560" alt="image" src="https://github.com/user-attachments/assets/3ade7b5c-33c9-4c14-b364-f6db428f977b" />

### 3.6. Usuari admintech

El projecte exigeix que les màquines s'administrin amb un usuari específic, no el per defecte (`ubuntu`). Es va crear l'usuari `admintech` a totes les màquines amb accés per clau pública/privada i sudo sense contrasenya per facilitar l'automatització amb Ansible.

```bash
sudo adduser --disabled-password --gecos '' admintech
sudo usermod -aG sudo admintech
sudo mkdir -p /home/admintech/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/admintech/.ssh/
sudo chown -R admintech:admintech /home/admintech/.ssh
sudo chmod 700 /home/admintech/.ssh
sudo chmod 600 /home/admintech/.ssh/authorized_keys
echo 'admintech ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/admintech
```

**Connexió SSH:**
```bash
# Linux/Ubuntu
ssh -i ~/Baixades/innovatetech-key.pem admintech@IP_MAQUINA

# Windows PowerShell
ssh -i "C:\Users\gamer\Downloads\innovatetech-key.pem" admintech@IP_MAQUINA
```

> 📸 **CAPTURA:** Connexió SSH amb usuari admintech.
> <img width="793" height="489" alt="image" src="https://github.com/user-attachments/assets/05e166b2-f98f-4be0-8536-2383f3c9ed51" />

### 3.7. Directorio Activo (OpenLDAP)

**OpenLDAP** centralitza la gestió d'usuaris i grups de l'empresa. Un usuari es crea una sola vegada i pot autenticar-se a múltiples serveis (SFTP, web) amb les mateixes credencials.

- **Servidor:** `innovatetech-ldap` (10.0.6.122 / 100.28.104.126)
- **Domini:** `innovatetech.local`
- **Admin:** `cn=admin,dc=innovatetech,dc=local` / contrasenya: `12345`

**Estructura del directori:**
```
dc=innovatetech,dc=local
├── ou=usuarios
│   ├── ou=vendes      → venda1, venda2, venda3
│   ├── ou=suport      → suport1, suport2, suport3
│   ├── ou=administracio → admin1, admin2, admin3
│   ├── ou=logistica   → logis1, logis2, logis3
│   └── ou=admin       → bd1, bd2, bd3
└── ou=grupos
    ├── cn=admin         (gid 3000)
    ├── cn=vendes        (gid 3001)
    ├── cn=administracio (gid 3002)
    ├── cn=treballador   (gid 3003)
    ├── cn=suport        (gid 3004)
    └── cn=logistica     (gid 3005)
```

| Departament | Usuaris | GID | Contrasenya |
|-------------|---------|-----|-------------|
| Vendes | venda1, venda2, venda3 | 3001 | 12345 |
| Administració | admin1, admin2, admin3 | 3002 | 12345 |
| Suport tècnic | suport1, suport2, suport3 | 3004 | 12345 |
| Logística | logis1, logis2, logis3 | 3005 | 12345 |
| Gestió BD | bd1, bd2, bd3 | 3000 | bd1234 |

**Instal·lació:**
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install slapd ldap-utils -y
sudo dpkg-reconfigure slapd
```

**Verificació:**
```bash
sudo systemctl status slapd
ldapsearch -x -H ldap://localhost -b "dc=innovatetech,dc=local"
```

> 📸 **CAPTURA:** ldapsearch mostrant tota l'estructura amb usuaris i grups.
> <img width="1045" height="473" alt="image" src="https://github.com/user-attachments/assets/298981c6-e26f-4b84-818b-3c283efc9cda" />

> 📸 **CAPTURA:** systemctl status slapd actiu.
> <img width="1106" height="363" alt="image" src="https://github.com/user-attachments/assets/50790549-2c8b-42ac-b417-a5dc45fd5c85" />

### 3.8. SFTP Seguro e Integración LDAP

El servei SFTP permet la transferència segura de fitxers. Cada departament té la seva pròpia carpeta i els usuaris queden confinats (chroot) a ella. L'autenticació es fa directament contra LDAP.

**Com funciona:**
1. L'usuari LDAP es connecta per SFTP amb uid i contrasenya
2. `libpam-ldap` verifica les credencials contra LDAP
3. `libnss-ldap` identifica el grup de l'usuari
4. SSH aplica la regla `Match Group` i confina l'usuari a la seva carpeta

**Paquets necessaris:**
```bash
sudo apt install libpam-ldap libnss-ldap ldap-utils nscd -y
```

**Estructura de carpetes:**
```
/sftp/
├── vendes/uploads/
├── suport/uploads/
├── administracio/uploads/
└── logistica/uploads/
```

**Configuració `/etc/ssh/sshd_config`:**
```
Match Group vendes
    ChrootDirectory /sftp/vendes
    ForceCommand internal-sftp
    AllowTcpForwarding no

Match Group administracio
    ChrootDirectory /sftp/administracio
    ForceCommand internal-sftp
    AllowTcpForwarding no

Match Group suport
    ChrootDirectory /sftp/suport
    ForceCommand internal-sftp
    AllowTcpForwarding no

Match Group logistica
    ChrootDirectory /sftp/logistica
    ForceCommand internal-sftp
    AllowTcpForwarding no
```

**Prova de connexió:**
```bash
sftp venda1@44.197.87.185    # entra a /sftp/vendes
sftp suport1@44.197.87.185   # entra a /sftp/suport
sftp admin1@44.197.87.185    # entra a /sftp/administracio
sftp logis1@44.197.87.185    # entra a /sftp/logistica
```

> 📸 **CAPTURA:** Connexió SFTP amb venda1 mostrant la carpeta uploads.
> <img width="380" height="168" alt="image" src="https://github.com/user-attachments/assets/c9faa7ae-8273-45be-ade5-700fa7c8cee3" />

> 📸 **CAPTURA:** Connexió SFTP amb els 4 departaments.
> <img width="370" height="315" alt="image" src="https://github.com/user-attachments/assets/b01d4477-33d0-40e0-818f-3fb878d177fa" />

### 3.9. Centralización de Logs (Rsyslog)

Rsyslog centralitza els registres de totes les màquines a `innovatetech-logs`. S'utilitza la **IP privada** `10.0.9.98` per a la comunicació interna ja que és permanent.

**Configuració del servidor** (`/etc/rsyslog.conf`):
```
module(load="imudp")
input(type="imudp" port="514")
module(load="imtcp")
input(type="imtcp" port="514")
```

**`/etc/rsyslog.d/remote.conf`:**
```
$template RemoteLogs,"/var/log/remote/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
```

**Configuració dels clients** (`/etc/rsyslog.d/client.conf`):
```
*.* @@10.0.9.98:514
```

**Verificació:**
```bash
sudo ls /var/log/remote/
```

> 📸 **CAPTURA:** `ls /var/log/remote/` mostrant les carpetes de totes les màquines.
> <img width="1050" height="144" alt="image" src="https://github.com/user-attachments/assets/db10733d-5c50-42e4-a4fc-0c1e3a373d95" />

### 3.10. Automatización con Ansible

Ansible automatitza la configuració de servidors des del node controlador `innovatetech-logs`. S'han creat playbooks capaços de crear instàncies EC2 des de zero i configurar-les completament.

**Estructura:**
```
~/ansible/
├── ansible.cfg
├── update-credentials.sh
├── inventory/hosts
├── roles/
│   ├── provision/tasks/main.yml
│   ├── web/tasks/main.yml + files/index.php
│   └── ldap/tasks/main.yml
├── playbook-web.yml
├── playbook-ldap.yml
├── playbook-eliminar-web.yml
├── playbook-provision-web.yml
└── playbook-provision-ldap.yml
```

**Inventari:**
```ini
[web]
10.0.7.135

[ldap]
10.0.6.122

[all:vars]
ansible_user=admintech
ansible_ssh_private_key_file=/home/admintech/.ssh/id_rsa
```

**Credencials AWS** — Les credencials AWS es gestionen amb `update-credentials.sh`. Cal actualitzar-les a cada sessió del lab:
```bash
nano ~/.aws/credentials
source ~/ansible/update-credentials.sh
```

**Playbooks:**

| Playbook | Funció |
|----------|--------|
| `playbook-web.yml` | Configura màquina web existent |
| `playbook-ldap.yml` | Configura màquina LDAP existent |
| `playbook-eliminar-web.yml` | Elimina NGINX (simulació fallada) |
| `playbook-provision-web.yml` | Crea EC2 nova + configura web complet |
| `playbook-provision-ldap.yml` | Crea EC2 nova + configura LDAP complet |

**Rol `provision`** — Crea una instància EC2, assigna IP elàstica i configura l'usuari admintech:
1. Crea EC2 (Ubuntu 24.04, t2.micro)
2. Espera SSH disponible
3. Assigna IP elàstica
4. Crea usuari `admintech` (connectant com a `ubuntu`)
5. Copia clau pública a `authorized_keys`
6. Configura sudo sense contrasenya

**Rol `web`** — Desplega el servidor web complet: NGINX + PHP 8.3 FPM, web corporativa `index.php`, integració LDAP per autenticació, SFTP amb chroot per departament.

**Rol `ldap`** — Instal·la OpenLDAP des de zero amb `debconf` per evitar preguntes interactives. Crea tota l'estructura: OUs, grups, 12 usuaris de departament i 3 usuaris de gestió BD (bd1, bd2, bd3).

**Execució:**
```bash
# Configurar màquines existents
cd ~/ansible
ansible-playbook -i inventory/hosts playbook-web.yml
ansible-playbook -i inventory/hosts playbook-ldap.yml

# Crear instàncies noves des de zero
source ~/ansible/update-credentials.sh
ansible-playbook playbook-provision-web.yml
ansible-playbook playbook-provision-ldap.yml

# Demostració recuperació de desastres
ansible-playbook -i inventory/hosts playbook-eliminar-web.yml
ansible-playbook -i inventory/hosts playbook-web.yml
```

**Verificació:**
```bash
cd ~/ansible && ansible all -m ping
```

> 📸 **CAPTURA:** `ansible all -m ping` amb totes les màquines en SUCCESS.
> <img width="568" height="259" alt="image" src="https://github.com/user-attachments/assets/88a8b353-07b8-4857-b52c-68c47e229ac6" />

> 📸 **CAPTURA:** Execució de `playbook-provision-web.yml` completada.
> <img width="1050" height="230" alt="image" src="https://github.com/user-attachments/assets/027d94fc-b1e4-48bb-866e-75f5e9aa2f6f" />

> 📸 **CAPTURA:** Execució de `playbook-provision-ldap.yml` completada.
> <img width="1116" height="236" alt="image" src="https://github.com/user-attachments/assets/f679891b-b5c3-46ca-bad9-b0780c8755c8" />

**Problemes i solucions Ansible:**

| Problema | Causa | Solució |
|----------|-------|---------|
| NoCredentialsError | El mòdul `amazon.aws` no llegia les variables d'entorn | Passar credencials via `lookup('env', ...)` + `source update-credentials.sh` |
| skipping: no hosts matched | La IP nova no estava a l'inventari | Usar `add_host` als `post_tasks` |
| Permission denied al crear admintech | Tasques `delegate_to` usaven `admintech` en lloc de `ubuntu` | Afegir `vars: ansible_user: ubuntu` |
| index.php es descarregava | Virtual Host sense PHP-FPM | Instal·lar `php8.3-fpm` + bloc `location ~ \.php$` |
| PasswordAuthentication bloquejada | AWS crea `60-cloudimg-settings.conf` amb `no` | `sed -i` sobre aquest fitxer específic |
| AddressLimitExceeded | Límit de 5 IPs elàstiques a Academy | Alliberar IPs de les instàncies de prova |

[⬆ Volver al índice](#tabla-de-contenidos)

---

## 4. Implantación de Servicios Multimedia

### 4.1. Servicio de Streaming de Audio

> Servidor: AWS EC2 — Ubuntu Server 22.04 LTS — IP: `34.225.147.8`

El servidor de audio ofrece dos modalidades: **audio bajo demanda** (archivos MP3 servidos por Nginx) y **streaming en directo** (Icecast2 en formato OGG/Vorbis).

**Security Group `servicios-multimedia`:**

| Puerto | Protocolo | Servicio |
|--------|-----------|---------|
| 22 | TCP | SSH |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 1935 | TCP | RTMP |
| 8000 | TCP | Icecast2 |
| 8080 | TCP | Nginx |

**Instalación Nginx:**
```bash
sudo apt update
sudo apt install -y nginx libnginx-mod-rtmp
```

**Verificación:**
```bash
nginx -v
sudo systemctl status nginx
<img width="1264" height="441" alt="image" src="https://github.com/user-attachments/assets/9617ca5e-8607-45a9-82fa-2c4f58665fad" />

sudo ss -tlnp | grep 8080
curl -I http://localhost:8080
<img width="754" height="262" alt="image" src="https://github.com/user-attachments/assets/0ae0ff2b-01fc-476c-a84b-eb233aeff171" />

```

**Audio bajo demanda — Nginx:**
```bash
sudo mkdir -p /var/www/html/audio
sudo wget -O /var/www/html/audio/audio1.mp3 "https://download.samplelib.com/mp3/sample-3s.mp3"
sudo wget -O /var/www/html/audio/audio2.mp3 "https://download.samplelib.com/mp3/sample-6s.mp3"
sudo wget -O /var/www/html/audio/audio3.mp3 "https://download.samplelib.com/mp3/sample-9s.mp3"
sudo ffmpeg -f lavfi -i sine=frequency=440:duration=30 \
  -c:a libmp3lame -b:a 128k /var/www/html/audio/audio4.mp3
sudo chown -R www-data:www-data /var/www/html/audio
```
<img width="818" height="187" alt="image" src="https://github.com/user-attachments/assets/a5d61c8e-406e-43f9-bbf7-b9b092a36bfa" />

<img width="1000" height="266" alt="image" src="https://github.com/user-attachments/assets/0ed60dc7-7b94-450f-8bc2-ce906d99c9bd" />

**Streaming en directo — Icecast2:**
```bash
sudo apt install -y icecast2
```
<img width="1254" height="511" alt="image" src="https://github.com/user-attachments/assets/882a4203-a9e9-496f-a0e6-243734c107ae" />


| Parámetro | Valor |
|-----------|-------|
| `source-password` | `12345` |
| Puerto | `8000` |
| Mount point | `/stream.ogg` |

**Verificación:**
```bash
sudo systemctl status icecast2
curl -v http://localhost:8000/stream.ogg --output /dev/null 2>&1 | head -20
```
<img width="1265" height="419" alt="image" src="https://github.com/user-attachments/assets/e80f686d-857b-487e-a9c9-bbd7674c8716" />

<img width="1089" height="554" alt="image" src="https://github.com/user-attachments/assets/d715185c-7fce-4903-9633-1c27c9326e25" />

<img width="1255" height="718" alt="image" src="https://github.com/user-attachments/assets/ea8e63b4-f816-41a4-9cf9-cf8693daeb34" />


**Protocolos utilizados:**

| Protocolo | Uso |
|-----------|-----|
| HTTP | Nginx sirve los archivos MP3 en el puerto 8080 |
| Icecast / HTTP Streaming | Icecast2 sirve el stream en el puerto 8000 |
| OGG/Vorbis | Formato del stream en directo |
| MP3 | Formato de los archivos bajo demanda |
| RTMP | Puerto 1935, módulo Nginx instalado |

### 4.2. Servicio de Streaming de Vídeo

> Servidor: AWS EC2 — Ubuntu Server 22.04 LTS — IP: `54.227.77.10`

El servicio de vídeo funciona en modo **VOD (Video on Demand)**. Los archivos MP4 con códec H.264 se almacenan en el servidor y se reproducen bajo demanda desde el navegador con **VideoJS**.

**Creación y descarga de vídeos:**
```bash
sudo mkdir -p /var/www/html/videos
sudo chown -R www-data:www-data /var/www/html/videos
sudo chmod -R 755 /var/www/html/videos

sudo wget -O /var/www/html/videos/video1.mp4 "https://download.samplelib.com/mp4/sample-5s.mp4"
sudo wget -O /var/www/html/videos/video2.mp4 "https://download.samplelib.com/mp4/sample-10s.mp4"
sudo wget -O /var/www/html/videos/video3.mp4 "https://download.samplelib.com/mp4/sample-15s.mp4"
sudo ffmpeg -f lavfi -i testsrc=duration=30:size=1280x720:rate=30 \
  -f lavfi -i sine=frequency=440:duration=30 -c:v libx264 -c:a aac \
  /var/www/html/videos/video4.mp4
sudo chown -R www-data:www-data /var/www/html/videos
```
<img width="830" height="218" alt="image" src="https://github.com/user-attachments/assets/797e12c5-1052-4137-be47-6ce324c52bda" />


**Verificación:**
```bash
curl -I http://localhost:8080/videos/video1.mp4
```
<img width="1015" height="316" alt="image" src="https://github.com/user-attachments/assets/f882031c-ffec-4fb4-8e5e-fca8d8d80f9a" />

**Protocolos utilizados:**

| Protocolo | Uso |
|-----------|-----|
| HTTP | Nginx sirve los archivos MP4 en el puerto 8080 |
| H.264 | Códec de vídeo de los archivos MP4 |
| HLS | Módulo RTMP configurado (preparado para streaming en directo) |

**Resultados:**
- ✅ Nginx operativo en el puerto 8080 — vídeo MP4 (H.264) y audio MP3 bajo demanda
- ✅ Icecast2 operativo en el puerto 8000 — streaming en directo OGG/Vorbis
- ✅ Interfaz web unificada en `http://34.225.147.8:8080`
  
<img width="1273" height="739" alt="image" src="https://github.com/user-attachments/assets/a664b9ec-e706-4b6f-9e2b-38b6869bf679" />

### 4.3. Videoconferencia (Jitsi Meet)

> Instalación nativa sobre Ubuntu 22.04 — EC2 sin Docker — IP: `54.227.77.10`

Jitsi Meet es una plataforma de videoconferencia de código abierto. Se instala de forma nativa sobre una instancia EC2 con Ubuntu 22.04, usando una IP elástica pública.

| Parámetro | Valor |
|-----------|-------|
| **IP elástica** | `54.227.77.10` |
| **IP privada** | `172.31.36.193` |
| **Hostname** | `jitsi-meet` |

**Paso 1 — Preparar el sistema:**
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apt-transport-https curl gnupg2 wget nginx software-properties-common
sudo hostnamectl set-hostname jitsi-meet
echo "127.0.0.1 jitsi-meet" | sudo tee -a /etc/hosts
echo "54.227.77.10 jitsi-meet" | sudo tee -a /etc/hosts
sudo apt install -y openjdk-11-jdk
```
<img width="815" height="354" alt="image" src="https://github.com/user-attachments/assets/9d8b5f54-3bcb-482d-ab4c-694545c27b26" />

<img width="1081" height="153" alt="image" src="https://github.com/user-attachments/assets/784f8c67-cc6e-496f-9a7f-f502ccbe6a1f" />

<img width="1205" height="295" alt="image" src="https://github.com/user-attachments/assets/94443cd7-29b4-4ea0-8547-57228a040928" />

**Paso 2 — Repositorio oficial de Jitsi:**
```bash
curl https://download.jitsi.org/jitsi-key.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/jitsi-key.gpg
echo "deb [signed-by=/usr/share/keyrings/jitsi-key.gpg] https://download.jitsi.org stable/" | \
  sudo tee /etc/apt/sources.list.d/jitsi-stable.list
sudo apt update
```

**Paso 3 — Instalar Jitsi Meet:**
```bash
sudo apt install -y jicofo jitsi-videobridge2 jitsi-meet
```

**Paso 4 — Configurar Prosody:**
```bash
sudo prosodyctl cert generate jitsi-meet
sudo prosodyctl cert generate auth.jitsi-meet
JVB_PASS=$(openssl rand -hex 16)
FOCUS_PASS=$(openssl rand -hex 16)
sudo prosodyctl register jvb auth.jitsi-meet $JVB_PASS
sudo prosodyctl register focus auth.jitsi-meet $FOCUS_PASS
sudo systemctl restart prosody
```
<img width="997" height="75" alt="image" src="https://github.com/user-attachments/assets/07fb83a4-3245-4449-b796-273aa95a8345" />

<img width="1225" height="300" alt="image" src="https://github.com/user-attachments/assets/3b55bb4f-f88c-485e-8495-97a9dedc4752" />


**Configuració NAT per AWS** (`/etc/jitsi/videobridge/jvb.conf`):
```hocon
ice4j {
    harvest {
        mapping {
            static-mappings = [{
                local-address = "172.31.36.193"
                public-address = "54.227.77.10"
            }]
        }
    }
}
```
<img width="915" height="734" alt="image" src="https://github.com/user-attachments/assets/7cc5483e-48fa-4630-9a56-432d645d20a1" />

> ⚠️ **Lección aprendida:** la versión JVB 2.3-291 lee la configuración XMPP únicamente de `sip-communicator.properties`, con claves en **MAYÚSCULAS**. Sin el mapeado NAT correcto, ICE no puede negociar los candidatos de media.>

<img width="1262" height="192" alt="image" src="https://github.com/user-attachments/assets/0ef373c7-481b-4474-811f-4f425c2bc767" />

<img width="1251" height="386" alt="image" src="https://github.com/user-attachments/assets/3e737ef6-d0a2-4b5e-8180-ad426d0cbd26" />

<img width="566" height="770" alt="image" src="https://github.com/user-attachments/assets/2cf5fb46-f7b0-4b01-9126-5411314958af" />

**Protocolos utilizados:**

| Protocolo | Uso |
|-----------|-----|
| XMPP | Señalización entre componentes (Prosody) |
| BOSH | Tunneling de XMPP sobre HTTPS |
| WebRTC | Comunicación en tiempo real del navegador |
| ICE / STUN | Negociación de ruta entre peers |
| SRTP | Transmisión segura de audio/vídeo |
| RTP/RTCP | Transporte de paquetes de media (UDP 10000) |

**Puertos necesarios en Security Group:**

| Puerto | Protocolo | Uso |
|--------|-----------|-----|
| 80 | TCP | Redirección HTTP → HTTPS |
| 443 | TCP | Frontend web y BOSH |
| 4443 | TCP | JVB fallback TCP |
| 10000 | UDP | **Media RTP/RTCP (crítico)** |

[⬆ Volver al índice](#tabla-de-contenidos)

---

## 5. Aplicación Web Corporativa

L'aplicació web corporativa d'InnovateTech s'ha desplegat a `innovatetech-web` amb NGINX + PHP 8.3 FPM.

**Accés:**
- IP: http://44.197.87.185
- Domini: http://innovatetech-itb.duckdns.org

**Virtual Host `/etc/nginx/sites-available/innovatetech`:**
```nginx
server {
    listen 80;
    server_name _;
    root /var/www/innovatetech;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    }
}
```

L'aplicació té tres seccions:

**Inici** — Pàgina corporativa amb estadístiques i accés ràpid als serveis.

**Streaming** — Enllaços als serveis multimèdia:
- Àudio: http://34.225.147.8:8080 (Icecast)
- Vídeo: https://54.227.77.10 (NGINX-RTMP)

**Gestió BD** — Sistema de gestió de base de dades amb autenticació LDAP. Accés restringit als usuaris `bd1`, `bd2`, `bd3` (contrasenya: `bd1234`). Permet visualitzar, inserir i eliminar registres de totes les taules.

**Autenticació:**
1. L'usuari introdueix `uid_ldap` i contrasenya
2. El sistema verifica contra LDAP (`10.0.6.122`)
3. Comprova que el `uid` comenci per `bd`
4. Si és correcte, dóna accés a la gestió

> 📸 **CAPTURA:** Pàgina d'inici de la web corporativa.
> <img width="2559" height="1438" alt="image" src="https://github.com/user-attachments/assets/281f7c64-fbf4-43aa-945f-2525636bca09" />

> 📸 **CAPTURA:** Secció de streaming amb els dos serveis.
> <img width="2531" height="636" alt="image" src="https://github.com/user-attachments/assets/245e8002-fb57-4755-9f5e-38501bdcf1db" />

> 📸 **CAPTURA:** Login de gestió BD.
> <img width="2537" height="720" alt="image" src="https://github.com/user-attachments/assets/ee0a2f7e-6250-4fd4-99d3-83813ef46ac5" />

> 📸 **CAPTURA:** Taula d'empleats amb dades.
> <img width="2515" height="956" alt="image" src="https://github.com/user-attachments/assets/a13d3431-8518-4f10-9bc3-9fcc62853f81" />

> 📸 **CAPTURA:** Formulari d'inserció de registre.
> <img width="1617" height="276" alt="image" src="https://github.com/user-attachments/assets/fefcde57-e9f3-4b35-a35a-1fa73ec746f9" />

[⬆ Volver al índice](#tabla-de-contenidos)

---

## 6. Diseño e Implementación de la Base de Datos

### 6.1. Diseño Conceptual (E/R)

El diagrama Entitat-Relació representa les 14 entitats de la base de dades d'InnovateTech, els seus atributs principals i les relacions entre elles amb la cardinalitat corresponent.

<img width="1832" height="1304" alt="image" src="https://github.com/user-attachments/assets/c8f4e5f1-8b2d-4add-8716-a14f1eee41e4" />
 Diagrama E/R exportat de dbdiagram.io mostrant totes les entitats i relacions.

### 6.2. Diseño Lógico (Relacional)

A partir del diagrama E/R s'ha obtingut l'esquema relacional complet:

```
DEPARTAMENTS (codi PK, nom, telefon)
EMPLEATS (dni PK, nom, cognoms, adreca, telefon, codi_dept FK→DEPARTAMENTS)
CLIENTS (id PK, nom, email, telefon, empresa)
CONFIG_QUALITAT (id PK, nivell, resolucio_video, bitrate_audio, amplada_banda_min)
USUARIS (id PK, uid_ldap, nom_complet, email, extensio, estat, tipus,
         dni_empleat FK→EMPLEATS, id_client FK→CLIENTS,
         id_config FK→CONFIG_QUALITAT, url_videotrucada)
PRODUCTES (id PK, nom, descripcio, preu, tipus)
COMANDES (id PK, data, estat, quantitat,
          id_client FK→CLIENTS, id_producte FK→PRODUCTES)
CISTELL (id PK, quantitat, data_afegit,
         id_client FK→CLIENTS, id_producte FK→PRODUCTES)
CATALEG_VIDEOS (id PK, titol, descripcio, categoria, durada, data_publicacio, url_streaming)
TRUCADES (id PK, inici, fi, durada, puntuacio, comentari,
          id_origen FK→USUARIS, id_desti FK→USUARIS, id_config FK→CONFIG_QUALITAT)
MESURES_AMPLADA_BANDA (id PK, equip_mesurat, data_hora, baixada, pujada,
                       latencia, resultat, notes, id_operari FK→USUARIS)
CONFIG_SERVIDOR (id PK, parametre, valor, descripcio)
TAULA_AVISOS (id PK, usuari_db, taula_afectada, operacio, data_hora, detalls)
CONTROL_BACKUP (id PK, data_hora, taules_incloses, resultat)
```

<img width="469" height="555" alt="image" src="https://github.com/user-attachments/assets/cd730bee-f70f-4222-962c-ae1915fba028" />
 Resultat de `SHOW TABLES` a MariaDB mostrant les 14 taules creades.

### 6.3. Instalación y Securización de MariaDB

S'ha escollit **MariaDB 10.11** com a SGBD per la seva compatibilitat amb MySQL, lleugeresa i per ser de codi obert.

**Instal·lació:**
```bash
sudo apt install mariadb-server -y
sudo mysql_secure_installation
```

**Configuració accés remot** (`/etc/mysql/mariadb.conf.d/50-server.cnf`):
```
bind-address = 0.0.0.0
event_scheduler = ON
default-time-zone = 'Europe/Madrid'
```

**Usuari d'administració:**
```sql
CREATE USER 'admin'@'%' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
GRANT FILE ON *.* TO 'admin'@'%';
FLUSH PRIVILEGES;
```

| Paràmetre | Valor |
|-----------|-------|
| Host intern | `10.0.0.208` |
| Host extern | `100.50.111.243` |
| Port | `3306` |
| Usuari | `admin` |

<img width="1092" height="561" alt="image" src="https://github.com/user-attachments/assets/90834699-6ac8-4367-8e65-d61e0209eb49" />
< `systemctl status mariadb` mostrant el servei actiu.

### 6.4. Script de Creación de Usuarios

S'ha creat un script Bash (`create_users.sh`) que automatitza la creació d'usuaris a MariaDB. L'script demana interactivament les dades mínimes, valida el rol, comprova si l'usuari ja existeix i genera un fitxer `.sql` d'auditoria.

**Funcionalitats:**
- Creació interactiva d'un o més usuaris
- Validació del rol (admin, vendes, administracio, treballador)
- Detecció d'usuaris duplicats
- Generació automàtica de `usuaris_creats.sql`
- `GRANT FILE` automàtic per als usuaris amb rol `admin`

```bash
#!/bin/bash
DB_HOST="localhost"
DB_PORT="3306"
DB_NAME="innovatetech"
DB_ADMIN="admin"
DB_PASS="12345"
OUTPUT_FILE="usuaris_creats.sql"
ROLS_VALIDS=("admin" "vendes" "administracio" "treballador")

rol_valid() {
    local rol=$1
    for r in "${ROLS_VALIDS[@]}"; do
        if [[ "$r" == "$rol" ]]; then return 0; fi
    done
    return 1
}

usuari_existeix() {
    local usuari=$1
    local host=$2
    local resultat=$(mysql -h "$DB_HOST" -P "$DB_PORT" \
        -u "$DB_ADMIN" -p"$DB_PASS" \
        -sse "SELECT COUNT(*) FROM mysql.user
              WHERE User='$usuari' AND Host='$host';" 2>/dev/null)
    [[ "$resultat" -gt 0 ]]
}

crear_usuari() {
    local usuari=$1
    local contrasenya=$2
    local rol=$3
    local host=$4

    if usuari_existeix "$usuari" "$host"; then
        echo "[ERROR] L'usuari '$usuari'@'$host' ja existeix. Saltant..."
        return 1
    fi

    if ! rol_valid "$rol"; then
        echo "[ERROR] El rol '$rol' no és vàlid."
        return 1
    fi

    local sql_create="CREATE USER '$usuari'@'$host' IDENTIFIED BY '$contrasenya';"
    local sql_grant="GRANT '$rol' TO '$usuari'@'$host';"
    local sql_default="SET DEFAULT ROLE '$rol' FOR '$usuari'@'$host';"
    local sql_file=""
    if [[ "$rol" == "admin" ]]; then
        sql_file="GRANT FILE ON *.* TO '$usuari'@'$host';"
    fi

    mysql -h "$DB_HOST" -P "$DB_PORT" \
        -u "$DB_ADMIN" -p"$DB_PASS" \
        -e "$sql_create $sql_grant $sql_default ${sql_file:+$sql_file} FLUSH PRIVILEGES;" 2>/dev/null

    if [[ $? -eq 0 ]]; then
        echo "[OK] Usuari '$usuari'@'$host' creat amb rol '$rol'."
        echo "-- Usuari: $usuari | Rol: $rol" >> "$OUTPUT_FILE"
        echo "$sql_create" >> "$OUTPUT_FILE"
        echo "$sql_grant" >> "$OUTPUT_FILE"
        echo "$sql_default" >> "$OUTPUT_FILE"
        [[ -n "$sql_file" ]] && echo "$sql_file" >> "$OUTPUT_FILE"
    fi
}

echo "-- INNOVATETECH - Usuaris generats: $(date '+%Y-%m-%d %H:%M:%S')" > "$OUTPUT_FILE"
echo "Rols disponibles: ${ROLS_VALIDS[*]}"

while true; do
    read -p "Nom d'usuari (o 'sortir' per acabar): " usuari
    [[ "$usuari" == "sortir" ]] && break
    read -p "Contrasenya: " contrasenya
    read -p "Rol: " rol
    read -p "Host (per defecte '%'): " host
    [[ -z "$host" ]] && host="%"
    crear_usuari "$usuari" "$contrasenya" "$rol" "$host"
done

echo "Fitxer SQL generat: $OUTPUT_FILE"
```

<img width="736" height="536" alt="image" src="https://github.com/user-attachments/assets/9061216f-bb98-4c89-82e2-6296b28ceae2" />

Execució de `create_users.sh` creant un usuari amb rol `admin`.

<img width="733" height="358" alt="image" src="https://github.com/user-attachments/assets/3384f1fb-8fb3-4276-96e3-08edea3fdaee" />

Contingut de `usuaris_creats.sql` mostrant les sentències `CREATE USER` i `GRANT`.

### 6.5. Roles y Permisos

S'han creat 4 rols a MariaDB seguint el principi de mínim privilegi:

| Rol | Permisos |
|-----|----------|
| `admin` | `ALL PRIVILEGES` + `GRANT FILE` |
| `vendes` | `SELECT/INSERT/UPDATE` sobre Clients, Comandes, Productes, Cistell, Trucades, Usuaris, Config_Qualitat |
| `administracio` | `SELECT/INSERT/UPDATE` sobre Empleats, Departaments, Usuaris, Config_Qualitat, Mesures_Amplada_Banda |
| `treballador` | `SELECT` sobre Productes, Cataleg_Videos, Config_Qualitat + `SELECT/INSERT` sobre Trucades |

```sql
CREATE ROLE 'admin';
CREATE ROLE 'vendes';
CREATE ROLE 'administracio';
CREATE ROLE 'treballador';
```

> <img width="1023" height="281" alt="image" src="https://github.com/user-attachments/assets/15b886f7-17da-4c36-8c47-2f3724e2daf1" />
 `SELECT Host, User, is_role FROM mysql.user WHERE is_role='Y'` mostrant els 4 rols.
> <img width="1121" height="437" alt="image" src="https://github.com/user-attachments/assets/636724d7-40ff-4bc9-913c-4f5454c021c6" />
 `SHOW GRANTS FOR 'vendes'` i `SHOW GRANTS FOR 'administracio'`.

### 6.6. Triggers, Events y Auditoría

S'han implementat 6 triggers i 1 event periòdic:

| Trigger | Funció |
|---------|--------|
| `trg_bloqueig_usuari` | Impedeix trucades si l'usuari origen o destí està bloquejat |
| `trg_quota_minuts_mensuals` | Bloqueja noves trucades si l'usuari supera els 600 minuts mensuals |
| `trg_quota_trucades_diaries` | Bloqueja noves trucades si l'usuari supera les 20 trucades diàries |
| `trg_audit_empleats_update` | Registra intents de modificar `Empleats` per usuaris no autoritzats |
| `trg_audit_comandes_delete` | Registra i bloqueja intents d'eliminar registres de `Comandes` |
| `trg_audit_trucades_admin` | Registra intents del rol `administracio` d'insertar a `Trucades` |

Tots els intents bloquejats queden registrats a `Taula_Avisos` amb: usuari, taula afectada, operació, data/hora i detalls.

**Event periòdic de backup** (`evt_backup_diari`) — S'executa cada dia a les 02:00 i exporta les taules crítiques (`Empleats`, `Clients`, `Comandes`, `Trucades`) en format CSV a `/var/backups/innovatetech/`. Cada execució queda registrada a `Control_Backup`.

```sql
SHOW EVENTS FROM innovatetech;
SHOW VARIABLES LIKE 'event_scheduler';
```

<img width="2359" height="189" alt="image" src="https://github.com/user-attachments/assets/cc73131d-f5b1-47eb-8f2c-214094e254a0" />
 `SHOW EVENTS FROM innovatetech` mostrant `evt_backup_diari` amb estat `ENABLED`.
> <img width="2016" height="731" alt="image" src="https://github.com/user-attachments/assets/de199b2e-53b1-4eb1-8022-65e87974b95e" />
 `SELECT * FROM Taula_Avisos` mostrant registres reals de triggers disparats.

**Automatització de mesures d'amplada de banda:**

Un script Bash (`test_amplada_bo.sh`) executa `speedtest-cli`, avalua si el resultat és acceptable (baixada >50 Mbit/s) i insereix automàticament els resultats a la taula `Mesures_Amplada_Banda`.

**Planificació cron:**
```cron
0 22-23,0-6 * * * /home/ubuntu/scripts/test_amplada_bo.sh >> /var/log/speedtest.log 2>&1
*/15 6-13 * * * /home/ubuntu/scripts/test_amplada_bo.sh >> /var/log/speedtest.log 2>&1
*/30 14-21 * * * /home/ubuntu/scripts/test_amplada_bo.sh >> /var/log/speedtest.log 2>&1
```

### 6.7. Incidencias y Soluciones BD

**Taula_Avisos usa motor MyISAM** — Necessari perquè els triggers que insereixen a aquesta taula s'executen dins de transaccions que poden ser revertides. Amb MyISAM, les insercions a `Taula_Avisos` són permanents independentment del resultat de la transacció principal.

**ERROR 1901 — CHECK clause no suportat** — MariaDB no admet restriccions `CHECK` que comparin columnes entre si. Solució: es va traslladar la lògica de validació als triggers.

[⬆ Volver al índice](#tabla-de-contenidos)

---

## 7. Comprobaciones de Rendimiento de Red

Les proves s'han realitzat entre les dues instàncies EC2 desplegades a la mateixa VPC (`us-east-1`), usant `iperf3` per mesurar l'amplada de banda real de la xarxa interna.

| Instància | Serveis | IP privada | IP pública |
|-----------|---------|------------|------------|
| Instància 1 | Servidor Audio/Vídeo (Nginx + Icecast2) | `172.31.17.184` | `34.225.147.8` |
| Instància 2 | Servidor Videoconferència (Jitsi Meet) | `172.31.36.193` | `54.227.77.10` |

```bash
sudo apt install -y iperf3
iperf3 -s -D   # servidor a Instància 1
```
<img width="1219" height="170" alt="image" src="https://github.com/user-attachments/assets/3261bfe7-4818-4441-b3a2-44452f18d9fb" />

**Prova 1 — Instància 2 → Instància 1:**
```bash
iperf3 -c 172.31.17.184 -p 5201 -t 10        # Download
<img width="1085" height="555" alt="image" src="https://github.com/user-attachments/assets/0e260609-4760-4d87-982d-d593eba488e0" />

iperf3 -c 172.31.17.184 -p 5201 -t 10 -R     # Upload
<img width="1061" height="577" alt="image" src="https://github.com/user-attachments/assets/06286965-852b-483b-8149-83ba06f8d04e" />

```

**Prova 2 — Instància 1 → Instància 2:**
```bash
iperf3 -c 172.31.36.193 -p 5201 -t 10
```

<img width="1092" height="544" alt="image" src="https://github.com/user-attachments/assets/b1d015c2-8f5a-483e-b983-fcfeb8a6e8d7" />


iperf3 -c 172.31.36.193 -p 5201 -t 10 -R
```

<img width="1117" height="584" alt="image" src="https://github.com/user-attachments/assets/153c88e2-5518-428e-9cf0-4e5f3db03b9e" />


**Resum de resultats:**

| Prova | Direcció | Download | Upload | Latència |
|-------|---------|----------|--------|---------|
| Prova 1 | Jitsi → Audio/Vídeo | 1.02 Gbits/s | 1.02 Gbits/s | <1ms |
| Prova 2 | Audio/Vídeo → Jitsi | 1.02 Gbits/s | 1.02 Gbits/s | <1ms |

**Anàlisi per servei:**

| Servei | Bitrate | Usuaris simultanis possibles |
|--------|---------|------------------------------|
| Streaming àudio (Icecast2) | 0.128 Mbits/s | ~7.968 |
| Streaming vídeo VOD | 2-5 Mbits/s | 204-510 |
| Videoconferència Jitsi | 1-4 Mbits/s | 255-1.020 |

> ✅ **SISTEMA CLASSIFICAT COM: ACCEPTABLE** — 1.02 Gbits/s, connexió simètrica, latència <1ms.

**Propostes de millora:**

| Millora | Prioritat |
|---------|-----------|
| CDN (AWS CloudFront) | Alta — reduir latència per a clients finals llunyans |
| Load Balancer | Mitjana — distribuir càrrega si el nombre d'usuaris creix |
| AWS CloudWatch | Mitjana — monitorar l'amplada de banda en producció |

[⬆ Volver al índice](#tabla-de-contenidos)

---
## 8. Seguridad lógica, monitorización y operaciones

### Copias de seguridad

La estrategia de copias de seguridad sigue la regla **3-2-1**, manteniendo varias copias de la información en ubicaciones diferentes.

#### Copias locales

El NAS corporativo QNAP TS-453D actúa como repositorio principal de copias de seguridad.

Se almacenan:

- Configuraciones de red.
- Configuración de firewall.
- Datos del servidor de administración.
- Bases de datos de monitorización.
- Configuraciones de AWS exportadas periódicamente.

#### Copias remotas

Además del almacenamiento local, las copias críticas se replican automáticamente hacia un bucket de almacenamiento en AWS.

Esto permite disponer de:

- Protección frente a fallos físicos del NAS.
- Recuperación ante desastres.
- Conservación de versiones históricas.

#### Automatización

La ejecución de los backups se realiza mediante tareas programadas.

**Servidores Linux**

- `rsync`
- `tar`
- Scripts Bash automatizados

**Servidores Windows**

- Windows Server Backup
- Scripts PowerShell

Las copias se ejecutan diariamente y se verifican periódicamente mediante pruebas de restauración.

---

### Configuración de almacenamiento del NAS

El NAS QNAP TS-453D utiliza **4 discos configurados en RAID 5**.

Esta configuración ofrece:

- Tolerancia al fallo de un disco.
- Buen equilibrio entre capacidad y seguridad.
- Aprovechamiento eficiente del almacenamiento.

Se ha seleccionado RAID 5 porque la carga principal del sistema consiste en almacenamiento de copias de seguridad y registros, donde la capacidad disponible tiene mayor importancia que el rendimiento extremo de escritura.

> Nota: aunque el NAS admite RAID 10, esta opción no aporta ventajas significativas en el escenario actual debido a que los servicios productivos se ejecutan en AWS.

---

### Seguridad lógica

Para reforzar la protección de la infraestructura se aplican las siguientes medidas:

- Segmentación de red mediante VLANs.
- Acceso administrativo exclusivamente mediante VPN.
- Uso de HTTPS, SSH y protocolos cifrados.
- Gestión centralizada de identidades mediante LDAP.
- Política de mínimos privilegios para usuarios y administradores.
- Actualizaciones periódicas de sistemas operativos y aplicaciones.
- Registro y auditoría de accesos mediante ELK.

---

## 9. Prevención de riesgos laborales y sostenibilidad

La infraestructura híbrida de InnovateTech ha sido diseñada siguiendo criterios de seguridad operativa, eficiencia energética y sostenibilidad. La migración de los servicios productivos a AWS ha reducido significativamente la complejidad del CPD local, permitiendo mantener únicamente los equipos necesarios para garantizar la conectividad, la administración y las copias de seguridad corporativas.

### Prevención de riesgos laborales (PRL)

Los riesgos asociados al CPD se han adaptado a las dimensiones reales de la infraestructura local y a la baja densidad de equipamiento instalado.

#### Riesgo eléctrico

Toda intervención sobre cuadros eléctricos, SAIs, PDUs o sistemas de alimentación queda restringida a personal técnico autorizado y debidamente cualificado. Los equipos disponen de protección frente a sobrecargas y se encuentran conectados a sistemas de alimentación ininterrumpida que garantizan la continuidad del servicio ante incidencias de la red eléctrica.

#### Riesgo de incendio

La protección contra incendios se basa en las medidas descritas en la sección de seguridad física:

- Sensores de humo instalados en los racks.
- Monitorización de temperatura y humedad.
- Sistemas de extinción automática mediante aerosol condensado.
- Extintor manual de CO₂ para intervención inicial.

Estas medidas permiten detectar y contener de forma temprana cualquier incidencia sin necesidad de sistemas de extinción de gran escala.

#### Orden, limpieza y seguridad en el trabajo

El cableado se organiza mediante bandejas y guías de gestión para evitar obstáculos y facilitar las tareas de mantenimiento. Asimismo, se mantiene una zona de trabajo libre alrededor de los racks para minimizar riesgos de golpes, tropiezos o caídas durante las operaciones de administración.

#### Ruido y condiciones ambientales

Debido al reducido número de equipos instalados, los niveles sonoros permanecen por debajo de los 60 dB, por lo que no se requiere protección auditiva específica. La ventilación general del edificio y la ventilación integrada de los racks permiten mantener unas condiciones adecuadas de funcionamiento sin recurrir a sistemas de climatización de precisión.

---

### Sostenibilidad y eficiencia energética

La arquitectura híbrida adoptada permite reducir significativamente el consumo energético y la huella ambiental respecto a un CPD tradicional completamente local.

#### Reducción de infraestructura física

La ejecución de los servicios principales en AWS elimina la necesidad de mantener múltiples servidores físicos de producción en las instalaciones de la empresa. Esto reduce tanto el consumo eléctrico como los requisitos de refrigeración y mantenimiento.

#### Monitorización energética

Los SAIs APC instalados en la infraestructura proporcionan métricas de consumo eléctrico mediante SNMP, integradas en la plataforma de monitorización. Esta información permite analizar tendencias de consumo y detectar posibles desviaciones energéticas.

#### Optimización del cableado y equipamiento

La distribución física de los equipos se ha diseñado para minimizar la longitud del cableado y simplificar las tareas de mantenimiento. Además, los dispositivos auxiliares, como la consola KVM y la pantalla de administración, permanecen apagados cuando no se utilizan.

#### Gestión eficiente de la climatización

La carga térmica total del CPD es inferior a 500 W, por lo que no se requiere climatización de precisión ni sistemas avanzados de refrigeración. La ventilación general del edificio y la ventilación propia de los racks son suficientes para mantener temperaturas de funcionamiento adecuadas.

### Beneficios obtenidos

La estrategia adoptada proporciona las siguientes ventajas:

- Reducción del consumo energético global.
- Menor generación de calor en las instalaciones.
- Disminución de costes operativos y de mantenimiento.
- Reducción de la huella de carbono asociada a la infraestructura local.
- Mayor aprovechamiento de recursos mediante escalado dinámico en AWS.
- Incremento de la vida útil de los equipos locales al asumir una carga de trabajo reducida.
- Simplificación de la operación diaria del CPD.

En conjunto, la combinación de servicios en la nube y una infraestructura local ligera permite a InnovateTech disponer de una plataforma tecnológica eficiente, segura y alineada con criterios actuales de sostenibilidad y optimización de recursos.
[⬆ Volver al índice](#tabla-de-contenidos)

---

## 10. Conclusiones

El proyecto InnovateTech ha culminado con el despliegue de una infraestructura tecnológica híbrida que cubre, de forma integrada, todos los requisitos planteados en el enunciado. No se trata únicamente de un conjunto de servicios funcionando en paralelo: es un ecosistema donde cada pieza ha sido diseñada para complementar a las demás.

La decisión de centralizar la identidad en **OpenLDAP** ha sido, probablemente, la más transversal de todo el proyecto. Un único directorio de usuarios alimenta el SFTP, la autenticación web, la gestión de la base de datos y, potencialmente, cualquier servicio futuro que se incorpore. Esto reduce la superficie de administración, evita la proliferación de credenciales y establece un punto de control único: cuando un empleado causa baja, una sola modificación en LDAP lo desactiva en todos los sistemas simultáneamente.

La apuesta por **Ansible** como motor de automatización ha demostrado su valor más allá de la simple instalación de paquetes. Los playbooks de provisión son capaces de crear una instancia EC2 desde cero en AWS, asignarle una IP elástica, crear el usuario de administración y dejar el servicio completamente operativo en menos de cinco minutos. Esto transforma la infraestructura en código reproducible: cualquier máquina que falle puede ser reemplazada de forma idéntica sin intervención manual, lo que en un entorno de producción real representaría una reducción drástica del tiempo de recuperación ante desastres (RTO).

La **base de datos** ha ido mucho más allá de un simple almacén de datos. Con 14 tablas interrelacionadas, 6 triggers de auditoría y seguridad, un evento periódico de backup y un sistema de roles granular, se ha construido una capa de datos que es, al mismo tiempo, funcional y segura. La decisión de implementar los triggers con `SIGNAL SQLSTATE` en lugar de confiarlo todo a la aplicación garantiza que las restricciones de seguridad se aplican independientemente del cliente que acceda a la base de datos.

Los **servicios multimedia** —Icecast2, NGINX-RTMP y Jitsi Meet— han supuesto el reto técnico más complejo del proyecto. La instalación nativa de Jitsi en AWS requirió resolver el problema del mapeado NAT entre la IP privada de la VPC y la IP elástica pública, y entender que la versión JVB 2.3-291 ignora la configuración HOCON de `jvb.conf` para la conexión XMPP, leyéndola exclusivamente de `sip-communicator.properties` con claves en mayúsculas. Esta clase de problemas, que no aparecen en ningún tutorial, son los que distinguen un despliegue real de uno de laboratorio, y el haberlos resuelto representa la parte de mayor valor formativo de todo el proyecto.

Las **pruebas de rendimiento de red** han revelado que la infraestructura interna de AWS en la misma zona de disponibilidad ofrece 1.02 Gbits/s simétricos con latencia inferior a 1 ms, lo que garantiza la cobertura de todos los servicios desplegados con un margen amplísimo. Esta capacidad no es mérito del diseño, sino de la plataforma subyacente, y subraya una de las ventajas fundamentales de la nube: el ancho de banda interno es prácticamente ilimitado sin coste adicional.

Desde el punto de vista de la **sostenibilidad**, el modelo híbrido adoptado también tiene implicaciones positivas. Concentrar los servicios de alta disponibilidad en AWS elimina la necesidad de mantener hardware propio sobredimensionado para cubrir picos de demanda puntuales. El CPD local, más pequeño y dimensionado para la carga base, consume menos energía y genera menos calor que una sala de servidores tradicional.

Como reflexión final, este proyecto ha puesto de manifiesto que la tecnología, por sí sola, no resuelve nada. Lo que transforma una lista de servicios en una infraestructura coherente es la comprensión de cómo interactúan entre sí: cómo el directorio LDAP alimenta el SFTP, cómo el servidor web consulta la base de datos, cómo Ansible orquesta la creación de recursos en AWS o cómo Jitsi negocia los candidatos ICE a través del NAT de la VPC. Esa capacidad de ver el sistema como un todo, y no como una suma de partes, es precisamente lo que define el perfil de un administrador de sistemas competente.

[⬆ Volver al índice](#tabla-de-contenidos)

---

## 11. Anexos y Entregables

El CPD local utiliza una segmentación VLAN simple pero funcional, orientada a seguridad, separación de tráfico y gestión eficiente en un entorno híbrido con AWS.
<img width="1939" height="973" alt="image" src="https://github.com/user-attachments/assets/7b7ec19c-63b4-4600-8f30-a6ab5e974f5d" />


| VLAN | Nombre         | Subred          | Gateway      | Uso                     |
| ---- | -------------- | --------------- | ------------ | ----------------------- |
| 10   | Gestión        | 192.168.10.0/24 | 192.168.10.1 | Administración, bastion |
| 20   | Servidores     | 192.168.20.0/24 | 192.168.20.1 | NAS, servicios locales  |
| 30   | Monitorización | 192.168.30.0/24 | 192.168.30.1 | Zabbix / métricas       |
| 40   | Usuarios       | 192.168.40.0/24 | 192.168.40.1 | Acceso interno          |
| 50   | Backup         | 192.168.50.0/24 | 192.168.50.1 | Tráfico NAS             |

| Origen    | Destino  | Puerto          | Acción   | Descripción              |
| --------- | -------- | --------------- | -------- | ------------------------ |
| VLAN10    | Todas    | SSH / RDP       | PERMITIR | Administración           |
| VLAN20    | AWS VPC  | HTTPS           | PERMITIR | Sincronización servicios |
| VLAN30    | Todos    | SNMP / HTTP API | PERMITIR | Monitorización           |
| VLAN40    | Internet | HTTP/HTTPS      | PERMITIR | Acceso usuarios          |
| VLAN50    | NAS      | SMB/NFS         | PERMITIR | Backups internos         |
| Cualquier | VLAN10   | -               | DENEGAR  | Protección gestión       |

[⬆ Volver al índice](#tabla-de-contenidos)

---

*Documentació del Projecte Transversal ASIXc1 — InnovateTech*
*Curs 25/26 · Institut Tecnològic de Barcelona*
