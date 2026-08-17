



# AUD2700 v3

Herramienta multiplataforma para **recopilación de evidencias y auditoría técnica basada en ISO/IEC 27001:2022**, desarrollada para ejecutarse principalmente en **Kali Linux** y **Windows**.

El programa recopila información técnica del sistema auditado, genera evidencias estructuradas, crea informes asociados a controles ISO/IEC 27001:2022 y permite trabajar tanto sobre el equipo local como sobre equipos remotos mediante **SSH (Linux)** o **WinRM (Windows)**.

---

## 1. ¿QUÉ HACE?

La herramienta automatiza tareas de recopilación de evidencias utilizadas durante una auditoría técnica de seguridad.

Entre sus principales funciones:

* Recopilación de logs del sistema.
* Recopilación de registros de autenticación.
* Análisis de usuarios y grupos.
* Revisión de configuración de red.
* Revisión de firewall.
* Revisión del framework de auditoría.
* Análisis de procesos y servicios.
* Revisión del sistema de archivos.
* Recopilación de información de seguridad de Windows.
* Generación automática de evidencias.
* Generación de informes en formato JSON y TXT.
* Cálculo de hash SHA-256 para integridad de informes.
* Generación de prompts preparados para análisis mediante IA.
* Exportación de resultados a PDF.
* Auditoría local.
* Auditoría remota de sistemas Linux mediante SSH.
* Auditoría remota de sistemas Windows mediante WinRM.
* Comprobación automática de herramientas necesarias.

La aplicación identifica la auditoría como **ISO/IEC 27001:2022** y relaciona las pruebas con controles concretos.

---

# 2. REQUISITOS

## 2.1. Kali Linux / Linux

Se recomienda:

* Kali Linux actualizado.
* Python 3.
* Acceso `sudo`.
* Conexión a Internet durante la instalación de dependencias.
* Permisos suficientes para acceder a logs y configuraciones del sistema.
3. LINUX AUDIT FRAMEWORK — MUY RECOMENDADO

Para realizar auditorías sobre sistemas Linux, es muy recomendable instalar y activar Linux Audit Framework (auditd) antes de comenzar la auditoría.

AUD2700 dispone de un módulo específico para comprobar el framework de auditoría y recopilar evidencias relacionadas con:

Estado del servicio de auditoría.
Configuración de auditd.
Reglas de auditoría.
Registros de auditoría.
Eventos de autenticación.
ausearch.
aureport.
Directorio /var/log/audit/.

La herramienta comprueba específicamente la disponibilidad de ausearch y la existencia del directorio /var/log/audit, y genera un informe asociado a los controles A.12.4.1 y A.12.4.2 de ISO/IEC 27001:2022 y al control 8.15 de ISO/IEC 27002:2022.

¿Cuándo instalarlo?
Antes de realizar la auditoría

Lo recomendable es instalarlo antes de comenzar la recopilación de evidencias.

El flujo recomendado sería:

Instalar auditd
      ↓
Activar auditd
      ↓
Comprobar que está funcionando
      ↓
Dejarlo recopilar eventos
      ↓
Ejecutar AUD2700
      ↓
Recopilar evidencias
      ↓
Generar informes
      ↓
Analizar resultados

Esto es especialmente importante si se pretende analizar actividad histórica del sistema.

Importante: si instalamos y activamos auditd justo antes de ejecutar AUD2700, no dispondremos de un histórico amplio de eventos anteriores a su instalación/configuración. Por ello, en una auditoría real es preferible que el sistema ya tenga auditd funcionando y generando registros.

3.1. INSTALAR AUDITD EN KALI LINUX

En Kali Linux:

sudo apt update

Instalar:

sudo apt install auditd audispd-plugins -y

Comprobar la instalación:

auditctl -v

También:

ausearch --version

Y:

aureport --version
3.2. ACTIVAR EL SERVICIO

Iniciar auditd:

sudo systemctl start auditd

Comprobar el estado:

sudo systemctl status auditd

Debe aparecer como:

active (running)

Para que se inicie automáticamente con el sistema:

sudo systemctl enable auditd

Comprobar:

sudo systemctl is-enabled auditd

Debería devolver:

enabled
3.3. COMPROBAR QUE AUDITD ESTÁ FUNCIONANDO

Ejecutar:

sudo auditctl -s

Este comando permite comprobar el estado actual del sistema de auditoría.

También:

sudo auditctl -l

para consultar las reglas de auditoría actualmente cargadas.

Comprobar los registros:

sudo ls -lah /var/log/audit/

Debería existir el directorio:

/var/log/audit/

y, si ya se han generado eventos, archivos como:

audit.log
3.4. REALIZAR UNA PRUEBA

Antes de ejecutar AUD2700 es recomendable comprobar que el sistema está registrando eventos.

Por ejemplo:

sudo ausearch -m USER_LOGIN

También:

sudo aureport

Si existen registros, ausearch y aureport deberían devolver información.

AUD2700 utiliza precisamente estas herramientas durante la recopilación de evidencias del framework de auditoría.

3.5. EJECUTAR AUD2700 DESPUÉS

Una vez instalado y funcionando auditd:

cd ~/AUD2700
sudo python3 aud2700_v3.py.bak

En el menú:

[1] Configurar datos de auditoria
[2] Recopilar TODAS las evidencias
...
[7] Framework de Auditoria

Para una auditoría completa:

1 → Configurar auditoría
        ↓
2 → Recopilar TODAS las evidencias
        ↓
7 → Framework de Auditoría
        ↓
A → Análisis mediante IA
        ↓
P → Exportación PDF

La opción 7 es especialmente relevante cuando auditd está instalado y funcionando.

3.6. ¿POR QUÉ ES IMPORTANTE?

auditd proporciona una capa adicional de trazabilidad sobre el sistema Linux.

Permite disponer de evidencias relacionadas con:

Autenticaciones.
Accesos.
Eventos de seguridad.
Ejecución de determinadas acciones.
Cambios relevantes.
Actividad administrativa.
Eventos registrados por las reglas de auditoría.

Esto mejora considerablemente la capacidad de AUD2700 para generar evidencias relacionadas con el registro y monitorización de eventos de seguridad.

Por este motivo:

RECOMENDACIÓN: en sistemas Linux que vayan a ser auditados con AUD2700, instalar y mantener auditd activo antes de comenzar la auditoría.
El programa utiliza herramientas estándar de Linux como:

* `journalctl`
* `last`
* `lastlog`
* `dmesg`
* `ss`
* `netstat`
* `lsof`
* `ps`
* `auditctl`
* `aureport`
* `ausearch`
* `iptables`
* `ufw`
* `systemctl`
* `crontab`
* `find`
* `stat`
* `getent`

La aplicación comprueba qué herramientas están disponibles y puede intentar instalar las que falten.

---

## 2.2. Windows

Se recomienda:

* Windows 10/11 o Windows Server.
* Python 3.
* Ejecutar el programa como Administrador.
* PowerShell disponible.

La aplicación utiliza herramientas de Windows como:

* `wevtutil`
* PowerShell
* `netsh`
* `tasklist`
* `sc`
* `reg`
* `systeminfo`

---

# 3. DEPENDENCIAS PYTHON

Para auditorías remotas necesita:

### SSH para Linux

```bash
pip install paramiko
```

### WinRM para Windows

```bash
pip install pywinrm
```

### Exportación PDF

La aplicación dispone de dos mecanismos para generar PDF:

1. WeasyPrint.
2. ReportLab como alternativa.

Instalación recomendada:

```bash
pip install paramiko pywinrm weasyprint reportlab
```

En Kali Linux puede ser necesario utilizar:

```bash
pip install paramiko pywinrm weasyprint reportlab --break-system-packages
```

---

# 4. INSTALACIÓN EN KALI LINUX

Crear el directorio del programa:

```bash
mkdir -p ~/AUD2700
cd ~/AUD2700
```

Copiar dentro el programa:

```text
aud2700_v3.py.bak
```

La estructura mínima será:

```text
AUD2700/
└── aud2700_v3.py.bak
```

---

# 5. COMPROBAR PYTHON

```bash
python3 --version
```

Debe aparecer una versión de Python 3.

---

# 6. INSTALAR DEPENDENCIAS

```bash
python3 -m pip install paramiko pywinrm weasyprint reportlab --break-system-packages
```

---

# 7. DAR PERMISOS DE EJECUCIÓN

Desde el directorio del programa:

```bash
chmod +x aud2700_v3.py.bak
```

Comprobar:

```bash
ls -l aud2700_v3.py.bak
```

---

# 8. EJECUTAR EN KALI LINUX

La forma recomendada es:

```bash
sudo python3 aud2700_v3.py.bak
```

También puede ejecutarse directamente si el archivo tiene permisos de ejecución:

```bash
sudo ./aud2700_v3.py.bak
```

---

# 9. EJECUTAR EN WINDOWS

Abrir **PowerShell como Administrador**.

Entrar en la carpeta:

```powershell
cd C:\AUD2700
```

Ejecutar:

```powershell
python aud2700_v3.py.bak
```

También:

```powershell
py aud2700_v3.py.bak
```

---

# 10. MENÚ PRINCIPAL

Al iniciar la aplicación aparecerá el menú principal:

```text
[1] Configurar datos de auditoria
[2] Recopilar TODAS las evidencias
[3] Logs de Sistema / Eventos
[4] Logs de Autenticacion / Seguridad
[5] Usuarios y Grupos
[6] Red y Firewall
[7] Framework de Auditoria
[8] Procesos y Servicios
[9] Sistema de Archivos / Software
[A] Analizar informes con IA (genera prompt)
[P] Exportar informes a PDF
[S] Salir
```

---

# 11. FLUJO RECOMENDADO

Para realizar una auditoría completa:

```text
1 → Configurar auditoría
        ↓
2 → Recopilar todas las evidencias
        ↓
A → Generar análisis para IA
        ↓
P → Exportar informe PDF
```

---

# 12. CONFIGURAR LA AUDITORÍA

Seleccionar:

```text
1
```

El programa solicitará información como:

* Organización.
* Alcance.
* Auditor.
* IP objetivo.
* Dominio.
* Contacto.
* Ubicación.
* Norma.
* Directorio de resultados.

También permite seleccionar:

```text
[L] Local
[R] Remoto
```

La auditoría genera automáticamente una estructura de directorios similar a:

```text
ISO27001_Audit_EMPRESA_FECHA/
├── 00_METADATA.json
├── evidencias/
├── informes/
└── export/
```

---

# 13. AUDITORÍA LOCAL

En modo local, el programa analiza el equipo donde se está ejecutando.

Ejemplo:

```text
AUD2700
   │
   ├── Logs
   ├── Autenticación
   ├── Usuarios
   ├── Red
   ├── Firewall
   ├── Auditoría
   ├── Procesos
   ├── Servicios
   └── Sistema de archivos
```

Para realizar todas las comprobaciones:

```text
2
```

---

# 14. AUDITORÍA REMOTA

La herramienta permite trabajar sobre equipos remotos.

## Linux mediante SSH

Configuración predeterminada:

```text
Protocolo: SSH
Puerto: 22
```

Se solicitarán:

```text
SO del objetivo: linux
IP/host:
Puerto:
Usuario administrador:
Contraseña:
```

La contraseña se mantiene en memoria durante la sesión y no se guarda deliberadamente en disco.

---

## Windows mediante WinRM

Configuración predeterminada:

```text
Protocolo: WinRM
Puerto: 5985
```

Se solicitarán:

```text
SO del objetivo: windows
IP/host:
Puerto:
Usuario administrador:
Contraseña:
```

En el equipo Windows puede ser necesario habilitar WinRM:

```powershell
winrm quickconfig -q
```

---

# 15. EVIDENCIAS GENERADAS

La aplicación organiza las evidencias por categorías:

```text
ISO27001_Audit_EMPRESA_FECHA/
│
├── 00_METADATA.json
│
├── evidencias/
│   ├── 01_sistema/
│   ├── 02_autenticacion/
│   ├── 03_usuarios/
│   ├── 04_red/
│   ├── 05_auditoria/
│   ├── 06_procesos/
│   └── 07_filesystem/
│
├── informes/
│   ├── ISO27001-SYS-001.json
│   ├── ISO27001-SYS-001.txt
│   ├── ISO27001-AUTH-002.json
│   └── ...
│
└── export/
```

Los informes incluyen:

* Control ISO.
* Título de la prueba.
* Descripción.
* Hallazgos.
* Evidencias.
* Datos técnicos.
* Nivel de cumplimiento.
* Severidad.
* Recomendaciones.
* Referencias.
* Hash SHA-256.

---

# 16. ANÁLISIS CON IA

Seleccionar:

```text
A
```

La aplicación genera:

```text
export/PROMPT_ANALISIS_IA.txt
```

El prompt está preparado para analizar los informes recopilados mediante una herramienta de IA.

El análisis solicitado incluye:

1. Resumen ejecutivo.
2. Mapa de riesgos por control ISO 27001.
3. No conformidades con severidad.
4. Recomendaciones prioritarias y roadmap.
5. Puntuación de madurez de 1 a 5 por dominio.

### Importante

La versión actual del programa **no realiza una conexión directa con una API de IA**.

Genera el prompt con los resultados de la auditoría para poder copiarlo posteriormente a ChatGPT, Claude u otra herramienta de IA.

---

# 17. EXPORTACIÓN A PDF

Seleccionar:

```text
P
```

La aplicación intenta generar:

```text
export/INFORME_AUDITORIA_ISO27001.pdf
```

El proceso utiliza:

1. WeasyPrint.
2. ReportLab como alternativa.

Si no encuentra ninguna librería PDF compatible, genera:

```text
export/INFORME_AUDITORIA_ISO27001.html
```

El HTML puede abrirse en un navegador e imprimirse posteriormente como PDF.

---

# 18. HASH DE INTEGRIDAD

Los informes JSON incorporan un hash:

```text
SHA-256
```

Esto permite comprobar si un informe ha sido modificado posteriormente.

---

# 19. CREAR LANZADOR EN EL ESCRITORIO DE KALI

Para evitar tener que escribir cada vez:

```bash
sudo python3 aud2700_v3.py.bak
```

se puede crear un lanzador.

Suponiendo que el programa se encuentra en:

```text
/home/kali/AUD2700/
```

crear:

```bash
nano ~/Desktop/AUD2700.desktop
```

Introducir:

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=AUD2700 - Auditoría ISO 27001
Comment=Auditoría ISO/IEC 27001:2022
Exec=xfce4-terminal --working-directory=/home/kali/AUD2700 -e "sudo python3 /home/kali/AUD2700/aud2700_v3.py.bak"
Icon=utilities-terminal
Terminal=false
Categories=Security;System;
```

Guardar:

```text
CTRL+O
ENTER
CTRL+X
```

Dar permisos:

```bash
chmod +x ~/Desktop/AUD2700.desktop
```

Si Kali muestra una advertencia de seguridad, seleccionar:

```text
Permitir lanzamiento
```

o:

```text
Trust and Launch
```

según la versión del escritorio.

---

# 20. LANZADOR MEDIANTE SCRIPT

También se puede crear un script de inicio.

Crear:

```bash
nano ~/AUD2700/arrancar.sh
```

Contenido:

```bash
#!/bin/bash

cd "$(dirname "$0")"

sudo python3 aud2700_v3.py.bak
```

Dar permisos:

```bash
chmod +x ~/AUD2700/arrancar.sh
```

El lanzador del escritorio puede utilizar entonces:

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=AUD2700 - Auditoría ISO 27001
Comment=Auditoría ISO/IEC 27001:2022
Exec=xfce4-terminal --working-directory=/home/kali/AUD2700 -e "/home/kali/AUD2700/arrancar.sh"
Icon=utilities-terminal
Terminal=false
Categories=Security;System;
```

---

# 21. LANZADOR EN EL MENÚ DE KALI

Crear el directorio:

```bash
mkdir -p ~/.local/share/applications
```

Crear:

```bash
nano ~/.local/share/applications/aud2700.desktop
```

Contenido:

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=AUD2700 - Auditoría ISO 27001
Comment=Auditoría ISO/IEC 27001:2022
Exec=xfce4-terminal --working-directory=/home/kali/AUD2700 -e "/home/kali/AUD2700/arrancar.sh"
Icon=utilities-terminal
Terminal=false
Categories=Security;System;
```

Dar permisos:

```bash
chmod +x ~/.local/share/applications/aud2700.desktop
```

La aplicación aparecerá en el menú de aplicaciones de Kali.

---

# 22. ESTRUCTURA RECOMENDADA

La estructura final recomendada es:

```text
AUD2700/
│
├── aud2700_v3.py.bak
├── arrancar.sh
├── README.md
│
└── auditorias/
```

Las auditorías pueden almacenarse dentro de:

```text
auditorias/
```

Ejemplo:

```text
AUD2700/
│
├── aud2700_v3.py.bak
├── arrancar.sh
├── README.md
│
└── auditorias/
    │
    ├── ISO27001_Audit_EMPRESA1_20260815_100000/
    │   ├── 00_METADATA.json
    │   ├── evidencias/
    │   ├── informes/
    │   └── export/
    │
    └── ISO27001_Audit_EMPRESA2_20260815_140000/
        ├── 00_METADATA.json
        ├── evidencias/
        ├── informes/
        └── export/
```

---

# 23. USO RÁPIDO

### Kali Linux

```bash
cd ~/AUD2700
sudo python3 aud2700_v3.py.bak
```

### Windows

```powershell
cd C:\AUD2700
python aud2700_v3.py.bak
```

---

# 24. ATAJOS

Durante la ejecución:

```text
CTRL + Z
```

permite volver al menú principal en los puntos en los que el programa captura esta señal.

Para salir:

```text
S
```

---

# 25. USO RESPONSABLE

La herramienta debe utilizarse únicamente sobre sistemas propios o sobre sistemas para los que se disponga de autorización expresa para realizar la auditoría.

La información recopilada puede contener datos sensibles, incluyendo:

* Logs.
* Usuarios.
* Configuración de seguridad.
* Configuración de red.
* Procesos.
* Servicios.
* Información del sistema.
* Configuración de autenticación.

Los resultados de las auditorías deben almacenarse y protegerse de acuerdo con las políticas de seguridad de la organización.

---

# 26. RESUMEN

| Función                                      | Disponible              |
| -------------------------------------------- | ----------------------- |
| Linux                                        | ✅                       |
| Windows                                      | ✅                       |
| Auditoría local                              | ✅                       |
| Auditoría Linux remota                       | ✅ SSH                   |
| Auditoría Windows remota                     | ✅ WinRM                 |
| Recopilación de evidencias                   | ✅                       |
| Informes JSON                                | ✅                       |
| Informes TXT                                 | ✅                       |
| Hash SHA-256                                 | ✅                       |
| Generación de prompt IA                      | ✅                       |
| Integración directa con API IA               | ❌                       |
| Exportación PDF                              | ✅                       |
| Exportación HTML                             | ✅                       |
| Comprobación de herramientas                 | ✅                       |
| Instalación automática de herramientas Linux | ✅                       |
| Lanzador de escritorio                       | ✅                       |
| Archivo principal                            | **`aud2700_v3.py.bak`** |
| Versión                                      | **3.1**                 |

---

# 27. COMANDO DE INICIO RÁPIDO

```bash
cd ~/AUD2700
sudo python3 aud2700_v3.py.bak
```

**AUD2700 v3 — Auditoría y recopilación de evidencias ISO/IEC 27001:2022**
