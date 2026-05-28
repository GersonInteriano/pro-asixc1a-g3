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
<br><br>

_[Enlace al plano logico](https://mermaid.ai/d/1d8e0a02-9697-4f68-99f1-cd7f8237895b)_




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

Índex


Introducció
Arquitectura general
Par de claus SSH
VPC
Security Group
Instàncies EC2
IPs elàstiques
Usuari admintech
OpenLDAP
NGINX + PHP + Web corporativa
SFTP autenticat amb LDAP
Rsyslog — Centralització de logs
MariaDB
Ansible
Aplicació web — Gestió BD i Streaming
Problemes i solucions
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
innovatetech-media	Àudio/Vídeo/Jitsi	10.0.7.77	—
La comunicació interna entre màquines es fa sempre per IP privada, que és permanent. Les IPs elàstiques s'utilitzen per a l'accés extern.
> 📸 **CAPTURA:** Panell EC2 mostrant les 5 instàncies en estat running.
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
---
6. Instàncies EC2
5 instàncies EC2 amb Ubuntu Server 24.04 LTS, t2.micro (1 vCPU, 1 GB RAM). Es va escollir Ubuntu 24.04 per la seva estabilitat i àmplia documentació. El tipus t2.micro és l'opció gratuïta de les comptes AWS Academy.
Instància	Storage	Servei
innovatetech-web	8 GiB	NGINX + PHP + SFTP
innovatetech-ldap	8 GiB	OpenLDAP
innovatetech-logs	8 GiB	Rsyslog + Ansible
innovatetech-db	8 GiB	MariaDB
innovatetech-media	20 GiB	Àudio/Vídeo/Jitsi
> 📸 **CAPTURA:** Llistat de les 5 instàncies EC2 en estat running amb les comprovacions en verd.
---
7. IPs elàstiques
Les comptes d'AWS Academy canvien les IPs públiques a cada reinici del laboratori. Per solucionar-ho s'han assignat IPs elàstiques (estàtiques) a les màquines principals.
Màquina	IP Privada (permanent)	IP Elàstica (permanent)
innovatetech-ldap	10.0.6.122	100.28.104.126
innovatetech-db	10.0.0.208	100.50.111.243
innovatetech-logs	10.0.9.98	3.208.185.55
innovatetech-web	10.0.7.135	35.171.63.1
> 📸 **CAPTURA:** Llistat d'IPs elàstiques a la consola AWS.
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
> 📸 **CAPTURA:** systemctl status slapd actiu.
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
IP: http://35.171.63.1
Domini: http://innovatetech-itb.duckdns.org (DuckDNS gratuït)
> 📸 **CAPTURA:** Pàgina web d'InnovateTech al navegador.
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
sftp venda1@35.171.63.1    # entra a /sftp/vendes
sftp suport1@35.171.63.1   # entra a /sftp/suport
sftp admin1@35.171.63.1    # entra a /sftp/administracio
sftp logis1@35.171.63.1    # entra a /sftp/logistica
```
> 📸 **CAPTURA:** Connexió SFTP amb venda1 mostrant la carpeta uploads.
> 📸 **CAPTURA:** Connexió SFTP amb els 4 departaments.
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
> 📸 **CAPTURA:** SHOW TABLES a la BD innovatetech.
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
> 📸 **CAPTURA:** Execució de `playbook-provision-web.yml` completada.
> 📸 **CAPTURA:** Execució de `playbook-provision-ldap.yml` completada.
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
> 📸 **CAPTURA:** Secció de streaming amb els dos serveis.
> 📸 **CAPTURA:** Login de gestió BD.
> 📸 **CAPTURA:** Taula d'empleats amb dades.
> 📸 **CAPTURA:** Formulari d'inserció de registre.
---
16. Problemes i solucions
P1 — Permisos del fitxer .pem a Windows
Problema: SSH rebutjava la clau privada per permisos massa oberts.
Solució:
```powershell
icacls "innovatetech-key.pem" /inheritance:r
icacls "innovatetech-key.pem" /grant:r "gamer:R"
icacls "innovatetech-key.pem" /remove "Pc-Ay-Ou\ayman"
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
