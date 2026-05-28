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

## Introducció

Aquest document recull tota la infraestructura desplegada al núvol AWS per a l'empresa fictícia **InnovateTech**, una empresa dedicada a la provisió de serveis tecnològics. L'objectiu és dissenyar i implementar un Centre de Processament de Dades (CPD) virtual al núvol que doni suport a totes les operacions de l'empresa, incloent-hi la gestió d'usuaris, la distribució de continguts multimèdia i la comunicació interna.

Tota la infraestructura s'ha desplegat a la regió `us-east-1` (N. Virginia) d'AWS, aprofitant els serveis de còmput, xarxa i emmagatzematge que ofereix el núvol per construir una solució escalable, segura i amb alta disponibilitat.

---

## Índex
3.1. [Infraestructura general](#1-infraestructura-general)
3.2. [Par de claus SSH](#2-par-de-claus-ssh)
3.3. [VPC — Xarxa privada virtual](#3-vpc--xarxa-privada-virtual)
3.4. [Security Group — Firewall](#4-security-group--firewall)
3.5. [Instàncies EC2](#5-instàncies-ec2)
3.6. [IPs elàstiques](#6-ips-elàstiques)
3.7. [Usuari admintech](#7-usuari-admintech)
3.8. [OpenLDAP — Directori actiu](#8-openldap--directori-actiu)
3.9. [NGINX — Servidor web](#9-nginx--servidor-web)
3.10. [SFTP autenticat amb LDAP](#10-sftp-autenticat-amb-ldap)
3.11. [Rsyslog — Centralització de logs](#11-rsyslog--centralització-de-logs)
3.12. [MariaDB — Base de dades](#12-mariadb--base-de-dades)
3.13. [Ansible — Automatització](#13-ansible--automatització)
3.14. [Problemes i solucions](#14-problemes-i-solucions)

---

## 3.1. Infraestructura general

La infraestructura d'InnovateTech es basa en **4 instàncies EC2** interconnectades dins d'una mateixa VPC. Cada instància té un rol específic i els serveis estan separats per màquina per garantir l'aïllament, la seguretat i la facilitat de manteniment.

La comunicació entre màquines es fa sempre per **IP privada**, que és permanent i no canvia entre sessions. Les IPs públiques (elàstiques) s'utilitzen únicament per a l'accés extern des d'internet.

| Màquina | Servei | IP Privada | IP Elàstica |
|---------|--------|------------|-------------|
| innovatetech-ldap | OpenLDAP | 10.0.6.122 | 100.28.104.126 |
| innovatetech-db | MariaDB | 10.0.0.208 | 100.50.111.243 |
| innovatetech-logs | Rsyslog | 10.0.9.98 | 3.208.185.55 |
| innovatetech-web | NGINX + SFTP | 10.0.7.135 | 35.171.63.1 |

<img width="900" height="346" alt="image" src="https://github.com/user-attachments/assets/b873146f-4d17-46af-b7cc-565107bd2e72" />


---
## 3.2. Par de claus SSH

Per connectar-se a les instàncies EC2 de forma segura, AWS utilitza un sistema d'autenticació basat en claus pública/privada. En lloc de fer servir contrasenyes (que poden ser vulnerables a atacs de força bruta), es genera un parell de claus RSA: la clau pública es guarda a les màquines i la clau privada la té únicament l'administrador.

Es va crear el parell de claus des de la consola AWS amb els paràmetres següents:
- **Nom:** `innovatetech-key`
- **Tipus:** RSA
- **Format:** `.pem`

Un cop descarregat el fitxer `.pem`, cal restringir-ne els permisos per evitar que SSH el rebutgi per ser "massa accessible".

**A Windows (PowerShell):**
```powershell
icacls "C:\Users\gamer\Downloads\innovatetech-key.pem" /inheritance:r
icacls "C:\Users\gamer\Downloads\innovatetech-key.pem" /grant:r "gamer:R"
```

**A Ubuntu/Linux:**
```bash
chmod 400 ~/Baixades/innovatetech-key.pem
```

> 📸 **CAPTURA:** Pantalla de creació del Key Pair a la consola AWS.

---

## 3.3. VPC — Xarxa privada virtual

Una **VPC (Virtual Private Cloud)** és una xarxa privada virtual dins d'AWS que aïlla els nostres recursos de la resta d'usuaris del núvol. És l'equivalent a tenir la nostra pròpia xarxa local, però al núvol. Sense VPC, les instàncies no es podrien comunicar entre elles de forma segura.

S'ha creat una VPC amb una subxarxa pública que permet que les màquines tinguin accés a internet, necessari tant per a l'administració remota com per als serveis que han de ser accessibles externament.

**Configuració:**
- **Nom:** `innovatetech-vpc`
- **CIDR IPv4:** `10.0.0.0/16` (65.536 adreces IP disponibles)
- **Subxarxes:** 1 subxarxa pública
- **Internet Gateway:** creat i associat automàticament
- **NAT Gateway:** cap (no necessari i té cost addicional)
- **DNS hostnames:** activat per poder resoldre noms de les instàncies

> 📸 **CAPTURA:** Diagrama de la VPC a la consola AWS mostrant la subxarxa, l'Internet Gateway i la taula d'enrutament.

---
## 3.4. Security Group — Firewall

Un **Security Group** és el firewall virtual d'AWS que controla el tràfic entrant i sortint de les instàncies. S'han definit regles específiques per a cada servei, restringint l'accés als ports sensibles únicament a la xarxa interna `10.0.0.0/16` i deixant oberts al públic només els ports estrictament necessaris.

Aquesta configuració segueix el principi de **mínim privilegi**: cada port només és accessible des d'on cal, i tot el que no s'especifica explícitament queda bloquejat.

**Nom:** `innovatetech-sg`

| Port | Protocol | Origen | Servei | Justificació |
|------|----------|--------|--------|--------------|
| 22 | TCP | 0.0.0.0/0 | SSH | Administració remota |
| 80 | TCP | 0.0.0.0/0 | HTTP | Web pública |
| 443 | TCP | 0.0.0.0/0 | HTTPS | Web segura |
| 389 | TCP | 10.0.0.0/16 | LDAP | Només xarxa interna |
| 636 | TCP | 10.0.0.0/16 | LDAPS | Només xarxa interna |
| 3306 | TCP | 10.0.0.0/16 | MariaDB | Només xarxa interna |
| 514 | TCP/UDP | 10.0.0.0/16 | Syslog | Només xarxa interna |
| 1935 | TCP | 0.0.0.0/0 | RTMP | Streaming vídeo |
| 8000 | TCP | 0.0.0.0/0 | Icecast | Streaming àudio |
| 10000 | TCP/UDP | 0.0.0.0/0 | Jitsi | Videoconferències |

> 📸 **CAPTURA:** Regles d'entrada del Security Group a la consola AWS.

---
## 3.5. Instàncies EC2

Les instàncies **EC2 (Elastic Compute Cloud)** són els servidors virtuals d'AWS. S'han llançat 4 instàncies, cadascuna amb un rol específic, seguint el principi de separació de serveis per garantir l'aïllament i la seguretat.

S'ha escollit **Ubuntu Server 24.04 LTS** com a sistema operatiu perquè és una distribució estable, àmpliament suportada i amb una gran comunitat. El tipus **t2.micro** és l'opció gratuïta disponible a les comptes d'AWS Academy i és suficient per als serveis que s'han de desplegar en un entorn de proves.

**Configuració comuna de totes les instàncies:**
- **AMI:** Ubuntu Server 24.04 LTS (HVM)
- **Tipus d'instància:** t2.micro (1 vCPU, 1 GB RAM)
- **Key pair:** `innovatetech-key`
- **VPC:** `innovatetech-vpc`
- **Subxarxa:** pública
- **IP pública automàtica:** activada
- **Security Group:** `innovatetech-sg`

| Instància | Storage | Motiu |
|-----------|---------|-------|
| innovatetech-web | 8 GiB | Suficient per NGINX |
| innovatetech-ldap | 8 GiB | Suficient per OpenLDAP |
| innovatetech-logs | 8 GiB | Suficient per rsyslog |
| innovatetech-db | 8 GiB | Suficient per MariaDB |

> 📸 **CAPTURA:** Llistat de les 4 instàncies EC2 en estat "running" amb totes les comprovacions en verd.

---
## 3.6. IPs elàstiques

Un dels problemes de les comptes d'AWS Academy és que les **IPs públiques canvien cada vegada que es reinicia el laboratori**. Això és un problema perquè caldria actualitzar les configuracions a cada sessió.

La solució és assignar **IPs elàstiques** a les màquines principals. Una IP elàstica és una adreça IP pública estàtica reservada al nostre compte que no canvia mai, independentment de si la instància s'atura o es reinicia.

Per a la comunicació **interna** entre màquines s'utilitzen les IPs privades, que també són permanents i no canvien mai.

| Màquina | IP Privada (permanent) | IP Elàstica (permanent) |
|---------|----------------------|------------------------|
| innovatetech-ldap | 10.0.6.122 | 100.28.104.126 |
| innovatetech-db | 10.0.0.208 | 100.50.111.243 |
| innovatetech-logs | 10.0.9.98 | 3.208.185.55 |
| innovatetech-web | 10.0.7.135 | 35.171.63.1 |

> 📸 **CAPTURA:** Llistat d'IPs elàstiques a la consola AWS amb les instàncies associades.

---
## 3.7. Usuari admintech

El projecte exigeix que les màquines s'administrin amb un **usuari específic**, no el per defecte (`ubuntu`). Això és una bona pràctica de seguretat: limita l'exposició del compte per defecte i permet auditar millor qui fa cada acció.

L'usuari `admintech` s'ha creat a totes les màquines amb les característiques següents:
- Pertany al grup `sudo` per poder executar comandes com a administrador
- L'accés es fa exclusivament amb **clau pública/privada**, sense contrasenya
- Pot executar `sudo` sense contrasenya per facilitar l'automatització amb Ansible

```bash
# Creació de l'usuari
sudo adduser --disabled-password --gecos '' admintech
sudo usermod -aG sudo admintech

# Configuració de l'accés per clau
sudo mkdir -p /home/admintech/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/admintech/.ssh/
sudo chown -R admintech:admintech /home/admintech/.ssh
sudo chmod 700 /home/admintech/.ssh
sudo chmod 600 /home/admintech/.ssh/authorized_keys

# Sudo sense contrasenya (necessari per Ansible)
echo 'admintech ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/admintech
```

> 📸 **CAPTURA:** Connexió SSH a una de les màquines amb l'usuari admintech mostrant el prompt.

---
## 3.8. OpenLDAP — Directori actiu

**OpenLDAP** és un servidor de directori de codi obert que implementa el protocol LDAP (Lightweight Directory Access Protocol). S'utilitza per centralitzar la gestió d'usuaris i grups de l'empresa, de manera que un usuari es crea una sola vegada i pot autenticar-se a múltiples serveis (SFTP, web, etc.) amb les mateixes credencials.

S'ha instal·lat a la màquina `innovatetech-ldap` i s'ha configurat el domini `innovatetech.local` com a base del directori.

### Estructura organitzativa

L'estructura del directori reflecteix l'organigrama de l'empresa, amb 4 departaments i els seus respectius grups i usuaris:

```
dc=innovatetech,dc=local
├── ou=usuarios
│   ├── ou=vendes        → venda1, venda2, venda3
│   ├── ou=suport        → suport1, suport2, suport3
│   ├── ou=administracio → admin1, admin2, admin3
│   └── ou=logistica     → logis1, logis2, logis3
└── ou=grupos
    ├── cn=admin         (gid 3000)
    ├── cn=vendes        (gid 3001)
    ├── cn=administracio (gid 3002)
    ├── cn=treballador   (gid 3003)
    ├── cn=suport        (gid 3004)
    └── cn=logistica     (gid 3005)
```

### Instal·lació
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install slapd ldap-utils -y
sudo dpkg-reconfigure slapd
```

### Verificació del servei
```bash
sudo systemctl status slapd
ldapsearch -x -H ldap://localhost -b "dc=innovatetech,dc=local"
ldapsearch -x -H ldap://localhost -b "ou=usuarios,dc=innovatetech,dc=local"
```

> 📸 **CAPTURA:** Resultat del ldapsearch mostrant la estructura completa amb tots els usuaris i grups.

> 📸 **CAPTURA:** systemctl status slapd mostrant el servei actiu i en execució.

---
## 3.9. NGINX — Servidor web

**NGINX** és un servidor web d'alt rendiment escollit per la seva eficiència i la seva àmplia adopció en entorns empresarials. S'ha instal·lat a la màquina `innovatetech-web` i s'ha configurat un Virtual Host per servir la pàgina corporativa d'InnovateTech.

S'ha escollit NGINX per sobre d'Apache perquè és el que s'ha practicat a classe i perquè el seu model de gestió de connexions asíncrones el fa més eficient per a càrregues elevades.

### Instal·lació i configuració
```bash
sudo apt install nginx -y
sudo mkdir -p /var/www/innovatetech
```

### Virtual Host `/etc/nginx/sites-available/innovatetech`
```nginx
server {
    listen 80;
    server_name 35.171.63.1;
    root /var/www/innovatetech;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

### Activació
```bash
sudo ln -s /etc/nginx/sites-available/innovatetech /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo systemctl restart nginx
```

La pàgina web corporativa és accessible des de:
- **IP:** http://35.171.63.1
- **Domini:** http://innovatetech-itb.duckdns.org (DuckDNS gratuït)

> 📸 **CAPTURA:** Pàgina web d'InnovateTech oberta al navegador mostrant el domini DuckDNS.

> 📸 **CAPTURA:** systemctl status nginx mostrant el servei actiu.

---


## 10. SFTP autenticat amb LDAP

El servei **SFTP (SSH File Transfer Protocol)** permet la transferència segura de fitxers entre clients i el servidor. S'ha configurat per departament, de manera que cada grup d'usuaris té accés exclusivament a la seva carpeta, sense poder veure les carpetes dels altres departaments. L'autenticació es fa directament contra el servidor LDAP, de manera que els usuaris utilitzen les mateixes credencials que al directori actiu.

### Com funciona
1. L'usuari LDAP es connecta per SFTP amb el seu usuari i contrasenya
2. El sistema consulta LDAP per verificar les credencials (`libpam-ldap`)
3. El sistema identifica el grup al qual pertany l'usuari (`libnss-ldap`)
4. SSH aplica la regla `Match Group` corresponent i el confina (chroot) a la seva carpeta de departament
5. L'usuari només pot veure i modificar fitxers de la seva carpeta

### Paquets necessaris
```bash
sudo apt install libpam-ldap libnss-ldap ldap-utils nscd -y
```

- `libpam-ldap` — connecta el sistema d'autenticació PAM amb LDAP
- `libnss-ldap` — permet resoldre usuaris i grups des de LDAP
- `nscd` — caché de noms per millorar el rendiment

### Configuració `/etc/nsswitch.conf`
```
passwd:   files ldap
group:    files ldap
shadow:   files ldap
```

### Estructura de carpetes
```
/sftp/
├── vendes/
│   └── uploads/     ← venda1, venda2, venda3
├── suport/
│   └── uploads/     ← suport1, suport2, suport3
├── administracio/
│   └── uploads/     ← admin1, admin2, admin3
└── logistica/
    └── uploads/     ← logis1, logis2, logis3
```

### Regles chroot a `/etc/ssh/sshd_config`
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

### Prova de connexió
```bash
sftp venda1@35.171.63.1    # entra a /sftp/vendes
sftp suport1@35.171.63.1   # entra a /sftp/suport
sftp admin1@35.171.63.1    # entra a /sftp/administracio
sftp logis1@35.171.63.1    # entra a /sftp/logistica
```

> 📸 **CAPTURA:** Connexió SFTP amb venda1 mostrant la carpeta uploads i el fitxer de prova.

> 📸 **CAPTURA:** Connexió SFTP amb suport1, admin1 i logis1 mostrant les seves respectives carpetes.

---

## 11. Rsyslog — Centralització de logs

**Rsyslog** és un sistema de gestió de logs que permet centralitzar els registres de totes les màquines de la infraestructura en un únic servidor. Això és fonamental per a la monitorització i l'auditoria del sistema: en lloc d'haver d'entrar a cada màquina per revisar els seus logs, tots els registres es concentren a un sol lloc.

S'ha configurat `innovatetech-logs` com a servidor central de logs, i la resta de màquines com a clients que envien els seus registres a aquest servidor per TCP (protocol `@@`) per garantir l'entrega.

S'utilitza la **IP privada** `10.0.9.98` per a la comunicació entre màquines, ja que és permanent i no canvia entre sessions del laboratori.

### Configuració del servidor (innovatetech-logs)

S'activa la recepció de logs per UDP i TCP al fitxer `/etc/rsyslog.conf`:
```
module(load="imudp")
input(type="imudp" port="514")
module(load="imtcp")
input(type="imtcp" port="514")
```

Es crea `/etc/rsyslog.d/remote.conf` per guardar els logs per màquina:
```
$template RemoteLogs,"/var/log/remote/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
```

### Configuració dels clients (resta de màquines)

A cada màquina es crea `/etc/rsyslog.d/client.conf`:
```
*.* @@10.0.9.98:514
```

### Verificació
```bash
sudo ls /var/log/remote/
# Resultat esperat:
# ip-10-0-6-122  ip-10-0-0-208  ip-10-0-7-135  ip-10-0-7-77  ip-10-0-9-98
```

> 📸 **CAPTURA:** Resultat de `ls /var/log/remote/` mostrant les carpetes de totes les màquines.

> 📸 **CAPTURA:** Contingut de la carpeta d'una màquina mostrant els fitxers de log rebuts.

---

## 12. MariaDB — Base de dades

**MariaDB** és un sistema gestor de bases de dades relacional de codi obert, compatible al 100% amb MySQL. S'ha instal·lat a la màquina `innovatetech-db` per allotjar la base de dades integral de l'empresa.

S'ha escollit MariaDB per sobre de MySQL perquè és més lleugera (important en una instància t2.micro amb només 1 GB de RAM), és de codi obert sense restriccions de llicència i els repositoris d'Ubuntu 24.04 la ofereixen com l'opció per defecte.

### Instal·lació i securització
```bash
sudo apt install mariadb-server -y
sudo mysql_secure_installation
```

La comanda `mysql_secure_installation` és fonamental per securitzar la instal·lació: elimina usuaris anònims, desactiva l'accés remot de root i elimina la base de dades de proves que ve per defecte.

### Configuració per accés remot

Per permetre que les aplicacions i el companyons es connectin des d'altres màquines, es modifica `/etc/mysql/mariadb.conf.d/50-server.cnf`:
```
bind-address = 0.0.0.0
```

### Creació de l'usuari d'accés
```sql
CREATE USER 'admin'@'%' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

**Dades de connexió per al company de BD:**
- **Host:** `10.0.0.208` (xarxa interna) / `100.50.111.243` (accés extern)
- **Port:** `3306`
- **Usuari:** `admin`
- **Contrasenya:** `12345`

> 📸 **CAPTURA:** systemctl status mariadb mostrant el servei actiu.

> 📸 **CAPTURA:** Connexió a MariaDB i resultat de SHOW DATABASES.

---

## 13.Ansible — Automatització d'InnovateTech
Introducció
Ansible és una eina d'automatització de configuració que permet gestionar múltiples servidors des d'un únic node controlador, sense necessitat d'instal·lar cap agent a les màquines gestionades. Funciona per SSH, executant tasques definides en fitxers YAML anomenats playbooks.
A InnovateTech s'ha configurat `innovatetech-logs` (10.0.9.98) com a node controlador. S'han creat playbooks capaços de crear instàncies EC2 des de zero, assignar-los una IP elàstica, crear l'usuari `admintech` i configurar el servei complet de forma totalment automatitzada.
---
Estructura de fitxers
```
~/ansible/
├── ansible.cfg
├── update-credentials.sh
├── inventory/
│   └── hosts
├── roles/
│   ├── provision/
│   │   └── tasks/
│   │       └── main.yml
│   ├── web/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── files/
│   │       └── index.php
│   └── ldap/
│       └── tasks/
│           └── main.yml
├── playbook-web.yml
├── playbook-ldap.yml
├── playbook-eliminar-web.yml
├── playbook-provision-web.yml
└── playbook-provision-ldap.yml
```
---
Configuració
Inventari `inventory/hosts`
```ini
[web]
10.0.7.135

[ldap]
10.0.6.122

[all:vars]
ansible_user=admintech
ansible_ssh_private_key_file=/home/admintech/.ssh/id_rsa
```
`ansible.cfg`
```ini
[defaults]
inventory = inventory/hosts
private_key_file = ~/.ssh/id_rsa
remote_user = admintech
host_key_checking = False
deprecation_warnings = False

[privilege_escalation]
become = True
become_method = sudo
become_user = root
```
Credencials AWS
Les comptes d'AWS Academy tenen credencials temporals que canvien cada sessió (~4 hores). Es gestionen amb el script `update-credentials.sh`.
Al inici de cada sessió:
Anar al panell AWS Academy → Show a "AWS CLI"
Copiar les credencials a `~/.aws/credentials`:
```
[default]
aws_access_key_id=TU_ACCESS_KEY
aws_secret_access_key=TU_SECRET_KEY
aws_session_token=TU_SESSION_TOKEN
```
Executar:
```bash
source ~/ansible/update-credentials.sh
```
---
Rols
Rol `provision`
Crea una nova instància EC2, assigna IP elàstica i configura l'usuari `admintech`.
Tasques:
Crea la instància EC2 (Ubuntu 24.04, t2.micro)
Espera que SSH estigui disponible
Assigna IP elàstica
Mostra la IP assignada
Crea l'usuari `admintech` a la nova instància (connectant com a `ubuntu`)
Copia la clau pública al `authorized_keys` d'`admintech`
Configura `sudo` sense contrasenya per a `admintech`
Paràmetres AWS:
AMI: `ami-05cf1e9f73fbad2e2` (Ubuntu 24.04 LTS)
Tipus: `t2.micro`
Subxarxa: `subnet-022a882efecf162cd`
Security Group: `sg-053223c3c2a657989`
Key pair: `innovatetech-key`
Rol `web`
Desplega completament el servidor web:
Actualitza paquets
Instal·la NGINX
Instal·la PHP 8.3 + FPM + mysql + ldap
Inicia PHP-FPM
Crea directori `/var/www/innovatetech`
Elimina `index.html` si existeix
Desplega `index.php` (web corporativa amb gestió BD)
Crea Virtual Host amb suport PHP
Activa Virtual Host i elimina el per defecte
Preconfigura i instal·la `libpam-ldap`, `libnss-ldap`, `nscd`
Configura `nsswitch.conf` per LDAP
Configura `ldap.conf` apuntant a `10.0.6.122`
Crea carpetes SFTP per departament
Configura chroot SFTP per grup
Habilita `PasswordAuthentication`
Reinicia SSH i NGINX
Rol `ldap`
Instal·la OpenLDAP des de zero amb tota l'estructura d'InnovateTech:
Desinstal·la `slapd` amb `purge`
Elimina fitxers residuals
Preconfigura amb `debconf` (domini, organització, contrasenya)
Instal·la `slapd`, `ldap-utils`, `python3-ldap`
Inicia el servei
Crea OUs: `usuarios`, `grupos`
Crea OUs per departament: `vendes`, `suport`, `administracio`, `logistica`, `admin`
Crea grups amb GIDs (3000-3005)
Crea 12 usuaris de departament (3 per departament)
Crea usuaris de gestió BD: `bd1`, `bd2`, `bd3` (contrasenya: `bd1234`)
---
Playbooks
`playbook-web.yml` i `playbook-ldap.yml`
Configuren les màquines ja existents a l'inventari.
```bash
cd ~/ansible
ansible-playbook -i inventory/hosts playbook-web.yml
ansible-playbook -i inventory/hosts playbook-ldap.yml
```
`playbook-eliminar-web.yml`
Simula una fallada catastròfica eliminant NGINX completament.
```bash
ansible-playbook -i inventory/hosts playbook-eliminar-web.yml
```
`playbook-provision-web.yml`
Crea una EC2 nova des de zero + configura el servidor web complet.
```bash
source ~/ansible/update-credentials.sh
ansible-playbook playbook-provision-web.yml
```
`playbook-provision-ldap.yml`
Crea una EC2 nova des de zero + configura OpenLDAP complet.
```bash
source ~/ansible/update-credentials.sh
ansible-playbook playbook-provision-ldap.yml
```
---
Demostració recuperació de desastres
```bash
# 1. Eliminar NGINX (simulació de fallada)
ansible-playbook -i inventory/hosts playbook-eliminar-web.yml

# 2. Verificar que la web no funciona
curl -I http://35.171.63.1

# 3. Recuperar el servei
ansible-playbook -i inventory/hosts playbook-web.yml

# 4. Verificar que funciona
curl -I http://35.171.63.1
```
---
Comprovacions
```bash
# Ansible ping a totes les màquines
cd ~/ansible && ansible all -m ping

# LDAP
ssh -i ~/.ssh/innovatetech-key.pem admintech@100.28.104.126 \
  "ldapsearch -x -H ldap://localhost -b 'dc=innovatetech,dc=local' | grep dn"

# Web
curl -I http://35.171.63.1

# Logs centralitzats
sudo ls /var/log/remote/

# MariaDB
ssh -i ~/.ssh/innovatetech-key.pem admintech@100.50.111.243 \
  "sudo mysql -u root -p12345 -e 'USE innovatetech; SHOW TABLES;'"
```
---
Problemes i solucions
NoCredentialsError
Causa: El mòdul `amazon.aws` no llegeix les variables d'entorn automàticament.
Solució: Passar credencials via `lookup('env', ...)` i exportar amb `source update-credentials.sh`.
skipping: no hosts matched
Causa: La IP de la nova instància no estava a l'inventari.
Solució: Usar `add_host` als `post_tasks` per afegir dinàmicament la IP al grup temporal.
Permission denied al crear admintech
Causa: Les tasques de `delegate_to` intentaven connectar amb `admintech` però la instància nova només tenia `ubuntu`.
Solució: Afegir `vars: ansible_user: ubuntu` a cada tasca `delegate_to`.
AddressLimitExceeded
Causa: Límit de 5 IPs elàstiques a les comptes Academy.
Solució: Alliberar IPs elàstiques de les instàncies de prova abans d'executar els playbooks de provisió.
index.php es descarrega en lloc d'executar-se
Causa: El Virtual Host no tenia configuració PHP-FPM.
Solució: Afegir al rol `web` la instal·lació de `php8.3-fpm` i el bloc `location ~ \.php$` al Virtual Host.
---
> 📸 **CAPTURA:** `ansible all -m ping` mostrant totes les màquines en SUCCESS
> 📸 **CAPTURA:** Execució de `playbook-provision-web.yml` completada sense errors
> 📸 **CAPTURA:** Execució de `playbook-provision-ldap.yml` completada sense errors
> 📸 **CAPTURA:** Web funcionant a la nova instància creada per Ansible
> 📸 **CAPTURA:** ldapsearch mostrant l'estructura completa creada pel playbook
---

---
Problemes i solucions
Problema — NoCredentialsError en els playbooks de provisió
Causa: El mòdul `amazon.aws` d'Ansible usa la seva pròpia versió de boto3, que no llegeix automàticament les variables d'entorn exportades amb `source`.
Solució: Passar les credencials explícitament als mòduls via `lookup('env', ...)` i exportar-les prèviament amb `source ~/ansible/update-credentials.sh`.
Problema — `skipping: no hosts matched` al segon play
Causa: La IP de la nova instància creada pel rol `provision` no estava a l'inventari, per la qual cosa Ansible no sabia on connectar-se per configurar-la.
Solució: Usar `add_host` als `post_tasks` del primer play per afegir dinàmicament la nova IP a un grup temporal `nova_instancia`, que s'usa com a `hosts` al segon play.
Problema — `AddressLimitExceeded` en assignar IP elàstica
Causa: Les comptes d'AWS Academy tenen un límit de 5 IPs elàstiques simultànies.
Solució: Alliberar les IPs elàstiques de les instàncies de prova abans d'executar els playbooks de provisió.
Problema — Permission denied en connectar a la nova instància
Causa: Ansible intentava connectar amb `~/.ssh/id_rsa` però la nova instància només accepta `innovatetech-key.pem`.
Solució: Copiar el fitxer `.pem` al servidor de logs i especificar-lo a `ansible_ssh_private_key_file` al `add_host`.
---
> 📸 **CAPTURA:** Execució de `playbook-provision-web.yml` mostrant totes les tasques completades.
> 📸 **CAPTURA:** Pàgina web funcionant a la nova instància creada per Ansible.
> 📸 **CAPTURA:** Execució de `playbook-provision-ldap.yml` mostrant totes les tasques completades.
> 📸 **CAPTURA:** ldapsearch a la nova instància LDAP mostrant l'estructura creada per Ansible.


## 14. Problemes i solucions

Durant el desplegament de la infraestructura es van trobar diversos problemes. A continuació es detallen els més rellevants amb les seves solucions.

---

### Problema 1 — Permisos del fitxer .pem a Windows

**Descripció:** En intentar connectar-se per SSH des de Windows amb el fitxer `.pem`, SSH mostrava l'error "UNPROTECTED PRIVATE KEY FILE" i denegava la connexió perquè el fitxer era accessible per altres usuaris del sistema.

**Causa:** Windows no restringeix automàticament els permisos dels fitxers descarregats, i SSH requereix que la clau privada només sigui llegible pel propietari.

**Solució:**
```powershell
icacls "innovatetech-key.pem" /inheritance:r
icacls "innovatetech-key.pem" /grant:r "gamer:R"
icacls "innovatetech-key.pem" /remove "Pc-Ay-Ou\ayman"
```

---

### Problema 2 — IPs públiques canvien cada sessió

**Descripció:** Cada vegada que es reiniciava el laboratori d'AWS Academy, les IPs públiques de totes les instàncies canviaven, obligant a actualitzar totes les configuracions.

**Causa:** Les comptes d'AWS Academy no mantenen les IPs públiques entre sessions per defecte.

**Solució:** Es van assignar **IPs elàstiques** a les 4 màquines principals. Per a la comunicació interna entre serveis (rsyslog clients, configuració LDAP, etc.) s'utilitzen les **IPs privades**, que mai canvien.

---

### Problema 3 — sudo demanava contrasenya en scripts remots

**Descripció:** En executar comandes amb `sudo` via SSH de forma no interactiva (des de scripts PowerShell), el sistema demanava contrasenya i els scripts fallaven.

**Causa:** L'usuari `admintech` estava configurat per demanar contrasenya per a `sudo`.

**Solució:** Configurar `admintech` per a `sudo` sense contrasenya a totes les màquines:
```bash
echo 'admintech ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/admintech
```

---

### Problema 4 — PasswordAuthentication bloquejada per AWS

**Descripció:** Els usuaris LDAP no podien connectar-se per SFTP amb contrasenya. L'error era "Permission denied (publickey)".

**Causa:** AWS crea un fitxer `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf` amb `PasswordAuthentication no` que **sobreescriu** la configuració del `sshd_config` principal, ja que els fitxers de la carpeta `.d/` tenen prioritat.

**Solució:** Modificar específicament aquest fitxer:
```bash
sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' \
  /etc/ssh/sshd_config.d/60-cloudimg-settings.conf
sudo systemctl restart ssh
```

---

### Problema 5 — NGINX no arrencava després de reinstal·lació

**Descripció:** En eliminar el directori `/etc/nginx` amb Ansible per fer la demostració de recuperació de desastres, en reinstal·lar NGINX el paquet no recreava el directori ni el `nginx.conf`, deixant el servei en estat fallit amb l'error "open() /etc/nginx/nginx.conf failed".

**Causa:** `dpkg` detectava que el paquet estava parcialment configurat i no completava la instal·lació correctament.

**Solució:** Eliminar completament l'estat del paquet i reinstal·lar des d'una nova instància EC2:
```bash
sudo dpkg --remove --force-remove-reinstreq nginx
sudo rm -rf /etc/nginx /var/log/nginx /var/lib/nginx
sudo apt clean && sudo apt update
```
Finalment es va optar per crear una nova instància EC2 i desplegar-la amb el playbook d'Ansible, demostrant precisament la utilitat d'aquesta eina.

---

### Problema 6 — Grups LDAP duplicats per suport i logística

**Descripció:** Els usuaris de `suport` i `logistica` entraven al SFTP i veien el sistema de fitxers arrel en lloc de la seva carpeta de departament.

**Causa:** Al disseny inicial, `suport` i `logistica` compartien el gid `3003` (treballador) perquè no tenien grups específics. Les regles `Match Group suport` i `Match Group logistica` al `sshd_config` no funcionaven perquè aquests grups no existien al sistema.

**Solució:** Crear grups LDAP específics:
- `suport` → gid 3004
- `logistica` → gid 3005

Reassignar els usuaris als nous grups i actualitzar les regles `Match Group` al `sshd_config` usant noms de grup en lloc de gids numèrics.

---

### Problema 7 — Ansible instal·lat a la màquina incorrecta

**Descripció:** Per error, Ansible es va instal·lar a `innovatetech-web` en lloc de `innovatetech-logs`.

**Solució:** Desinstal·lar d'on no tocava i instal·lar al node controlador correcte:
```bash
# Desinstal·lar del web
sudo apt remove ansible -y

# Instal·lar al servidor de logs
sudo apt install ansible -y
```

---

### Problema 8 — ssh-copy-id denegat entre EC2

**Descripció:** No es podia copiar la clau pública d'Ansible entre màquines amb `ssh-copy-id` perquè les EC2 només accepten la clau original `.pem` per defecte.

**Causa:** El fitxer `authorized_keys` de les màquines només contenia la clau pública corresponent al `.pem` original.

**Solució:** Copiar manualment el contingut de la clau pública del node controlador al `authorized_keys` de cada màquina gestionada des del terminal local:
```bash
ssh -i ~/Baixades/innovatetech-key.pem admintech@IP_MAQUINA \
  "echo 'CLAU_PUBLICA_LOGS' >> /home/admintech/.ssh/authorized_keys"
```

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
