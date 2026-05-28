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
3.1 [Despliegue en el Núvol (AWS)](#3-despliegue-en-el-nuvol-aws)
   * [3.1.1 Arquitectura de Red (VPC)](#31-arquitectura-de-red-vpc)
   * [3.2.2 Instancias EC2](#32-instancias-ec2)
   * [3.3.3 Gestión de Accesos (SSH Keys)](#33-gestion-de-accesos-ssh-keys)
3.2. [Servicios de Red y Gestión de Identidad](#5-servicios-de-red-y-gestion-de-identidad)
   * [3.1.1 Directorio Activo (AD/LDAP)](#51-directorio-activo-adldap)
   * [3.2.2. SFTP Seguro e Integración](#52-sftp-seguro-e-integracion)
   * [3.2.3. Centralización de Logs](#53-centralizacion-de-logs)
   * [3.2.4. Automatización con Ansible](#34-automatizacion-con-ansible)
4. [Implantación de Servicios Multimedia](#4-implantacion-de-servicios-multimedia)
   * [4.1. Servicio de Streaming de Audio](#41-servicio-de-streaming-de-audio)
   * [4.2. Servicio de Streaming de Vídeo](#42-servicio-de-streaming-de-video)
   * [4.3. Videoconferencia (Jitsi Meet)](#43-videoconferencia-jitsi-meet)

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
<br><br>

_[Enlace al plano logico](https://mermaid.ai/d/1d8e0a02-9697-4f68-99f1-cd7f8237895b)_


## 2. Propuesta de CPD Local (Infraestructura Física) <a name="2-propuesta-de-cpd-local-infraestructura-fisica"></a>
 
El Centro de Procesamiento de Datos (CPD) local se ha concebido como el centro neurálgico de administración y conectividad de InnovateTech. Su diseño físico prioriza la integridad del hardware y la continuidad del servicio.
 
La arquitectura híbrida adoptada asume que todos los servicios críticos de producción —aplicaciones web, streaming, bases de datos, LDAP y gestión de logs— residen en AWS, mientras que el CPD local actúa como nodo de administración, monitorización y redundancia de datos. Esta decisión de diseño reduce el dimensionamiento físico del CPD, pero no exime de exigir los mismos estándares de disponibilidad y seguridad que un centro de datos convencional.
 
---
 
### 2.1. Ubicación y Acondicionamiento <a name="21-ubicacion-y-acondicionamiento"></a>
 
La infraestructura física de InnovateTech se encuentra alojada en un edificio de varias plantas. La planta destinada al CPD local alberga los elementos de red, administración y backup, mientras que el resto de plantas concentran los puestos de trabajo de los distintos departamentos de la empresa. Esta distribución exige una arquitectura de cableado cuidadosamente planificada que garantice conectividad de alta calidad tanto hacia los servicios en la nube de AWS como entre los propios equipos locales.
 
Desde el punto de vista de la **seguridad física**, la sala no dispondrá de ventanas ni señalización que identifique su contenido. El diseño de puertas y paredes se integrará con el resto del edificio, empleando materiales resistentes que mantengan la discreción. Las rutas de acceso no serán evidentes ni estarán conectadas con los accesos principales. El control de acceso se implementará mediante cerradura electrónica con registro de entradas para personal autorizado, complementado con videovigilancia CCTV que cubrirá toda la zona de infraestructura.
 
Para el **acondicionamiento térmico**, se implementará un sistema de aire acondicionado de precisión que mantendrá la temperatura constante entre **20 °C y 22 °C**. El diseño aplica la metodología de pasillos fríos y calientes: el aire frío es impulsado a través del suelo técnico elevado (30 cm) hacia los frontales de los racks, mientras que el aire caliente es extraído por la parte posterior y recirculado hacia las unidades de climatización a través del falso techo técnico. Para los servicios desplegados en la nube, la climatización es responsabilidad directa del proveedor.
 
El **suelo técnico elevado** (30 cm) permitirá la canalización ordenada del cableado y de los sistemas de climatización de forma segura y flexible. El **falso techo técnico** facilitará la instalación de equipos de ventilación, iluminación y la distribución del retorno de aire caliente, manteniendo la sala organizada y accesible para el mantenimiento.
 
El sistema de **detección de incendios** contará con sensores ópticos de humo y sensores de temperatura y humedad. La extinción automática se realizará mediante **gas inerte (Novec o CO₂)**, que sofoca el fuego sin dañar los componentes electrónicos ni dejar residuos.
 
**Cableado e infraestructura de red**
 
Una conectividad robusta y bien estructurada es condición indispensable para que InnovateTech pueda operar con fluidez tanto en su entorno local como sobre los servicios de AWS. La planificación del cableado abarca tres niveles diferenciados: el cableado interno del CPD, la distribución horizontal hacia las plantas de trabajo y el enlace de salida a internet.
 
Para las conexiones internas dentro del CPD se utilizará **fibra óptica monomodo**, que ofrece anchos de banda muy elevados y latencias mínimas, siendo la opción idónea para los uplinks entre switches y para cualquier conexión que requiera alta velocidad en distancias medias o largas dentro del edificio. Para las conexiones directas entre servidores, patch panels y switches de acceso se empleará **cableado estructurado Cat 7**, garantizando velocidades de **10 Gbps** con una excelente inmunidad al ruido electromagnético gracias a su apantallamiento individual por par y global (S/FTP).
 
La distribución del cableado hacia las plantas de trabajo seguirá un diseño de red jerárquica con capa de acceso, distribución y núcleo, canalizando el tráfico de los distintos departamentos de forma segmentada mediante VLANs. Todo el cableado estará **etiquetado e inventariado** para facilitar el mantenimiento y reducir los tiempos de intervención ante incidencias. La separación física entre cableado de datos y cableado eléctrico será obligatoria en todas las canalizaciones, evitando interferencias electromagnéticas que puedan degradar el rendimiento de la red.
 
Para la conectividad con AWS, se dispondrá de acceso a internet de alta capacidad con **redundancia de proveedor**, garantizando rutas alternativas de conexión que mantengan la disponibilidad en caso de fallo de uno de los enlaces. Con el objetivo de minimizar la latencia y maximizar la fiabilidad del tráfico hacia la nube, se valorará la implementación de tecnologías **SD-WAN**, que permiten gestionar de forma inteligente el tráfico entre múltiples enlaces y priorizar los flujos críticos de negocio. Esta aproximación es funcionalmente equivalente, en un entorno empresarial de escala media, a los principios de baja latencia y alta fiabilidad que ofrecen tecnologías como MPLS o las conexiones dedicadas de fibra óptica en escenarios de múltiples CPDs internacionales. El túnel **VPN Site-to-Site** cifrado garantizará la confidencialidad e integridad de todas las comunicaciones entre el CPD local y la VPC corporativa en AWS.
 
[⬆ Volver al índice](#-tabla-de-contenidos)
 
---
 
### 2.2. Diseño de Racks y Organización <a name="22-diseno-de-racks-y-organizacion"></a>
 
La infraestructura local de InnovateTech se concentra en **dos armarios rack de 42U**, que separan funcionalmente los equipos de red de los de administración y servicios. Dado que el CPD local tiene un alcance deliberadamente limitado —su función es la monitorización, la administración de usuarios y el backup—, el dimensionamiento es compacto pero diseñado con los mismos criterios de resiliencia que un CPD convencional.
 
**Rack 1 — Red y Seguridad (Alta Disponibilidad)**
 
Alberga los elementos que garantizan la comunicación interna y el enlace con los servicios en la nube:
 
- **Patch panels:** centralización y gestión del cableado estructurado proveniente de los puestos de trabajo de todas las plantas del edificio.
- **Router de borde y Firewall (activo/pasivo):** responsables de la seguridad perimetral, el filtrado de tráfico y el mantenimiento del túnel VPN Site-to-Site con AWS. Configurados en alta disponibilidad activo-pasivo con sincronización de estado entre ambos nodos.
- **Switch Core capa 3 (stacking):** distribución de alta velocidad con segmentación en VLANs para Administración, Servidores, Monitorización y Usuarios.
**Rack 2 — Gestión y Administración (Servicios Locales)**
 
Orientado al soporte administrativo y a la protección de datos local:
 
- **Bastion Host / Jump Server:** punto de acceso seguro y auditado para la administración remota de los servicios tanto locales como en la nube.
- **Servidor de administración local:** controlador de dominio secundario y gestión de políticas internas de la organización.
- **NAS corporativo:** almacenamiento dedicado a las copias de seguridad de la base de datos y los logs procedentes de AWS, asegurando una copia de los datos críticos fuera de la nube y bajo control directo de InnovateTech.
- **Consola KVM:** administración física de los servidores sin necesidad de periféricos individuales, permitiendo intervención directa incluso cuando los sistemas operativos no responden.
- **Servidor de monitorización:** supervisión en tiempo real de la temperatura, el consumo energético y el estado de los servicios mediante herramientas como Zabbix o Prometheus, con alertas configuradas para notificar al equipo de sistemas ante cualquier anomalía.
[⬆ Volver al índice](#-tabla-de-contenidos)
 
---
 
### 2.3. Infraestructura Eléctrica (SAI) <a name="23-infraestructura-electrica-sai"></a>
 
La continuidad del suministro eléctrico es un requisito crítico para cualquier CPD. InnovateTech implementará un **SAI redundante (UPS online)** ubicado en el Rack 1, que proporcionará alimentación ininterrumpida a todos los elementos críticos de red. El diseño online de doble conversión garantiza que los equipos estén siempre alimentados desde la batería, eliminando cualquier microcorte procedente de la red eléctrica. La autonomía estará dimensionada para permitir un apagado ordenado de los sistemas en caso de corte de suministro prolongado, evitando pérdidas de datos y daños en el hardware.
 
Adicionalmente, se contempla la instalación de un **grupo electrógeno** como respaldo de segundo nivel para escenarios de corte eléctrico prolongado, garantizando así la continuidad operativa de los servicios de administración y backup durante el tiempo necesario para restablecer el suministro principal.
 
[⬆ Volver al índice](#-tabla-de-contenidos)
 
---
 
### 2.4. Seguridad Física y PRL <a name="24-seguridad-fisica-y-prl"></a>
 
La seguridad física del CPD de InnovateTech se articula en múltiples capas complementarias que cubren tanto la prevención de accesos no autorizados como la protección frente a riesgos medioambientales y laborales.
 
En materia de **control de acceso**, se implementará un sistema de cerradura electrónica con identificación RFID y registro automatizado de todas las entradas y salidas del personal autorizado. El sistema de videovigilancia CCTV cubrirá de forma continua la zona de infraestructura, con retención de grabaciones conforme a la normativa vigente de protección de datos.
 
Para la **detección y extinción de incendios**, la sala dispondrá de sensores ópticos de humo y sensores combinados de temperatura y humedad conectados a una central de alarmas. El sistema de extinción automática empleará gas inerte (Novec o CO₂), tecnología que sofoca el incendio por desplazamiento del oxígeno sin generar residuos conductores ni dañar los componentes electrónicos, a diferencia de los sistemas de agua o polvo.
 
Desde el punto de vista de la **Prevención de Riesgos Laborales (PRL)**, la sala contará con señalización de emergencia, iluminación de seguridad autónoma y un protocolo de evacuación específico para el personal técnico. El acceso estará restringido al mínimo número de personas necesario para las tareas de mantenimiento, y cualquier intervención en los equipos bajo tensión seguirá los procedimientos establecidos por el Reglamento Electrotécnico de Baja Tensión (REBT) y la normativa de seguridad eléctrica aplicable.
 
[⬆ Volver al índice](#-tabla-de-contenidos)
### 2.3. Infraestructura Eléctrica (SAI) <a name="23-infraestructura-electrica-sai"></a>
*(Contenido aquí...)*

### 2.4. Seguridad Física y PRL <a name="24-seguridad-fisica-y-prl"></a>
*(Contenido aquí...)*
[⬆ Volver al índice](#-tabla-de-contenidos)

## 3. Despliegue en el Núvol (AWS) <a name="3-despliegue-en-el-nuvol-aws"></a>

Índex


[Introducció]
[Arquitectura general]
[Par de claus SSH]
[VPC]
[Security Group]
[Instàncies EC2]
[IPs elàstiques]
[Usuari admintech]
[OpenLDAP]
[NGINX + PHP + Web corporativa]
[SFTP autenticat amb LDAP]
[Rsyslog — Centralització de logs]
[MariaDB]
[Ansible]
[Aplicació web — Gestió BD i Streaming]
[Problemes i solucions]
---
1. Introducció
Aquest document recull tota la infraestructura desplegada al núvol AWS per a l'empresa fictícia InnovateTech, una empresa dedicada a la provisió de serveis tecnològics. L'objectiu és dissenyar i implementar un Centre de Processament de Dades (CPD) virtual al núvol que doni suport a totes les operacions de l'empresa.
La infraestructura inclou gestió centralitzada d'usuaris via LDAP, servidor web corporatiu amb PHP, transferència segura de fitxers per departament via SFTP, centralització de logs de totes les màquines, base de dades MariaDB i automatització completa amb Ansible.
Tota la infraestructura s'ha desplegat a la regió `us-east-1` (N. Virginia) d'AWS.
---
2. Arquitectura general
Màquina	Servei	IP Privada	IP Elàstica
innovatetech-ldap	OpenLDAP	10.0.6.122	100.28.104.126
innovatetech-db	MariaDB	10.0.0.208	100.50.111.243
innovatetech-logs	Rsyslog + Ansible	10.0.9.98	3.208.185.55
innovatetech-web	NGINX + PHP + SFTP	10.0.7.135	35.171.63.1

La comunicació interna entre màquines es fa sempre per IP privada, que és permanent. Les IPs elàstiques s'utilitzen per a l'accés extern.
> 📸 **CAPTURA:** Panell EC2 mostrant les 4 instàncies en estat running.
<img width="900" height="346" alt="image" src="https://github.com/user-attachments/assets/8f099e21-a334-4320-a059-64d25d64fccf" />

---
3. Par de claus SSH
Per connectar-se a les instàncies EC2 de forma segura s'utilitza autenticació per clau pública/privada. Es va crear un par de claus RSA des de la consola AWS.
Nom: `innovatetech-key`
Tipus: RSA
Format: `.pem`
Permisos a Windows:
```powershell
icacls "C:\Users\gamer\Downloads\innovatetech-key.pem" /inheritance:r
icacls "C:\Users\gamer\Downloads\innovatetech-key.pem" /grant:r "gamer:R"
```
Permisos a Linux/Ubuntu:
```bash
chmod 400 ~/Baixades/innovatetech-key.pem
```
> �📸 **CAPTURA:** Creació del Key Pair a la consola AWS.
> <img width="1675" height="202" alt="image" src="https://github.com/user-attachments/assets/0150a8fc-4c46-411a-a0ad-a0cd78e428d0" />


---
4. VPC
Una VPC (Virtual Private Cloud) és la xarxa privada virtual dins d'AWS que aïlla els recursos. És equivalent a tenir una xarxa local pròpia al núvol.
Nom: `innovatetech-vpc`
CIDR: `10.0.0.0/16`
Subxarxa pública: 1
Internet Gateway: creat i associat automàticament
NAT Gateway: cap (redueix costos)
DNS hostnames: activat
> 📸 **CAPTURA:** Diagrama de la VPC a la consola AWS.
> <img width="1662" height="766" alt="image" src="https://github.com/user-attachments/assets/6c61f9ae-6328-4e81-9c25-0b21634f03da" />
---
5. Security Group
El Security Group és el firewall virtual d'AWS. S'ha configurat seguint el principi de mínim privilegi: els ports sensibles només són accessibles des de la xarxa interna.
Nom: `innovatetech-sg`
Port	Protocol	Origen	Servei
22	TCP	0.0.0.0/0	SSH
80	TCP	0.0.0.0/0	HTTP
443	TCP	0.0.0.0/0	HTTPS
389	TCP	10.0.0.0/16	LDAP (intern)
636	TCP	10.0.0.0/16	LDAPS (intern)
3306	TCP	10.0.0.0/16	MariaDB (intern)
514	TCP/UDP	10.0.0.0/16	Syslog (intern)
1935	TCP	0.0.0.0/0	RTMP streaming
8000	TCP	0.0.0.0/0	Icecast àudio
10000	TCP/UDP	0.0.0.0/0	Jitsi Meet
> 📸 **CAPTURA:** Regles d'entrada del Security Group.
> <img width="1629" height="560" alt="image" src="https://github.com/user-attachments/assets/3ade7b5c-33c9-4c14-b364-f6db428f977b" />

---
6. Instàncies EC2
5 instàncies EC2 amb Ubuntu Server 24.04 LTS, t2.micro (1 vCPU, 1 GB RAM). Es va escollir Ubuntu 24.04 per la seva estabilitat i àmplia documentació. El tipus t2.micro és l'opció gratuïta de les comptes AWS Academy.
Instància	Storage	Servei
innovatetech-web	8 GiB	NGINX + PHP + SFTP
innovatetech-ldap	8 GiB	OpenLDAP
innovatetech-logs	8 GiB	Rsyslog + Ansible
innovatetech-db	8 GiB	MariaDB
> 📸 **CAPTURA:** Llistat de les 4 instàncies EC2 en estat running amb les comprovacions en verd.
> <img width="1179" height="335" alt="image" src="https://github.com/user-attachments/assets/1160b450-a360-4d0f-bde8-62e9e2580be7" />

---
7. IPs elàstiques
Les comptes d'AWS Academy canvien les IPs públiques a cada reinici del laboratori. Per solucionar-ho s'han assignat IPs elàstiques (estàtiques) a les màquines principals.
Màquina	IP Privada (permanent)	IP Elàstica (permanent)
innovatetech-ldap	10.0.6.122	100.28.104.126
innovatetech-db	10.0.0.208	100.50.111.243
innovatetech-logs	10.0.9.98	3.208.185.55
innovatetech-web	10.0.7.135	35.171.63.1
> 📸 **CAPTURA:** Llistat d'IPs elàstiques a la consola AWS.
> <img width="1680" height="274" alt="image" src="https://github.com/user-attachments/assets/ce13248e-32cc-460e-a3eb-6398ed8ec02e" />

---
8. Usuari admintech
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
Connexió SSH:
```bash
# Linux/Ubuntu
ssh -i ~/Baixades/innovatetech-key.pem admintech@IP_MAQUINA

# Windows PowerShell
ssh -i "C:\Users\gamer\Downloads\innovatetech-key.pem" admintech@IP_MAQUINA
```
> 📸 **CAPTURA:** Connexió SSH amb usuari admintech.
> <img width="793" height="489" alt="image" src="https://github.com/user-attachments/assets/05e166b2-f98f-4be0-8536-2383f3c9ed51" />

---
9. OpenLDAP
OpenLDAP centralitza la gestió d'usuaris i grups de l'empresa. Un usuari es crea una sola vegada i pot autenticar-se a múltiples serveis (SFTP, web) amb les mateixes credencials.
Servidor: `innovatetech-ldap` (10.0.6.122 / 100.28.104.126)
Domini: `innovatetech.local`
Admin: `cn=admin,dc=innovatetech,dc=local` / contrasenya: `12345`
Estructura del directori
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
Usuaris
Departament	Usuaris	GID	Contrasenya
Vendes	venda1, venda2, venda3	3001	12345
Administració	admin1, admin2, admin3	3002	12345
Suport tècnic	suport1, suport2, suport3	3004	12345
Logística	logis1, logis2, logis3	3005	12345
Gestió BD	bd1, bd2, bd3	3000	bd1234
Instal·lació
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install slapd ldap-utils -y
sudo dpkg-reconfigure slapd
```
Verificació
```bash
sudo systemctl status slapd
ldapsearch -x -H ldap://localhost -b "dc=innovatetech,dc=local"
ldapsearch -x -H ldap://localhost -b "ou=usuarios,dc=innovatetech,dc=local"
```
> 📸 **CAPTURA:** ldapsearch mostrant tota l'estructura amb usuaris i grups.

> <img width="1045" height="473" alt="image" src="https://github.com/user-attachments/assets/298981c6-e26f-4b84-818b-3c283efc9cda" />

> 📸 **CAPTURA:** systemctl status slapd actiu.

> <img width="1106" height="363" alt="image" src="https://github.com/user-attachments/assets/50790549-2c8b-42ac-b417-a5dc45fd5c85" />

---
10. NGINX + PHP + Web corporativa
NGINX és el servidor web instal·lat a `innovatetech-web`. S'ha configurat amb PHP 8.3 FPM per servir l'aplicació web corporativa d'InnovateTech.
Instal·lació
```bash
sudo apt install nginx php8.3-fpm php8.3-mysql php8.3-ldap -y
sudo mkdir -p /var/www/innovatetech
```
Virtual Host `/etc/nginx/sites-available/innovatetech`
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
Accés
IP: http://44.197.87.185
Domini: http://innovatetech-itb.duckdns.org (DuckDNS gratuït)
http://innovatetech-itb.duckdns.org/
> 📸 **CAPTURA:** Pàgina web d'InnovateTech al navegador.

> <img width="2559" height="1438" alt="image" src="https://github.com/user-attachments/assets/dbf675ec-3a07-4f73-a910-133262a8c8ba" />

---
11. SFTP autenticat amb LDAP
El servei SFTP permet la transferència segura de fitxers. Cada departament té la seva pròpia carpeta i els usuaris queden confinats (chroot) a ella. L'autenticació es fa directament contra LDAP.
Com funciona
L'usuari LDAP es connecta per SFTP amb uid i contrasenya
`libpam-ldap` verifica les credencials contra LDAP
`libnss-ldap` identifica el grup de l'usuari
SSH aplica la regla `Match Group` i confina l'usuari a la seva carpeta
Paquets
```bash
sudo apt install libpam-ldap libnss-ldap ldap-utils nscd -y
```
Estructura de carpetes
```
/sftp/
├── vendes/uploads/
├── suport/uploads/
├── administracio/uploads/
└── logistica/uploads/
```
Configuració `/etc/ssh/sshd_config`
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
Prova de connexió
```bash
sftp venda1@44.197.87.185   # entra a /sftp/vendes
sftp suport1@44.197.87.185  # entra a /sftp/suport
sftp admin1@44.197.87.185    # entra a /sftp/administracio
sftp logis1@44.197.87.185    # entra a /sftp/logistica
```
> 📸 **CAPTURA:** Connexió SFTP amb venda1 mostrant la carpeta uploads.

> <img width="380" height="168" alt="image" src="https://github.com/user-attachments/assets/c9faa7ae-8273-45be-ade5-700fa7c8cee3" />

> 📸 **CAPTURA:** Connexió SFTP amb els 4 departaments.

> <img width="370" height="315" alt="image" src="https://github.com/user-attachments/assets/b01d4477-33d0-40e0-818f-3fb878d177fa" />

---
12. Rsyslog — Centralització de logs
Rsyslog centralitza els registres de totes les màquines a `innovatetech-logs`. En lloc d'entrar a cada màquina per revisar els logs, tots es concentren en un sol lloc.
S'utilitza la IP privada `10.0.9.98` per a la comunicació interna ja que és permanent.
Configuració del servidor
`/etc/rsyslog.conf` — activar recepció UDP i TCP:
```
module(load="imudp")
input(type="imudp" port="514")
module(load="imtcp")
input(type="imtcp" port="514")
```
`/etc/rsyslog.d/remote.conf`:
```
$template RemoteLogs,"/var/log/remote/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
```
Configuració dels clients
`/etc/rsyslog.d/client.conf` a cada màquina:
```
*.* @@10.0.9.98:514
```
Verificació
```bash
sudo ls /var/log/remote/
# ip-10-0-6-122  ip-10-0-0-208  ip-10-0-7-135  ip-10-0-7-77  ip-10-0-9-98
```
> 📸 **CAPTURA:** `ls /var/log/remote/` mostrant les carpetes de totes les màquines.
>
> <img width="1050" height="144" alt="image" src="https://github.com/user-attachments/assets/db10733d-5c50-42e4-a4fc-0c1e3a373d95" />

---
13. MariaDB
MariaDB s'instal·la a `innovatetech-db` per allotjar la base de dades integral d'InnovateTech. Es va escollir MariaDB per ser més lleugera que MySQL i compatible al 100%.
Instal·lació
```bash
sudo apt install mariadb-server -y
sudo mysql_secure_installation
```
Configuració accés remot `/etc/mysql/mariadb.conf.d/50-server.cnf`
```
bind-address = 0.0.0.0
```
Usuari d'accés
```sql
CREATE USER 'admin'@'%' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
Dades de connexió
Host: `10.0.0.208` (intern) / `100.50.111.243` (extern)
Port: `3306`
Usuari: `admin` / Contrasenya: `12345`
Taules de la BD innovatetech
```
Cataleg_Videos, Clients, Comandes, Config_Qualitat,
Control_Backup, Departaments, Empleats, Mesures_Amplada_Banda,
Productes, Taula_Avisos, Trucades, Usuaris
```
> 📸 **CAPTURA:** systemctl status mariadb actiu.
> <img width="880" height="334" alt="image" src="https://github.com/user-attachments/assets/fb86346a-4c46-4cc8-a530-367de08cd806" />

> 📸 **CAPTURA:** SHOW TABLES a la BD innovatetech.
> <img width="781" height="286" alt="image" src="https://github.com/user-attachments/assets/7d2e7bb3-f6ca-4b03-bb54-6fc24fafc506" />

---
14. Ansible
Ansible automatitza la configuració de servidors des del node controlador `innovatetech-logs`. S'han creat playbooks capaços de crear instàncies EC2 des de zero i configurar-les completament.
Estructura
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
Inventari
```ini
[web]
10.0.7.135

[ldap]
10.0.6.122

[all:vars]
ansible_user=admintech
ansible_ssh_private_key_file=/home/admintech/.ssh/id_rsa
```
Credencials AWS
Les credencials AWS es gestionen amb `update-credentials.sh`. Cal actualitzar-les a cada sessió del lab:
```bash
# 1. Actualitzar ~/.aws/credentials amb les noves credencials
nano ~/.aws/credentials

# 2. Exportar-les
source ~/ansible/update-credentials.sh
```
Playbooks
Playbook	Funció
`playbook-web.yml`	Configura màquina web existent
`playbook-ldap.yml`	Configura màquina LDAP existent
`playbook-eliminar-web.yml`	Elimina NGINX (simulació fallada)
`playbook-provision-web.yml`	Crea EC2 nova + configura web complet
`playbook-provision-ldap.yml`	Crea EC2 nova + configura LDAP complet
Rol `provision`
El rol més important. Crea una instància EC2, assigna IP elàstica i configura l'usuari admintech:
Crea EC2 (Ubuntu 24.04, t2.micro)
Espera SSH disponible
Assigna IP elàstica
Crea usuari `admintech` (connectant com a `ubuntu`)
Copia clau pública a `authorized_keys`
Configura sudo sense contrasenya
Rol `web`
Desplega el servidor web complet:
NGINX + PHP 8.3 FPM
Web corporativa `index.php`
Integració LDAP per autenticació
SFTP amb chroot per departament
Rol `ldap`
Instal·la OpenLDAP des de zero amb `debconf` per evitar preguntes interactives. Crea tota l'estructura: OUs, grups, 12 usuaris de departament i 3 usuaris de gestió BD (bd1, bd2, bd3).
Execució
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
Verificació
```bash
cd ~/ansible && ansible all -m ping
```
> 📸 **CAPTURA:** `ansible all -m ping` amb totes les màquines en SUCCESS.
> <img width="568" height="259" alt="image" src="https://github.com/user-attachments/assets/88a8b353-07b8-4857-b52c-68c47e229ac6" />

> 📸 **CAPTURA:** Execució de `playbook-provision-web.yml` completada.
> <img width="1050" height="230" alt="image" src="https://github.com/user-attachments/assets/027d94fc-b1e4-48bb-866e-75f5e9aa2f6f" />



> 📸 **CAPTURA:** Execució de `playbook-provision-ldap.yml` completada.
><img width="1116" height="236" alt="image" src="https://github.com/user-attachments/assets/f679891b-b5c3-46ca-bad9-b0780c8755c8" />


---
15. Aplicació web — Gestió BD i Streaming
L'aplicació web corporativa d'InnovateTech té tres seccions principals:
Inici
Pàgina corporativa amb estadístiques i accés ràpid als serveis.
Streaming
Enllaços als serveis multimèdia del company:
Àudio: http://34.255.147.8:8080 (Icecast)
Vídeo: https://54.227.77.10 (NGINX-RTMP)
Gestió BD
Sistema de gestió de base de dades amb autenticació LDAP:
Accés restringit — només usuaris `bd1`, `bd2`, `bd3` (contrasenya: `bd1234`)
Funcionalitats:
Visualització de totes les taules de la BD
Inserció de nous registres
Eliminació de registres
Navegació entre taules
Taules accessibles:
`Empleats`, `Clients`, `Departaments`, `Productes`, `Usuaris`, `Trucades`, `Cataleg_Videos`
Autenticació:
L'usuari introdueix `uid_ldap` i contrasenya
El sistema verifica contra LDAP (`10.0.6.122`)
Comprova que el `uid` comenci per `bd`
Si és correcte, dóna accés a la gestió
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

---
16. Problemes i solucions
P1 — Permisos del fitxer .pem a Windows
Problema: SSH rebutjava la clau privada per permisos massa oberts.
Solució:
```powershell
icacls "innovatetech-key.pem" /inheritance:r
icacls "innovatetech-key.pem" /grant:r "gamer:R"
icacls "innovatetech-key.pem" /remove "Pc-\userrr"
```
P2 — IPs públiques canvien cada sessió
Problema: Les IPs públiques de les EC2 canvien a cada reinici del lab.
Solució: Assignar IPs elàstiques a les màquines principals. Usar IPs privades per a la comunicació interna.
P3 — sudo demanava contrasenya en scripts remots
Problema: Els scripts automatitzats fallaven perquè sudo demanava contrasenya interactiva.
Solució:
```bash
echo 'admintech ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/admintech
```
P4 — PasswordAuthentication bloquejada per AWS
Problema: AWS crea `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf` amb `PasswordAuthentication no` que sobreescriu el `sshd_config` principal.
Solució:
```bash
sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' \
  /etc/ssh/sshd_config.d/60-cloudimg-settings.conf
```
P5 — NGINX no arrencava després de reinstal·lació
Problema: En eliminar `/etc/nginx` completament, en reinstal·lar el paquet no recreava el directori ni el `nginx.conf`.
Solució: Crear una nova instància EC2 i desplegar-la amb Ansible, demostrant precisament la utilitat d'aquesta eina.
P6 — Grups LDAP duplicats per suport i logística
Problema: Els usuaris de `suport` i `logistica` compartien el gid `3003` i el chroot SFTP no funcionava.
Solució: Crear grups específics: `suport` (gid 3004) i `logistica` (gid 3005). Usar noms de grup en lloc de gids numèrics a `Match Group`.
P7 — Ansible instal·lat a la màquina incorrecta
Problema: Ansible es va instal·lar a `innovatetech-web` en lloc de `innovatetech-logs`.
Solució: Desinstal·lar del web i instal·lar al servidor de logs.
P8 — ssh-copy-id denegat entre EC2
Problema: No es podia copiar la clau pública d'Ansible entre màquines.
Solució: Copiar manualment el contingut de la clau pública al `authorized_keys` de cada màquina des del terminal local.
P9 — NoCredentialsError en playbooks de provisió
Problema: El mòdul `amazon.aws` no llegia les variables d'entorn automàticament.
Solució: Passar credencials via `lookup('env', ...)` i exportar amb `source update-credentials.sh`.
P10 — skipping: no hosts matched al segon play
Problema: La IP de la nova instància no estava a l'inventari.
Solució: Usar `add_host` als `post_tasks` per afegir dinàmicament la IP al grup temporal `nova_instancia`.
P11 — Permission denied al crear admintech via Ansible
Problema: Les tasques `delegate_to` intentaven connectar amb `admintech` però la nova instància només tenia `ubuntu`.
Solució: Afegir `vars: ansible_user: ubuntu` a cada tasca `delegate_to`.
P12 — index.php es descarregava en lloc d'executar-se
Problema: El Virtual Host no tenia configuració PHP-FPM, NGINX servia el PHP com a fitxer estàtic.
Solució: Instal·lar `php8.3-fpm` i afegir el bloc `location ~ \.php$` al Virtual Host.
P13 — Login web no acceptava usuaris bd
Problema: Els usuaris `bd1`, `bd2`, `bd3` estan a `ou=admin,ou=usuarios` però el search de LDAP només buscava a `ou=usuarios`.
Solució: Canviar el search base a `dc=innovatetech,dc=local` per buscar a tot el directori.
P14 — Logout no funcionava correctament
Problema: Après de fer logout, l'usuari seguia logejat.
Solució:
```php
session_unset();
session_destroy();
setcookie(session_name(), '', time()-3600, '/');
header('Location: /?section=home');
exit;
```
P15 — AddressLimitExceeded en assignar IP elàstica
Problema: Les comptes Academy tenen límit de 5 IPs elàstiques.
Solució: Alliberar les IPs de les instàncies de prova abans d'executar els playbooks de provisió.
---
Comprovacions finals
```bash
# Connexió SSH a totes les màquines
ssh -i ~/Baixades/innovatetech-key.pem admintech@100.28.104.126  # LDAP
ssh -i ~/Baixades/innovatetech-key.pem admintech@3.208.185.55    # Logs
ssh -i ~/Baixades/innovatetech-key.pem admintech@100.50.111.243  # DB
ssh -i ~/Baixades/innovatetech-key.pem admintech@35.171.63.1     # Web

# LDAP
ssh -i ~/Baixades/innovatetech-key.pem admintech@100.28.104.126 \
  "ldapsearch -x -H ldap://localhost -b 'dc=innovatetech,dc=local' | grep dn"

# Web
curl -I http://innovatetech-itb.duckdns.org

# SFTP per departament
sftp venda1@35.171.63.1
sftp suport1@35.171.63.1
sftp admin1@35.171.63.1
sftp logis1@35.171.63.1

# Logs centralitzats
ssh -i ~/Baixades/innovatetech-key.pem admintech@3.208.185.55 \
  "sudo ls /var/log/remote/"

# MariaDB
ssh -i ~/Baixades/innovatetech-key.pem admintech@100.50.111.243 \
  "sudo mysql -u root -p12345 -e 'USE innovatetech; SHOW TABLES;'"

# Ansible ping
ssh -i ~/Baixades/innovatetech-key.pem admintech@3.208.185.55 \
  "cd ~/ansible && ansible all -m ping"

# Gestió BD web
# Obrir http://innovatetech-itb.duckdns.org?section=bd
# Login: bd1 / bd1234
```
---

---

### 4.1. Servicio de Streaming de Audio <a name="41-servicio-de-streaming-de-audio"></a>

> Servidor: AWS EC2 — Ubuntu Server 22.04 LTS

El servidor de audio ofrece dos modalidades: **audio bajo demanda** (archivos MP3 servidos por Nginx) y **streaming en directo** (Icecast2 en formato OGG/Vorbis).

---

### Infraestructura AWS

Se ha desplegado una instancia EC2 `t2.micro` con Ubuntu 22.04 LTS. El entorno AWS Academy impone restricciones de red que han condicionado algunas decisiones de configuración: cambio de puerto 80 → 8080, IPs públicas dinámicas y Security Groups que se resetean al reiniciar el lab.

**Security Group `servicios-multimedia` — puertos abiertos:**

| Puerto | Protocolo | Servicio  |
|--------|-----------|-----------|
| 22     | TCP       | SSH       |
| 80     | TCP       | HTTP      |
| 443    | TCP       | HTTPS     |
| 1935   | TCP       | RTMP      |
| 8000   | TCP       | Icecast2  |
| 8080   | TCP       | Nginx     |

> 📷 **CAPTURA 1:** Consola AWS → EC2 → Security Groups → `servicios-multimedia` → pestaña "Inbound rules". Debe verse la tabla completa con los 6 puertos y origen `0.0.0.0/0`.

---

### Servidor web — Nginx

Nginx actúa como servidor web en el puerto 8080 y sirve los contenidos multimedia. Se instala también el módulo RTMP para soporte de streaming futuro.

**Instalación:**

```bash
sudo apt update
sudo apt install -y nginx libnginx-mod-rtmp
```

**Verificación del servicio:**

```bash
nginx -v
sudo systemctl status nginx
sudo ss -tlnp | grep 8080
```

> 📷 **CAPTURA 2:** Terminal mostrando `sudo systemctl status nginx` con estado `active (running)` en verde y la línea `nginx -v` con la versión instalada.

**Prueba de respuesta HTTP:**

```bash
curl -I http://localhost:8080
```

> 📷 **CAPTURA 3:** Terminal con la salida de `curl -I http://localhost:8080` mostrando `HTTP/1.1 200 OK`.

**Configuración `/etc/nginx/nginx.conf`** con bloques `location /videos` y `location /audio` con los `Content-Type` adecuados y cabeceras CORS:

```bash
cat /etc/nginx/nginx.conf
```

> 📷 **CAPTURA 4:** Terminal mostrando el contenido de `nginx.conf` con los bloques `location /audio` y `location /videos` visibles.

---

### Audio bajo demanda — Nginx

Los archivos MP3 se almacenan en el servidor y se sirven por HTTP desde `/var/www/html/audio/`.

**Creación de la carpeta y descarga de audios:**

```bash
sudo mkdir -p /var/www/html/audio

sudo wget -O /var/www/html/audio/audio1.mp3 "https://download.samplelib.com/mp3/sample-3s.mp3"
sudo wget -O /var/www/html/audio/audio2.mp3 "https://download.samplelib.com/mp3/sample-6s.mp3"
sudo wget -O /var/www/html/audio/audio3.mp3 "https://download.samplelib.com/mp3/sample-9s.mp3"

# Generar audio sintético con ffmpeg (tono 440Hz, 30s)
sudo ffmpeg -f lavfi -i sine=frequency=440:duration=30 \
  -c:a libmp3lame -b:a 128k /var/www/html/audio/audio4.mp3

sudo chown -R www-data:www-data /var/www/html/audio
```

**Verificación de los archivos:**

```bash
ls -lh /var/www/html/audio/
```

> 📷 **CAPTURA 5:** Terminal con `ls -lh /var/www/html/audio/` mostrando los 4 archivos MP3 con propietario `www-data` y sus tamaños.

**Prueba de acceso HTTP:**

```bash
curl -I http://localhost:8080/audio/audio1.mp3
```

> 📷 **CAPTURA 6:** Terminal con la respuesta `200 OK` y cabecera `Content-Type: audio/mpeg`.

---

### Streaming en directo — Icecast2

Icecast2 es el servidor de streaming de audio en directo. Emite en formato OGG/Vorbis en el puerto 8000. El cliente emisor es `ffmpeg`, que genera un tono de prueba de 440Hz y lo envía al mount `/stream.ogg`.

**Instalación:**

```bash
sudo apt install -y icecast2
```

**Configuración en `/etc/icecast2/icecast.xml`:**

| Parámetro        | Valor          |
|------------------|----------------|
| `source-password`| `12345`        |
| Puerto           | `8000`         |
| Mount point      | `/stream.ogg`  |

**Verificación del servicio:**

```bash
sudo systemctl status icecast2
sudo ss -tlnp | grep 8000
```

> 📷 **CAPTURA 7:** Terminal mostrando `sudo systemctl status icecast2` con estado `active (running)` y el puerto 8000 escuchando.

**Verificación del stream activo** (Icecast no soporta HEAD para streams):

```bash
curl -v http://localhost:8000/stream.ogg --output /dev/null 2>&1 | head -20
```

> 📷 **CAPTURA 8:** Terminal con la respuesta `200 OK` y `Content-Type: application/ogg` — stream funcional.

**Estado del servidor desde el navegador** (`http://IP:8000/status.xsl`):

> 📷 **CAPTURA 9:** Navegador mostrando la página de estado de Icecast2 en `http://IP:8000/status.xsl` con el Mount Point `/stream.ogg` activo y el contador de Listeners.

---

### Interfaz web

La interfaz web es accesible en `http://IP:8080`. La pestaña **Audio** incluye un banner de radio en directo (Icecast2), la lista de pistas MP3 bajo demanda y el botón "Escoltar Radio" que abre Icecast en una nueva pestaña.

> 📷 **CAPTURA 10:** Navegador mostrando la pestaña Audio de la interfaz web en `http://IP:8080` con el banner de radio, el botón "Escoltar Radio" y la lista de pistas MP3.

> 📷 **CAPTURA 11:** Navegador reproduciendo una pista MP3 — el reproductor de audio en funcionamiento.

---

### Protocolos utilizados

| Protocolo | Uso |
|-----------|-----|
| **HTTP** | Nginx sirve los archivos MP3 en el puerto 8080 (bajo demanda) |
| **Icecast / HTTP Streaming** | Icecast2 sirve el stream en el puerto 8000 vía HTTP |
| **RTMP** | Puerto 1935, módulo Nginx instalado (preparado para streaming en directo futuro) |
| **OGG/Vorbis** | Formato y códec del stream en directo de Icecast2 |
| **MP3** | Formato de los archivos de audio bajo demanda |
| **TCP** | Transporte de todas las conexiones HTTP y streaming |

[⬆ Volver al índice](#-tabla-de-contenidos)


### 4.2. Servicio de Streaming de Vídeo <a name="42-servicio-de-streaming-de-video"></a>
> Servidor: AWS EC2 — Ubuntu Server 22.04 LTS

El servicio de vídeo funciona en modo **VOD (Video on Demand)**. Los archivos MP4 con códec H.264 se almacenan en el servidor y se reproducen bajo demanda desde el navegador con **VideoJS**.

> ℹ️ El profesor confirmó que la práctica requiere vídeo pregrabado servido por HTTP, no streaming en directo.

---

### Creación de la carpeta y permisos

```bash
sudo mkdir -p /var/www/html/videos
sudo chown -R www-data:www-data /var/www/html/videos
sudo chmod -R 755 /var/www/html/videos
```

---

### Descarga de los vídeos de prueba

El entorno AWS Academy restringe muchos dominios externos. Se descargan vídeos de muestra desde `samplelib.com` y se genera un cuarto vídeo sintético con `ffmpeg` (barras de color + tono de 440Hz):

```bash
sudo wget -O /var/www/html/videos/video1.mp4 "https://download.samplelib.com/mp4/sample-5s.mp4"
sudo wget -O /var/www/html/videos/video2.mp4 "https://download.samplelib.com/mp4/sample-10s.mp4"
sudo wget -O /var/www/html/videos/video3.mp4 "https://download.samplelib.com/mp4/sample-15s.mp4"

# Generar vídeo sintético con ffmpeg (barras de color + tono 440Hz, 30s)
sudo apt install -y ffmpeg
sudo ffmpeg -f lavfi -i testsrc=duration=30:size=1280x720:rate=30 \
  -f lavfi -i sine=frequency=440:duration=30 -c:v libx264 -c:a aac \
  /var/www/html/videos/video4.mp4

sudo chown -R www-data:www-data /var/www/html/videos
```

**Verificación de los archivos:**

```bash
ls -lh /var/www/html/videos/
```

> 📷 **CAPTURA 1:** Terminal con `ls -lh /var/www/html/videos/` mostrando los 4 archivos MP4 con propietario `www-data` y sus tamaños.

---

### Prueba de acceso HTTP al vídeo

```bash
curl -I http://localhost:8080/videos/video1.mp4
```

> 📷 **CAPTURA 2:** Terminal con la respuesta `200 OK` y cabecera `Content-Type: video/mp4` — Nginx sirve el vídeo correctamente.

---

### Reproductor web — VideoJS

La interfaz web es accesible en `http://IP:8080`. Tiene dos pestañas (**Vídeo** y **Audio**), reproductor central y lista lateral de contenidos.

> 📷 **CAPTURA 3:** Navegador mostrando la pestaña Vídeo de la interfaz web en `http://IP:8080` con el reproductor VideoJS y la lista de clips en la barra lateral.

> 📷 **CAPTURA 4:** Navegador con un vídeo reproduciéndose correctamente dentro del reproductor VideoJS.

---

### Estructura de ficheros y permisos

Todo el contenido servido por Nginx pertenece a `www-data`:

```bash
ls -lhR /var/www/html/
```

> 📷 **CAPTURA 5:** Terminal con `ls -lhR /var/www/html/` mostrando la estructura completa de carpetas `/videos` y `/audio` con propietario `www-data` en todos los archivos.

---

### Todos los puertos en escucha

```bash
sudo ss -tlnp
```

> 📷 **CAPTURA 6:** Terminal con `sudo ss -tlnp` mostrando los puertos activos: 22 (SSH), 8000 (Icecast2) y 8080 (Nginx).

---

### Firewall y seguridad de red

El firewall interno de Ubuntu (UFW) está desactivado. La seguridad se gestiona vía AWS Security Groups a nivel de VPC.

```bash
sudo ufw status
```

> 📷 **CAPTURA 7:** Terminal con `sudo ufw status` mostrando `Status: inactive`.

---

### Protocolos utilizados

| Protocolo | Uso |
|-----------|-----|
| **HTTP**  | Nginx sirve los archivos MP4 en el puerto 8080 (VOD bajo demanda) |
| **HLS**   | Módulo RTMP de Nginx configurado para generar fragmentos HLS (preparado; finalmente se optó por VOD estático) |
| **H.264** | Códec de vídeo de los archivos MP4 |
| **TCP**   | Transporte de todas las conexiones HTTP |

---

### Conclusiones

- ✅ **Nginx** operativo en el puerto 8080 — vídeo MP4 (H.264) y audio MP3 bajo demanda vía HTTP
- ✅ **Icecast2** operativo en el puerto 8000 — streaming en directo OGG/Vorbis funcional en el navegador
- ✅ **Interfaz web unificada** en `http://34.225.147.8:8080` con pestañas Vídeo y Audio
- ✅ **Formatos:** MP4/H.264 para vídeo, MP3 y OGG/Vorbis para audio
- ✅ **Seguridad** vía AWS Security Groups — puertos 22, 8000 y 8080 abiertos desde `0.0.0.0/0`

[⬆ Volver al índice](#-tabla-de-contenidos)


### 4.3. Videoconferencia (Jitsi Meet) <a name="43-videoconferencia-jitsi-meet"></a>
> Instalación nativa sobre Ubuntu 22.04 — EC2 sin Docker

Jitsi Meet es una plataforma de videoconferencia de código abierto que permite crear salas de reuniones virtuales sin necesidad de cuentas de usuario. Se instala de forma nativa (sin Docker) sobre una instancia EC2 de AWS Academy con Ubuntu 22.04, usando una IP elástica pública para garantizar la accesibilidad desde el exterior.

| Parámetro | Valor |
|-----------|-------|
| **IP elástica (pública)** | `54.227.77.10` |
| **IP privada (VPC)** | `172.31.36.193` |
| **Sistema operativo** | Ubuntu 22.04 LTS |
| **Hostname configurado** | `jitsi-meet` |
| **Acceso a la instancia** | EC2 Instance Connect (sin `.pem`) |
| **Método de instalación** | Nativo — sin Docker ni Ngrok |

> ⚠️ **Lección aprendida:** la versión JVB 2.3-291 **NO** lee la configuración XMPP del fichero `jvb.conf` (HOCON). La lee exclusivamente de `sip-communicator.properties` mediante el `ConfigurationService`, con claves en **MAYÚSCULAS**.

---

### Paso 1 — Preparar el sistema

#### 1.1 Actualizar paquetes e instalar dependencias

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apt-transport-https curl gnupg2 wget nginx software-properties-common
```

> 📷 **CAPTURA 1:** Terminal con la salida de `apt upgrade` finalizado sin errores.

#### 1.2 Configurar el hostname

> ⚠️ Es imprescindible configurar el hostname **antes** de instalar cualquier componente de Jitsi. Los paquetes lo usan para generar los certificados TLS y configurar los dominios XMPP. Si se configura después, hay que regenerar todos los certificados.

```bash
sudo hostnamectl set-hostname jitsi-meet
echo "127.0.0.1 jitsi-meet" | sudo tee -a /etc/hosts
echo "54.227.77.10 jitsi-meet" | sudo tee -a /etc/hosts
hostname   # verificación — debe mostrar: jitsi-meet
```

> 📷 **CAPTURA 2:** Terminal mostrando el resultado de `hostname` con el valor `jitsi-meet` y el contenido de `/etc/hosts` con las dos entradas añadidas.

#### 1.3 Instalar Java 11

Jicofo y Jitsi Videobridge (JVB) son aplicaciones Java. Se requiere la versión 11 como mínimo.

```bash
sudo apt install -y openjdk-11-jdk
java -version
```

> 📷 **CAPTURA 3:** Terminal con `java -version` mostrando `OpenJDK 11`.

---

### Paso 2 — Añadir el repositorio oficial de Jitsi

Jitsi Meet no está en los repositorios oficiales de Ubuntu. Hay que añadir el repositorio propio de Jitsi con su clave GPG.

```bash
curl https://download.jitsi.org/jitsi-key.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/jitsi-key.gpg

echo "deb [signed-by=/usr/share/keyrings/jitsi-key.gpg] https://download.jitsi.org stable/" | \
  sudo tee /etc/apt/sources.list.d/jitsi-stable.list

sudo apt update
```

> 📷 **CAPTURA 4:** Terminal con la clave GPG de Jitsi añadida correctamente y `apt update` ejecutado sin errores.

---

### Paso 3 — Instalar Jitsi Meet

Se instalan los tres componentes principales en un solo comando:

| Componente | Descripción |
|------------|-------------|
| `jicofo` | Coordinador de conferencias (Jitsi Conference Focus) |
| `jitsi-videobridge2` | Servidor de media que gestiona los flujos RTP |
| `jitsi-meet` | Frontend web (HTML/JS) servido por Nginx |

```bash
sudo apt install -y jicofo jitsi-videobridge2 jitsi-meet
```

Durante la instalación aparecen dos diálogos interactivos:
1. **Hostname:** escribir `jitsi-meet`
2. **Certificado SSL:** seleccionar `Generate a new self-signed certificate`

> 📷 **CAPTURA 5:** Terminal mostrando la instalación completada de los tres componentes sin errores.

---

### Paso 4 — Configurar Prosody (servidor XMPP)

Prosody es el servidor XMPP que gestiona toda la señalización entre los clientes web y los componentes de Jitsi.

#### 4.1 Generar certificados TLS

```bash
sudo prosodyctl cert generate jitsi-meet
sudo prosodyctl cert generate auth.jitsi-meet
sudo ls /etc/prosody/certs/
```

Deben existir los cuatro ficheros: `jitsi-meet.key`, `jitsi-meet.crt`, `auth.jitsi-meet.key`, `auth.jitsi-meet.crt`.

> 📷 **CAPTURA 6:** Terminal con `ls /etc/prosody/certs/` mostrando los 4 ficheros de certificados generados.

#### 4.2 Crear usuarios XMPP

JVB y Jicofo se autentican contra Prosody como usuarios internos:

```bash
JVB_PASS=$(openssl rand -hex 16)
FOCUS_PASS=$(openssl rand -hex 16)
echo "JVB_PASS=$JVB_PASS"
echo "FOCUS_PASS=$FOCUS_PASS"

sudo prosodyctl register jvb auth.jitsi-meet $JVB_PASS
sudo prosodyctl register focus auth.jitsi-meet $FOCUS_PASS
```

> ⚠️ **Guardar los valores de `JVB_PASS` y `FOCUS_PASS` inmediatamente.** Se necesitan en los pasos 5 y 6.

#### 4.3 Reiniciar Prosody

```bash
sudo systemctl restart prosody
sudo systemctl status prosody
```

> 📷 **CAPTURA 7:** Terminal con `sudo systemctl status prosody` mostrando estado `active (running)`.

---

### Paso 5 — Configurar el mapeado NAT para AWS (JVB)

> ⚠️ **Parte más crítica de la instalación.** JVB se ejecuta en la IP privada de la VPC (`172.31.36.193`) pero los clientes externos deben conectarse a la IP elástica pública (`54.227.77.10`). Sin el mapeado correcto, ICE no puede negociar los candidatos de media y las videoconferencias no se establecen.

#### 5.1 `sip-communicator.properties` — conexión XMPP del JVB

> ⚠️ La versión JVB 2.3-291 lee la configuración XMPP **ÚNICAMENTE** de este fichero, no de `jvb.conf`. Las claves deben estar en **MAYÚSCULAS** exactas.

```bash
sudo nano /etc/jitsi/videobridge/sip-communicator.properties
```

```properties
org.jitsi.videobridge.xmpp.user.shard.HOSTNAME=localhost
org.jitsi.videobridge.xmpp.user.shard.DOMAIN=auth.jitsi-meet
org.jitsi.videobridge.xmpp.user.shard.USERNAME=jvb
org.jitsi.videobridge.xmpp.user.shard.PASSWORD=<JVB_PASS del paso 4.2>
org.jitsi.videobridge.xmpp.user.shard.MUC_JIDS=JvbBrewery@internal.auth.jitsi-meet
org.jitsi.videobridge.xmpp.user.shard.MUC_NICKNAME=<cat /proc/sys/kernel/random/uuid>
org.jitsi.videobridge.xmpp.user.shard.DISABLE_CERTIFICATE_VERIFICATION=true
org.ice4j.ice.harvest.DISABLE_AWS_HARVESTER=false
org.ice4j.ice.harvest.STUN_MAPPING_HARVESTER_ADDRESSES=meet-jit-si-turnrelay.jitsi.net:443
```

> 📷 **CAPTURA 8:** Terminal con `cat /etc/jitsi/videobridge/sip-communicator.properties` mostrando el contenido completo con las credenciales del JVB.

#### 5.2 `jvb.conf` — WebSockets y mapeado de IPs

```bash
sudo nano /etc/jitsi/videobridge/jvb.conf
```

```hocon
videobridge {
    http-servers { public { port = 9090 } }
    websockets {
        enabled = true
        domain = "jitsi-meet:443"
        tls = true
    }
}

ice4j {
    harvest {
        mapping {
            aws { enabled = true }
            stun { addresses = ["meet-jit-si-turnrelay.jitsi.net:443"] }
            static-mappings = [{
                local-address = "172.31.36.193"
                public-address = "54.227.77.10"
            }]
        }
    }
}
```

> 📷 **CAPTURA 9:** Terminal con `cat /etc/jitsi/videobridge/jvb.conf` mostrando el contenido completo con el mapeado IP privada → pública.

---

### Paso 6 — Configurar Jicofo

```bash
sudo nano /etc/jitsi/jicofo/jicofo.conf
```

```hocon
jicofo {
    xmpp {
        client {
            server = "localhost"
            domain = "auth.jitsi-meet"
            username = "focus"
            password = "<FOCUS_PASS del paso 4.2>"
            resource = "focus"
            disable-certificate-verification = true
        }
        trusted-domains = [ "recorder.jitsi-meet" ]
    }
    bridge {
        brewery-jid = "JvbBrewery@internal.auth.jitsi-meet"
        selection-strategy = SingleBridgeSelectionStrategy
    }
    conference { enable-auto-owner = true }
}
```

**Security Group de AWS — puertos necesarios para Jitsi:**

| Puerto | Protocolo | Uso |
|--------|-----------|-----|
| 80     | TCP       | Redirección HTTP → HTTPS |
| 443    | TCP       | Frontend web y BOSH |
| 4443   | TCP       | JVB fallback TCP |
| 10000  | UDP       | **Media RTP/RTCP (JVB)** ← el más importante |

> ⚠️ **El puerto 10000 UDP es crítico:** es el que usa JVB para enviar y recibir los flujos de media. Sin este puerto abierto las videoconferencias no tendrán audio ni vídeo.

> 📷 **CAPTURA 10:** Consola AWS → EC2 → Security Groups → pestaña "Inbound rules" mostrando los puertos 80, 443, 4443 TCP y 10000 UDP abiertos.

---

### Paso 7 — Configurar la IP pública en el frontend

Se usa BOSH (HTTP long-polling sobre HTTPS) en lugar de WebSocket porque el certificado es autofirmado y los navegadores modernos bloquean las conexiones WSS a certificados no válidos.

#### 7.1 `jitsi-meet-config.js`

```bash
sudo nano /etc/jitsi/meet/jitsi-meet-config.js
```

Modificaciones:
```javascript
bosh: 'https://54.227.77.10/http-bind',
//websocket: 'wss://jitsi-meet/' + subdir + 'xmpp-websocket',  // comentado
```

#### 7.2 Fix crítico en Nginx: header Host

**Problema:** el bloque `/http-bind` de Nginx usa `$http_host`, pero cuando el navegador accede por IP el header que envía es la IP pública, no el hostname interno. Prosody rechaza la conexión.

```bash
sudo nano /etc/nginx/sites-available/jitsi-meet.conf
```

```nginx
# Dentro del bloque 'location = /http-bind', cambiar:
proxy_set_header Host $http_host;   # ← incorrecto

# Por:
proxy_set_header Host jitsi-meet;   # ← correcto
```

#### 7.3 Desactivar el Service Worker

```bash
sudo bash -c 'echo "" > /usr/share/jitsi-meet/pwa-worker.js'
```

#### 7.4 Verificar y recargar Nginx

```bash
sudo nginx -t
sudo systemctl reload nginx
```

> 📷 **CAPTURA 11:** Terminal con `sudo nginx -t` mostrando `syntax is ok` y `test is successful`.

---

### Paso 8 — Arrancar los servicios

> ⚠️ El orden de arranque es importante: **Prosody debe estar operativo** antes de que JVB y Jicofo intenten conectarse.

```bash
sudo systemctl restart prosody
sleep 3
sudo systemctl restart jitsi-videobridge2
sleep 5
sudo systemctl restart jicofo

# Verificación de los tres servicios
sudo systemctl status prosody jitsi-videobridge2 jicofo --no-pager | grep -E "Active|●"
```

> 📷 **CAPTURA 12:** Terminal con el resultado del comando de verificación mostrando los tres servicios con estado `active (running)`.

**Verificar los logs de JVB:**

```bash
sudo tail -30 /var/log/jitsi/jvb.log
```

**Verificar los logs de Jicofo:**

```bash
sudo tail -30 /var/log/jitsi/jicofo.log
```

Líneas que confirman el funcionamiento correcto:
- `Joined the room` → Jicofo unido al MUC JvbBrewery
- `Added new videobridge: Bridge[jid=jvbbrewery@...]` → JVB detectado y disponible

> 📷 **CAPTURA 13:** Terminal con `sudo tail -30 /var/log/jitsi/jicofo.log` mostrando la línea `Added new videobridge` con la versión 2.3.291.

---

### Paso 9 — Prueba de funcionamiento

Con todos los servicios activos, se accede a la interfaz web:

```
https://54.227.77.10
```

> ⚠️ El navegador mostrará un aviso de certificado autofirmado. Hay que aceptarlo: **Avanzado → Continuar de todas formas**.

> 📷 **CAPTURA 14:** Navegador mostrando la página de inicio de Jitsi Meet en `https://54.227.77.10` con el campo para crear una sala.

> 📷 **CAPTURA 15:** Navegador dentro de una sala de videoconferencia activa, con cámara y/o audio funcionando (puede ser con dos pestañas o dos dispositivos).

---

### Resumen de ficheros modificados

| Fichero | Cambio aplicado |
|---------|-----------------|
| `/etc/jitsi/videobridge/sip-communicator.properties` | Configuración XMPP del JVB (claves MAYÚSCULAS) |
| `/etc/jitsi/videobridge/jvb.conf` | WebSockets + mapeado IP privada/pública |
| `/etc/jitsi/jicofo/jicofo.conf` | Credenciales XMPP + brewery-jid + disable-cert |
| `/etc/jitsi/meet/jitsi-meet-config.js` | `bosh` con IP pública, websocket comentado |
| `/usr/share/jitsi-meet/pwa-worker.js` | Vaciado — evita errores Service Worker |
| `/etc/nginx/sites-available/jitsi-meet.conf` | `proxy_set_header Host jitsi-meet` (bloque `/http-bind`) |

---

### Protocolos utilizados

**Señalización (control):**

| Protocolo | Uso |
|-----------|-----|
| **XMPP** | Protocolo base de mensajería. Prosody coordina todos los componentes (JVB, Jicofo, clientes) |
| **BOSH** | Tunneling de XMPP sobre HTTP/HTTPS. El navegador se conecta a Prosody a través de `/http-bind` |

**Media (audio y vídeo):**

| Protocolo | Uso |
|-----------|-----|
| **WebRTC** | Estándar del navegador para comunicación en tiempo real |
| **ICE** | Negocia la ruta óptima entre peers |
| **STUN** | Permite descubrir la IP pública del cliente (`meet-jit-si-turnrelay.jitsi.net:443`) |
| **DTLS** | Cifrado del canal de media |
| **SRTP** | Transmisión segura de audio/vídeo |
| **RTP/RTCP** | Transporte real de los paquetes de media sobre UDP puerto 10000 |

**Transporte web:**

| Protocolo | Uso |
|-----------|-----|
| **HTTPS (TLS 1.2/1.3)** | Puerto 443 — todo el frontend y BOSH |
| **HTTP** | Puerto 80 — redirección a HTTPS |
| **Colibri** | Protocolo propio de Jitsi sobre WebSocket entre el navegador y JVB (puerto 9090 interno, expuesto por Nginx en `/colibri-ws/`) |

[⬆ Volver al índice](#-tabla-de-contenidos)

---

## 7. Comprobaciones de Rendimiento y Seguridad

> Pruebas de rendimiento de red — AWS EC2 · CFGS ASIX · ITB · Curs 25/26

El objetivo de este apartado es verificar que la infraestructura desplegada en AWS es capaz de soportar simultáneamente los servicios de audio, vídeo y videoconferencia sin degradación del servicio.

Las pruebas se han realizado entre las dos instancias EC2 desplegadas en la misma VPC (`us-east-1`), usando `iperf3` para medir el ancho de banda real de la red interna.

### Infraestructura de pruebas

| Instancia | Servicios | IP privada | IP pública |
|-----------|-----------|------------|------------|
| **Instancia 1** | Servidor Audio/Vídeo (Nginx + Icecast2) | `172.31.17.184` | `34.225.147.8` |
| **Instancia 2** | Servidor Videoconferencia (Jitsi Meet) | `172.31.36.193` | `54.227.77.10` |

> Las pruebas se ejecutan usando las **IPs privadas** para que el tráfico pase por la red interna de la VPC.

### Herramienta — iperf3

```bash
sudo apt install -y iperf3
iperf3 -s -D   # iniciar servidor en Instancia 1
```

---

### Prueba 1 — Instancia 2 (Jitsi) → Instancia 1 (Audio/Vídeo)

**Download:**

```bash
iperf3 -c 172.31.17.184 -p 5201 -t 10
```

> 📷 **CAPTURA 16:** Terminal con el resultado de `iperf3` mostrando **1.02 Gbits/s de download** sostenidos durante 10 segundos, con las retransmisiones (Retr: 24) y la Congestion Window estable.

**Resultado: 1.02 Gbits/s de download — retransmisiones mínimas (Retr: 24), normales en conexiones de alta velocidad.**

**Upload:**

```bash
iperf3 -c 172.31.17.184 -p 5201 -t 10 -R
```

> 📷 **CAPTURA 17:** Terminal con el resultado de `iperf3 -R` mostrando **1.02 Gbits/s de upload — 0 retransmisiones**.

**Resultado: 1.02 Gbits/s de upload — 0 retransmisiones. La simetría entre download y upload confirma que la red interna AWS es completamente simétrica.**

**Latencia:**

```bash
iperf3 -c 172.31.17.184 -p 5201 -t 1
```

> 📷 **CAPTURA 18:** Terminal con el resultado de la prueba de 1 segundo mostrando 121 MBytes transferidos.

**Resultado: 121 MBytes en 1 segundo — latencia estimada <1ms.**

> ℹ️ El ping ICMP da 100% packet loss porque AWS Academy bloquea ICMP por defecto en el Security Group. La latencia real se estima <1ms por estar en la misma zona de disponibilidad.

---

### Prueba 2 — Instancia 1 (Audio/Vídeo) → Instancia 2 (Jitsi)

Segunda prueba en dirección inversa para verificar la simetría de la red.

**Download:**

```bash
iperf3 -c 172.31.36.193 -p 5201 -t 10
```

> 📷 **CAPTURA 19:** Terminal con resultado **1.02 Gbits/s — 0 retransmisiones**. Conexión perfectamente estable.

**Upload:**

```bash
iperf3 -c 172.31.36.193 -p 5201 -t 10 -R
```

> 📷 **CAPTURA 20:** Terminal con resultado **1.02 Gbits/s — conexión simétrica confirmada en ambas direcciones**.

**Latencia:**

```bash
iperf3 -c 172.31.36.193 -p 5201 -t 1
```

> 📷 **CAPTURA 21:** Terminal con 121 MBytes en 1 segundo — **latencia <1ms confirmada en ambas direcciones**.

---

### Resumen de resultados

| Prueba | Dirección | Download | Upload | Latencia | Retr |
|--------|-----------|----------|--------|----------|------|
| **Prueba 1** | Jitsi → Audio/Vídeo | 1.02 Gbits/s | 1.02 Gbits/s | <1ms | 24 / 0 |
| **Prueba 2** | Audio/Vídeo → Jitsi | 1.02 Gbits/s | 1.02 Gbits/s | <1ms | 0 / 25 |

---

### Análisis y relación con los servicios

**Streaming de audio — Icecast2:**
```
Listeners máximos = Ancho de banda disponible ÷ Bitrate por listener
1.020 Mbits/s ÷ 0.128 Mbits/s = ~7.968 listeners simultáneos
```

**Streaming de vídeo — Nginx VOD:**
```
Caso conservador (5 Mbits/s): 1.020 ÷ 5 = 204 conexiones simultáneas
Caso optimista   (2 Mbits/s): 1.020 ÷ 2 = 510 conexiones simultáneas
```

**Videoconferencia — Jitsi Meet (WebRTC):**
```
Caso conservador (4 Mbits/s): 1.020 ÷ 4 = 255 participantes simultáneos
Caso optimista   (1 Mbits/s): 1.020 ÷ 1 = 1.020 participantes simultáneos
```

---

### Clasificación del sistema

> ✅ **SISTEMA CLASIFICADO COMO: ACEPTABLE**

- Ancho de banda interno de **1.02 Gbits/s** — muy por encima de los requisitos de todos los servicios
- Conexión **simétrica**: download y upload idénticos en ambas direcciones
- **Latencia interna <1ms** entre instancias de la misma VPC
- **Retransmisiones mínimas** — calidad de conexión excelente
- Capacidad para soportar **miles de usuarios simultáneos** en todos los servicios

---

### Automatización de medidas de ancho de banda

#### Qué hace

Un script Bash (`test_amplada_bo.sh`) que se ejecuta automáticamente, mide el ancho de banda de la máquina multimedia e inserta los resultados directamente en la base de datos de InnovateTech.

#### Cómo funciona

- Ejecuta `speedtest-cli --simple` y extrae ping, bajada y subida
- Evalúa si el resultado es `acceptable` (bajada >50 Mbit/s) o `no_acceptable`
- Inserta automáticamente en la tabla `Mesures_Amplada_Banda` de la BD (`100.50.111.243`)
- El umbral de 50 Mbit/s cubre ampliamente todos los servicios: audio (~0,5 Mbit/s), vídeo HD (~5 Mbit/s) y Jitsi (~2 Mbit/s)

#### Ubicación del script

```
/home/ubuntu/scripts/test_amplada_bo.sh
```

#### Conexión a la BD

| Parámetro | Valor |
|-----------|-------|
| Host | `100.50.111.243` |
| Base de datos | `innovatetech` |
| Tabla | `Mesures_Amplada_Banda` |

#### Planificación cron (3 franjas)

```cron
# De 22:00 a 06:00 — cada hora (poco tráfico nocturno)
0 22-23,0-6 * * * /home/ubuntu/scripts/test_amplada_bo.sh >> /var/log/speedtest.log 2>&1

# De 06:00 a 14:00 — cada 15 minutos (franja laboral alta)
*/15 6-13 * * * /home/ubuntu/scripts/test_amplada_bo.sh >> /var/log/speedtest.log 2>&1

# De 14:00 a 22:00 — cada 30 minutos (franja laboral media)
*/30 14-21 * * * /home/ubuntu/scripts/test_amplada_bo.sh >> /var/log/speedtest.log 2>&1
```

| Franja | Horas | Intervalo | Motivo |
|--------|-------|-----------|--------|
| Nocturna | 22:00–06:00 | Cada hora | Tráfico mínimo |
| Mañana/mediodía | 06:00–14:00 | Cada 15 min | Máxima actividad laboral |
| Tarde | 14:00–22:00 | Cada 30 min | Actividad moderada |

#### Verificación

**Ejecución manual del script:**

```bash
/home/ubuntu/scripts/test_amplada_bo.sh
# Salida esperada: Insert fet: DOWN=... UP=... PING=... RESULTAT=acceptable
```

> 📷 **CAPTURA 22:** Terminal con la ejecución manual del script mostrando la salida `Insert fet: DOWN=... UP=... PING=... RESULTAT=acceptable`.

**Crontab configurado:**

```bash
crontab -l
```

> 📷 **CAPTURA 23:** Terminal con `crontab -l` mostrando las 3 líneas de planificación configuradas.

**Verificación en la BD:**

```sql
SELECT * FROM Mesures_Amplada_Banda ORDER BY id DESC LIMIT 3;
```

> 📷 **CAPTURA 24:** Terminal con el resultado de la consulta SQL mostrando las últimas 3 filas insertadas automáticamente por el script con los valores de DOWN, UP, PING y RESULTAT.

**Log del script:**

```bash
cat /var/log/speedtest.log
```

> 📷 **CAPTURA 25:** Terminal con `cat /var/log/speedtest.log` mostrando varias entradas del historial de medidas.

---

### Propuestas de mejora

| Mejora | Prioridad | Descripción |
|--------|-----------|-------------|
| **CDN (AWS CloudFront)** | Alta | Reducir latencia para clientes finales lejanos. La red interna es <1ms pero desde Europa puede ser >100ms |
| **Elastic IP** | Alta | Evitar que la IP pública cambie en cada reinicio de la instancia en el Learner Lab |
| **Load Balancer** | Media | Distribuir carga entre múltiples instancias si el número de usuarios crece significativamente |
| **Monitorización (AWS CloudWatch)** | Media | Supervisar el ancho de banda en producción y detectar saturación antes de que afecte al servicio |

[⬆ Volver al índice](#-tabla-de-contenidos)


## 6. Disseny e Implementació de la Base de Dades

### 6.1. Disseny Conceptual (E/R)

El diagrama Entitat-Relació representa les 14 entitats de la base de dades d'InnovateTech, els seus atributs principals i les relacions entre elles amb la cardinalitat corresponent.

> 📸 **CAPTURA:** Diagrama E/R exportat de dbdiagram.io mostrant totes les entitats i relacions.

---

### 6.2. Model Relacional

A partir del diagrama E/R s'ha obtingut l'esquema relacional complet. Les claus primàries s'indiquen amb **PK** i les foranes amb **FK**.

```
DEPARTAMENTS (codi PK, nom, telefon)

EMPLEATS (dni PK, nom, cognoms, adreca, telefon,
          codi_dept FK → DEPARTAMENTS.codi)

CLIENTS (id PK, nom, email, telefon, empresa)

CONFIG_QUALITAT (id PK, nivell, resolucio_video, bitrate_audio, amplada_banda_min)

USUARIS (id PK, uid_ldap, nom_complet, email, extensio, estat, tipus,
         dni_empleat FK → EMPLEATS.dni,
         id_client FK → CLIENTS.id,
         id_config FK → CONFIG_QUALITAT.id,
         url_videotrucada)

PRODUCTES (id PK, nom, descripcio, preu, tipus)

COMANDES (id PK, data, estat, quantitat,
          id_client FK → CLIENTS.id,
          id_producte FK → PRODUCTES.id)

CISTELL (id PK, quantitat, data_afegit,
         id_client FK → CLIENTS.id,
         id_producte FK → PRODUCTES.id)

CATALEG_VIDEOS (id PK, titol, descripcio, categoria, durada, data_publicacio, url_streaming)

TRUCADES (id PK, inici, fi, durada, puntuacio, comentari,
          id_origen FK → USUARIS.id,
          id_desti FK → USUARIS.id,
          id_config FK → CONFIG_QUALITAT.id)

MESURES_AMPLADA_BANDA (id PK, equip_mesurat, data_hora, baixada, pujada, latencia, resultat, notes,
                       id_operari FK → USUARIS.id)

CONFIG_SERVIDOR (id PK, parametre, valor, descripcio)

TAULA_AVISOS (id PK, usuari_db, taula_afectada, operacio, data_hora, detalls)

CONTROL_BACKUP (id PK, data_hora, taules_incloses, resultat)
```

> 📸 **CAPTURA:** Resultat de `SHOW TABLES` a MariaDB mostrant les 14 taules creades.

---

### 6.3. Instal·lació i Securització de MariaDB

S'ha escollit **MariaDB 10.11** com a SGBD per la seva compatibilitat amb MySQL, lleugeresa (important en una instància t2.micro amb 1 GB de RAM) i per ser de codi obert sense restriccions de llicència.

**Instal·lació:**

```bash
sudo apt install mariadb-server -y
sudo mysql_secure_installation
```

**Configuració per accés remot** (`/etc/mysql/mariadb.conf.d/50-server.cnf`):

```
bind-address = 0.0.0.0
event_scheduler = ON
default-time-zone = 'Europe/Madrid'
```

**Creació de l'usuari d'administració:**

```sql
CREATE USER 'admin'@'%' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
GRANT FILE ON *.* TO 'admin'@'%';
FLUSH PRIVILEGES;
```

**Dades de connexió:**
| Paràmetre | Valor |
|-----------|-------|
| Host intern | `10.0.0.208` |
| Host extern | `100.50.111.243` |
| Port | `3306` |
| Usuari | `admin` |
| Base de dades | `innovatetech` |

> 📸 **CAPTURA:** `systemctl status mariadb` mostrant el servei actiu.
> 📸 **CAPTURA:** `SHOW DATABASES` mostrant la base de dades `innovatetech`.

---

### 6.4. Script de Creació d'Usuaris

S'ha creat un script Bash (`create_users.sh`) que automatitza la creació d'usuaris a MariaDB. L'script demana interactivament les dades mínimes, valida el rol, comprova si l'usuari ja existeix i genera un fitxer `.sql` d'auditoria.

**Funcionalitats:**
- Creació interactiva d'un o més usuaris
- Validació del rol (admin, vendes, administracio, treballador)
- Detecció d'usuaris duplicats
- Generació automàtica de `usuaris_creats.sql`
- `GRANT FILE` automàtic per als usuaris amb rol `admin`

**Script `~/scripts/create_users.sh`:**

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
        echo "-- [ERROR] Usuari '$usuari'@'$host' ja existia." >> "$OUTPUT_FILE"
        return 1
    fi

    if ! rol_valid "$rol"; then
        echo "[ERROR] El rol '$rol' no és vàlid."
        echo "-- [ERROR] Rol '$rol' no vàlid per a '$usuari'." >> "$OUTPUT_FILE"
        return 1
    fi

    local sql_create="CREATE USER '$usuari'@'$host' IDENTIFIED BY '$contrasenya';"
    local sql_grant="GRANT '$rol' TO '$usuari'@'$host';"
    local sql_default="SET DEFAULT ROLE '$rol' FOR '$usuari'@'$host';"
    local sql_file=""
    if [[ "$rol" == "admin" ]]; then
        sql_file="GRANT FILE ON *.* TO '$usuari'@'$host';"
    fi
    local sql_flush="FLUSH PRIVILEGES;"

    mysql -h "$DB_HOST" -P "$DB_PORT" \
        -u "$DB_ADMIN" -p"$DB_PASS" \
        -e "$sql_create $sql_grant $sql_default ${sql_file:+$sql_file} $sql_flush" 2>/dev/null

    if [[ $? -eq 0 ]]; then
        echo "[OK] Usuari '$usuari'@'$host' creat amb rol '$rol'."
        echo "" >> "$OUTPUT_FILE"
        echo "-- Usuari: $usuari | Rol: $rol | Host: $host" >> "$OUTPUT_FILE"
        echo "$sql_create" >> "$OUTPUT_FILE"
        echo "$sql_grant" >> "$OUTPUT_FILE"
        echo "$sql_default" >> "$OUTPUT_FILE"
        [[ -n "$sql_file" ]] && echo "$sql_file" >> "$OUTPUT_FILE"
        echo "$sql_flush" >> "$OUTPUT_FILE"
    else
        echo "[ERROR] No s'ha pogut crear '$usuari'@'$host'."
        return 1
    fi
}

echo "-- INNOVATETECH - Usuaris generats automàticament" > "$OUTPUT_FILE"
echo "-- Data: $(date '+%Y-%m-%d %H:%M:%S')" >> "$OUTPUT_FILE"

echo "============================================"
echo " INNOVATETECH - Creació d'usuaris MariaDB"
echo "============================================"
echo "Rols disponibles: ${ROLS_VALIDS[*]}"

while true; do
    read -p "Nom d'usuari (o 'sortir' per acabar): " usuari
    [[ "$usuari" == "sortir" ]] && break
    [[ -z "$usuari" ]] && echo "[ERROR] Nom buit." && continue
    read -p "Contrasenya: " contrasenya
    [[ -z "$contrasenya" ]] && echo "[ERROR] Contrasenya buida." && continue
    read -p "Rol (${ROLS_VALIDS[*]}): " rol
    [[ -z "$rol" ]] && echo "[ERROR] Rol buit." && continue
    read -p "Host (per defecte '%'): " host
    [[ -z "$host" ]] && host="%"
    crear_usuari "$usuari" "$contrasenya" "$rol" "$host"
done

echo "Fitxer SQL generat: $OUTPUT_FILE"
```

> 📸 **CAPTURA:** Execució de `create_users.sh` creant un usuari amb rol `admin` mostrant el missatge `[OK]`.
> 📸 **CAPTURA:** Contingut de `usuaris_creats.sql` mostrant les sentències `CREATE USER`, `GRANT` i `GRANT FILE`.

---

### 6.5. Rols i Permisos

S'han creat 4 rols a MariaDB amb permisos diferenciats seguint el principi de mínim privilegi:

| Rol | Permisos |
|-----|----------|
| `admin` | `ALL PRIVILEGES` + `GRANT FILE` |
| `vendes` | `SELECT/INSERT/UPDATE` sobre Clients, Comandes, Productes, Cistell, Trucades, Usuaris, Config_Qualitat |
| `administracio` | `SELECT/INSERT/UPDATE` sobre Empleats, Departaments, Usuaris, Config_Qualitat, Mesures_Amplada_Banda |
| `treballador` | `SELECT` sobre Productes, Cataleg_Videos, Config_Qualitat + `SELECT/INSERT` sobre Trucades |

**Creació dels rols:**

```sql
CREATE ROLE 'admin';
CREATE ROLE 'vendes';
CREATE ROLE 'administracio';
CREATE ROLE 'treballador';
```

> 📸 **CAPTURA:** `SELECT Host, User, is_role FROM mysql.user WHERE is_role='Y'` mostrant els 4 rols.
> 📸 **CAPTURA:** `SHOW GRANTS FOR 'vendes'` i `SHOW GRANTS FOR 'administracio'` mostrant els permisos diferenciats.

---

### 6.6. Triggers, Events i Auditoria

S'han implementat 6 triggers i 1 event periòdic per garantir la seguretat, el control d'accés i les còpies de seguretat automàtiques.

#### Triggers implementats

**1. `trg_bloqueig_usuari`** — Impedeix trucades si l'usuari origen o destí està bloquejat.

**2. `trg_quota_minuts_mensuals`** — Bloqueja noves trucades si l'usuari supera els 600 minuts mensuals.

**3. `trg_quota_trucades_diaries`** — Bloqueja noves trucades si l'usuari supera les 20 trucades diàries.

**4. `trg_audit_empleats_update`** — Registra a `Taula_Avisos` qualsevol intent de modificar `Empleats` per part d'un usuari sense rol `admin` o `administracio`.

**5. `trg_audit_comandes_delete`** — Registra i bloqueja intents d'eliminar registres de `Comandes` per part d'usuaris no autoritzats.

**6. `trg_audit_trucades_admin`** — Registra i bloqueja intents del rol `administracio` d'insertar a la taula `Trucades`.

Tots els intents bloquejats queden registrats a la taula `Taula_Avisos` amb: usuari, taula afectada, operació, data/hora i detalls.

> 📸 **CAPTURA:** `SELECT * FROM Taula_Avisos` mostrant registres reals de triggers disparats.

#### Event periòdic de backup

L'event `evt_backup_diari` s'executa cada dia a les 02:00 i exporta les taules crítiques en format CSV a `/var/backups/innovatetech/`.

**Taules exportades:** `Empleats`, `Clients`, `Comandes`, `Trucades`

**Registre:** Cada execució queda registrada a la taula `Control_Backup` amb data, taules incloses i resultat.

**Configuració:**
- Periodicitat: diària a les 02:00 (franja de mínim tràfic)
- Format: CSV amb separador `;` i camps entre cometes
- Nom de fitxer: `taula_YYYYMMDD_HHMMSS.csv`

```sql
SHOW EVENTS FROM innovatetech;
SHOW VARIABLES LIKE 'event_scheduler';
```

> 📸 **CAPTURA:** `SHOW EVENTS FROM innovatetech` mostrant `evt_backup_diari` amb estat `ENABLED`.
> 📸 **CAPTURA:** `SHOW VARIABLES LIKE 'event_scheduler'` mostrant valor `ON`.

---

### 6.7. Incidències i Solucions

**Problema: Taula_Avisos usa motor MyISAM**

La taula `Taula_Avisos` s'ha creat amb motor `MyISAM` en lloc d'`InnoDB`. Això és necessari perquè els triggers que insereixen a aquesta taula s'executen dins de transaccions que poden ser revertides (`ROLLBACK`). Amb `InnoDB`, si el trigger falla i la transacció es reverteix, el registre d'auditoria també desapareix. Amb `MyISAM`, les insercions a `Taula_Avisos` són permanents independentment del resultat de la transacció principal.

**Problema: ERROR 1901 — CHECK clause no suportat**

En crear les taules `Usuaris` i `Trucades`, MariaDB va retornar l'error `ERROR 1901 (HY000): Function or expression cannot be used in the CHECK clause` en intentar validar condicions creuades entre columnes nullables.

**Causa:** MariaDB no admet restriccions `CHECK` que comparin columnes entre si directament a la definició de la taula.

**Solució:** Es van eliminar les restriccions `CHECK` de la definició estructural de les taules i es va traslladar aquesta lògica de validació als **triggers** (secció 6.6), que és la capa adequada per a validacions complexes en MariaDB.

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
