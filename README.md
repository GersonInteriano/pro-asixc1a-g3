<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/75f17801-4b98-4781-934b-30af8fb42f61" />

# Proyecto Transversal: InnovateTech — Infraestructura Híbrida
**CFGS Administració de Sistemes Informàtics en Xarxa · Curs 25/26 · Institut Tecnològic de Barcelona**

---

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
8. [Digitalización y Sostenibilidad](#8-digitalizacion-y-sostenibilidad)
9. [Conclusiones](#9-conclusiones)
10. [Anexos y Entregables](#10-anexos-y-entregables)

---

## 1. Introducción y Contexto del Proyecto

El presente proyecto tiene como finalidad diseñar e implementar una infraestructura tecnológica robusta para **InnovateTech**, una empresa en expansión dedicada a la provisión de servicios digitales. El núcleo de la propuesta es un modelo híbrido que combina la seguridad y el control de un CPD local con la escalabilidad y alta disponibilidad de la nube de Amazon Web Services (AWS).

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

El Centro de Procesamiento de Datos (CPD) local se ha concebido como el centro neurálgico de administración y conectividad de InnovateTech. Su diseño físico prioriza la integridad del hardware y la continuidad del servicio.

### 2.1. Ubicación y Acondicionamiento

La sala técnica se ha acondicionado siguiendo normativas de seguridad y eficiencia:

<img width="8192" height="5517" alt="image" src="https://github.com/user-attachments/assets/27725aa7-95fe-4cb1-b4dc-702155227373" />

_[Enlace al plano lógico](https://mermaid.ai/d/1d8e0a02-9697-4f68-99f1-cd7f8237895b)_

- **Entorno Físico:** Ubicación en una sala interior, sin ventanas y con muros de resistencia al fuego. Se utiliza suelo técnico elevado (30 cm) para la canalización oculta de cables y falso techo para la extracción de aire caliente.

- **Climatización de Precisión:** Sistema de aire acondicionado industrial manteniendo la temperatura constante entre 20°C y 22°C. El diseño utiliza la metodología de pasillos fríos y calientes para evitar puntos de calor.

- **Detección de Incendios:** Sensores ópticos de humo y temperatura.

- **Extinción:** Sistema automático mediante gas inerte (Novec o CO2), que sofoca el fuego sin dañar los componentes electrónicos ni dejar residuos.

- **Control de Acceso:** Cerradura electrónica con registro de entrada para personal autorizado.

### 2.2. Diseño de Racks y Organización

Para una gestión eficiente, la infraestructura se divide en dos armarios Rack de 42U:

#### Rack 1: Networking y Seguridad

Contiene los elementos que garantizan la comunicación interna y el enlace con AWS:
- **Patch Panels:** Gestión del cableado estructurado.
- **Router de Borde y Firewall:** Seguridad perimetral y túnel VPN con la nube.
- **Switch Core:** Dispositivo de alta velocidad (Capa 3) para distribución de VLANs.
- **SAI:** Ubicado en la parte inferior para proporcionar estabilidad eléctrica.

#### Rack 2: Gestión y Administración

Orientado al soporte administrativo y la protección de datos local:
- **Servidor de Administración Local:** Controlador de dominio secundario y gestión de políticas internas.
- **Almacenamiento NAS:** Nodo dedicado a las copias de seguridad de la base de datos y logs de AWS.
- **Consola KVM:** Para la administración física de los servidores.
- **Servidor de Monitorización:** Supervisión en tiempo real de temperatura, consumo y estado de servicios.

# 2.3 Infraestructura elèctrica i SAI — Arquitectura híbrida AWS

## Descripción general

Tras la migración de la mayor parte de los servicios corporativos a AWS, el CPD local de InnovateTech adopta un modelo híbrido ligero. La infraestructura local ya no aloja servicios críticos de producción ni aplicaciones principales, sino únicamente servicios de soporte, administración, seguridad y copias de seguridad.

Según la arquitectura definida, los servicios principales (LDAP, streaming, web, base de datos y logs) se ejecutan sobre instancias EC2 dentro de la VPC corporativa de AWS, mientras que el CPD local mantiene únicamente:

* Firewall y conectividad híbrida con AWS.
* Switch core y segmentación VLAN.
* Servidor de administración local.
* NAS corporativo de backups.
* Sistema de monitorización.
* Consola KVM y electrónica auxiliar.

Esta nueva arquitectura reduce considerablemente el consumo eléctrico, la necesidad de refrigeración y la complejidad operativa del CPD físico.

---

## Infraestructura eléctrica simplificada

La infraestructura eléctrica sigue manteniendo criterios profesionales de redundancia y alta disponibilidad, aunque adaptados al nuevo volumen real de carga.

### Alimentación redundante

El CPD dispone de:

* Doble acometida eléctrica (Feed A y Feed B).
* Cuadro General de Distribución (CGD).
* Dos líneas redundantes:
  * Línea A
  * Línea B

Cada línea alimenta un SAI independiente:

* SAI A
* SAI B

La distribución eléctrica se realiza mediante PDUs redundantes instaladas en el rack principal.

---

## Equipamiento local definitivo

### Rack 1 — Networking y Seguridad

**Firewall en alta disponibilidad**

Modelo propuesto:
* 2 × FortiGate 60F HA

Consumo operativo estimado:
* 18 W cada uno

**Switch Core Layer 3**

Modelo:
* Aruba CX 6100 24G 4SFP+

Consumo estimado:
* 45 W

**Router / SD-WAN**

Modelo:
* Ubiquiti EdgeRouter 4

Consumo estimado:
* 11 W

**Electrónica auxiliar**

Patch panels, SFP+, ventilación y gestión:
* 20 W

---

### Rack 2 — Gestión y Administración

**Servidor de administración local**

Modelo:
* Dell PowerEdge R250

Funciones:
* Controlador de dominio secundario
* Gestión interna

Consumo operativo estimado:
* 110 W

**NAS corporativo**

Modelo:
* QNAP TS-453D

Consumo operativo estimado:
* 35 W

**Servidor de monitorización**

Modelo:
* Mini PC Intel NUC / appliance de monitorización

Consumo operativo estimado:
* 25 W

**Consola KVM + periféricos**

Consumo estimado:
* 10 W

---

## Distribución de cargas en los SAI

### SAI A

| Equipo                  | Consumo |
| ----------------------- | ------- |
| Firewall principal      | 18 W    |
| Switch Core             | 45 W    |
| Servidor administración | 110 W   |
| Monitorización          | 25 W    |
| Electrónica auxiliar    | 20 W    |

**Total SAI A**
P_L_SAI_A = 218 W

Aplicando margen de seguridad del 25%:
P_{SAI\ A}=218\cdot1.25=272.5\ W

**Potencia final de diseño**
P_L_SAI_A = 272,5 W

---

### SAI B

| Equipo               | Consumo |
| -------------------- | ------- |
| Firewall secundario  | 18 W    |
| Router EdgeRouter 4  | 11 W    |
| NAS corporativo      | 35 W    |
| KVM                  | 10 W    |

**Total SAI B**
P_L_SAI_B = 74 W

Aplicando margen de seguridad del 25%:
P_{SAI\ B}=74\cdot1.25=92.5\ W

**Potencia final de diseño**
P_L_SAI_B = 92,5 W

---

## Sistema SAI seleccionado

Modelo recomendado:
* APC Smart-UPS SMTL1500RMI3UC
* Tecnología Lithium-Ion
* 1500 VA / 1350 W
* Rack 2U
* Gestión SNMP integrada

Este modelo proporciona una autonomía muy superior a los 20 minutos requeridos gracias a la drástica reducción de carga tras la migración a AWS.

---

## Parámetros utilizados para el cálculo

| Parámetro  | Valor |
| ---------- | ----- |
| V_b_total  | 48 V  |
| η_inv      | 0,90  |
| DoD        | 0,90  |
| f_p(I_d)   | 0,95  |
| f_t(T)     | 1     |
| f_e        | 1     |
| f_c        | 0,99  |
| C_n        | 3,10 Ah |

Energía nominal estimada:
E_{bat}=3.10\ Ah\cdot48\ V=148.8\ Wh

Energía neta entregable:
E_{neta}=148.8\cdot0.90\cdot0.90\cdot0.95\cdot1\cdot1\cdot0.99\approx113.36\ Wh

---

## Cálculo de autonomía

### Autonomía SAI A

t_{SAI\ A}=\frac{113.36}{272.5}=0.416\ h

Conversión a minutos:
0.416\cdot60\approx24.96\ min

**Resultado final**
Autonomía estimada SAI A:
* 25 minutos aproximadamente.

---

### Autonomía SAI B

t_{SAI\ B}=\frac{113.36}{92.5}=1.225\ h

Conversión a minutos:
1.225\cdot60\approx73.5\ min

**Resultado final**
Autonomía estimada SAI B:
* 73 minutos aproximadamente.

---

## Conclusión técnica

La migración de la infraestructura principal a AWS ha reducido drásticamente la dependencia del CPD físico local. El nuevo diseño mantiene únicamente servicios esenciales de:

* administración,
* conectividad,
* seguridad,
* monitorización,
* backups.

Esta simplificación permite:

* Reducir consumo energético.
* Reducir necesidades de refrigeración.
* Aumentar la autonomía de los SAI.
* Simplificar el mantenimiento.
* Reducir riesgos operativos.
* Mantener alta disponibilidad híbrida con AWS.

Los cálculos realizados demuestran que el sistema APC SMTL1500RMI3UC cubre ampliamente el requisito mínimo de 20 minutos de autonomía.


[⬆ Volver al índice](#tabla-de-contenidos)

---
## 2.4 Seguridad Física

### Enfoque de diseño

Aunque no protegemos servidores que facturan en tiempo real, sí custodiamos los elementos que garantizan la conectividad con la nube y la integridad de las copias de seguridad locales. Nuestro principio rector ha sido claro: **mantener un nivel de seguridad profesional sin sobredimensionar ni generar costes operativos innecesarios**.

A continuación, detallamos las medidas adoptadas, todas ellas dimensionadas para un CPD reducido a dos racks y coherentes con la filosofía de simplicidad operativa del proyecto.

---

### Control de acceso físico

Hemos descartado el despliegue de una infraestructura de control de accesos a nivel de sala (puerta blindada, tornos, lectores murales) porque el nuevo emplazamiento probablemente sea un habitáculo técnico compartido o un espacio de oficina adaptado. En su lugar, trasladamos la barrera de seguridad directamente al armario.

**Solución implantada**

- **Armario rack con cerradura de seguridad**, paneles frontal y trasero con llave. Modelo de referencia: armario de 42U con puerta perforada y cerradura de tres puntos.
- **Cerradura electrónica RFID + PIN** instalada en la propia puerta del rack. Hemos seleccionado un kit de control de acceso para rack compatible con tarjetas MIFARE y registro horario (logs almacenados en memoria interna con descarga USB).
- **Política de acceso por roles**: solo los miembros del equipo de administración de sistemas tienen tarjeta habilitada. El registro digital nos permite auditoría de accesos sin necesidad de software adicional.

**Por qué lo hemos hecho así**

- La solución no requiere obra civil, se instala en el propio rack y cubre el 100% de los activos físicos.
- Mantenemos la doble barrera (llave + RFID) sin complejidad biométrica, desproporcionada para un espacio con menos de cinco personas autorizadas.
- Cumplimos con los requisitos de trazabilidad de accesos que exige el Esquema Nacional de Seguridad en su categoría básica.

---

### Videovigilancia

Desplegar un sistema CCTV completo con NVR dedicado, switch PoE y varias cámaras 360º habría ido contra la filosofía del proyecto. Hemos buscado una solución que aproveche recursos ya existentes y que no añada nuevos servidores físicos.

**Solución implantada**

- **Una cámara IP bullet** (modelo Ubiquiti UniFi G4 Bullet o equivalente) montada en la pared o en el techo, con encuadre directo a la parte frontal de los dos racks.
- **Grabación sobre el NAS corporativo QNAP TS-453D**, que ya disponemos para backups. Utilizamos el software Surveillance Station de QNAP, con licencia para una cámara incluida.
- **Señalización de zona videovigilada** mediante adhesivo normalizado en la puerta de acceso a la sala.

**Por qué lo hemos hecho así**

- Una sola cámara cubre todo el plano de trabajo. No necesitamos visión nocturna porque la sala dispone de iluminación de emergencia.
- Aprovechamos el NAS existente como grabador, eliminando la necesidad de un NVR adicional y su consumo eléctrico (habríamos añadido unos 35-40 W extra).
- La gestión de la cámara se integra en la misma interfaz de administración del NAS, unificando la monitorización del entorno.

---

### Prevención, detección y extinción de incendios

Este ha sido el punto donde más hemos ajustado la escala. Los sistemas de aspiración de aire (tipo VESDA) y las instalaciones automáticas de gas Novec 1230 por inundación total de sala están pensados para CPDs de varios racks y superficies superiores a 20-30 m². Nuestro escenario no lo justifica ni técnica ni económicamente.

**Solución implantada: prevención**

- **Cableado certificado LSZH** (Low Smoke Zero Halogen) en todas las conexiones internas y latiguillos del rack.
- **Sensor ambiental de temperatura y humedad** APC AP9335TH (o compatible) instalado en la parte superior del rack, conectado al SAI. Si la temperatura supera los 35 °C, el SAI genera un evento SNMP que recoge el sistema de monitorización.
- Separación natural entre el rack de networking (calor moderado) y el rack de gestión, con ventilación forzada propia en cada armario.

**Solución implantada: detección**

- **Detector de humo óptico autónomo** instalado en el interior del rack, con salida de contacto seco cableada a la entrada de contactos del SAI. Cualquier alarma de humo dispara una notificación inmediata al equipo de operaciones.
- El mismo sensor de temperatura nos sirve como verificación térmica ante una posible alarma de humo (doble comprobación antes de actuar).

**Solución implantada: extinción**

- **Dispositivo de extinción automática por aerosol condensado** dentro de cada rack. Hemos seleccionado unidades compactas tipo FirePro o STAT-X, con activación por fusible térmico a 79 °C. No llevan electrónica, no caducan en 10 años y no dañan los equipos ni dejan residuos conductores.
- **Extintor manual de CO₂ de 2 kg** fijado en la pared junto a la entrada del habitáculo, para uso del personal en caso de conato.

**Por qué lo hemos hecho así**

- Los dispositivos de aerosol condensado son la solución más limpia y escalable para racks individuales. No necesitan tuberías, ni depósitos presurizados, ni mantenimiento anual obligatorio.
- La integración de los sensores con el SAI nos permite recibir alarmas sin desplegar un sistema de detección de incendios independiente.
- Todo el conjunto añade menos de 200 € al presupuesto del rack y proporciona una protección real contra el fuego sin falsos techos ni obras.

---

### Vías de evacuación e iluminación de emergencia

La responsabilidad sobre las vías de evacuación corresponde al plan de autoprotección del edificio, no al proyecto del CPD. Nuestra intervención se ha limitado a dos verificaciones y una mejora menor:

- **Verificación de ubicación**: hemos comprobado que los dos racks no interfieren con puertas de emergencia ni reducen la anchura de los pasillos por debajo de 1 metro, tal como exige el CTE DB-SI.
- **Iluminación de emergencia**: la sala ya dispone de luminaria LED autónoma con batería recargable. Si no hubiera existido, habríamos añadido una unidad de superficie con autonomía de 1 hora.
- **Simulacros**: incluimos en el plan de operaciones un procedimiento de corte de suministro y verificación de apagado controlado, coordinado con el responsable de PRL del edificio.

---

### Integración con el sistema de monitorización

Todas las señales de seguridad física convergen en nuestro sistema de monitorización local:

- Los contactos de alarma del sensor de humo y del sensor de temperatura se cablean a los puertos de entrada digital del SAI APC.
- El SAI envía traps SNMP a la estación de monitorización (Intel NUC con Zabbix/Grafana, definida en la sección de infraestructura).
- La cámara IP es accesible desde el panel de administración del NAS QNAP, donde también se configuran las alertas por detección de movimiento.

Con esto logramos un **cuadro de seguridad física unificado**, sin consolas adicionales ni software propietario más allá de lo que ya tenemos en operación.

---

### Resumen de soluciones adoptadas

| Ámbito | Solución | Consumo extra | Mantenimiento requerido |
|--------|----------|:-------------:|-------------------------|
| Control de acceso | Cerradura electrónica RFID en rack | 0 W (batería o PoE) | Revisión anual de logs |
| Videovigilancia | 1 cámara IP → grabación en NAS | PoE desde switch | Revisión trimestral de almacenamiento |
| Detección incendios | Sensor óptico de humo + sensor de temperatura en rack | 0 W (contactos secos) | Prueba funcional semestral |
| Extinción incendios | Aerosol condensado (por rack) + extintor CO₂ | 0 W | Inspección visual anual; sustitución a los 10 años |
| Iluminación emergencia | Luminaria LED autónoma existente | 0 W | Prueba mensual de autonomía |

Todas las decisiones que hemos tomado en seguridad física responden a la misma premisa que guió la migración a AWS: **mantener lo necesario, eliminar lo superfluo y asegurar que cada elemento tenga una razón de peso para estar ahí**. El resultado es un CPD ligero, protegido y perfectamente auditado.


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
| innovatetech-media | 20 GiB | Àudio/Vídeo/Jitsi |

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
sudo ss -tlnp | grep 8080
curl -I http://localhost:8080
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

**Streaming en directo — Icecast2:**
```bash
sudo apt install -y icecast2
```

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

**Verificación:**
```bash
curl -I http://localhost:8080/videos/video1.mp4
```

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

> ⚠️ **Lección aprendida:** la versión JVB 2.3-291 lee la configuración XMPP únicamente de `sip-communicator.properties`, con claves en **MAYÚSCULAS**. Sin el mapeado NAT correcto, ICE no puede negociar los candidatos de media.

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

> <img width="1832" height="1304" alt="image" src="https://github.com/user-attachments/assets/c8f4e5f1-8b2d-4add-8716-a14f1eee41e4" />
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

> <img width="469" height="555" alt="image" src="https://github.com/user-attachments/assets/cd730bee-f70f-4222-962c-ae1915fba028" />
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
> `systemctl status mariadb` mostrant el servei actiu.

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

> <img width="736" height="536" alt="image" src="https://github.com/user-attachments/assets/9061216f-bb98-4c89-82e2-6296b28ceae2" />
 Execució de `create_users.sh` creant un usuari amb rol `admin`.
> <img width="733" height="358" alt="image" src="https://github.com/user-attachments/assets/3384f1fb-8fb3-4276-96e3-08edea3fdaee" />
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

> 📸 **CAPTURA:** `SELECT Host, User, is_role FROM mysql.user WHERE is_role='Y'` mostrant els 4 rols.
> 📸 **CAPTURA:** `SHOW GRANTS FOR 'vendes'` i `SHOW GRANTS FOR 'administracio'`.

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

> 📸 **CAPTURA:** `SHOW EVENTS FROM innovatetech` mostrant `evt_backup_diari` amb estat `ENABLED`.
> 📸 **CAPTURA:** `SELECT * FROM Taula_Avisos` mostrant registres reals de triggers disparats.

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

**Prova 1 — Instància 2 → Instància 1:**
```bash
iperf3 -c 172.31.17.184 -p 5201 -t 10        # Download
iperf3 -c 172.31.17.184 -p 5201 -t 10 -R     # Upload
```

**Prova 2 — Instància 1 → Instància 2:**
```bash
iperf3 -c 172.31.36.193 -p 5201 -t 10
iperf3 -c 172.31.36.193 -p 5201 -t 10 -R
```

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

## 8. Digitalización y Sostenibilidad

*(Contenido aquí...)*

[⬆ Volver al índice](#tabla-de-contenidos)

---

## 9. Conclusiones

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

## 10. Anexos y Entregables

*(Contenido aquí...)*

[⬆ Volver al índice](#tabla-de-contenidos)

---

*Documentació del Projecte Transversal ASIXc1 — InnovateTech*
*Curs 25/26 · Institut Tecnològic de Barcelona*
