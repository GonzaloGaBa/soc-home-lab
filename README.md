# SOC Home Lab - Defensive Security & Tier 1/2 Analysis

Laboratorio práctico de ciberseguridad defensiva diseñado para simular flujos de trabajo de un Analista SOC Tier 1/2, abarcando recolección de telemetría, investigación de incidentes, ingeniería de detección y documentación técnica.

---

## 📌 Fase 1: Diseño y Arquitectura del Laboratorio

La arquitectura está optimizada para ejecutarse en entornos con recursos limitados (8 GB de RAM), utilizando una estructura híbrida basada en telemetría de endpoints y análisis estructurado:

    +-----------------------------------------------------------------------+
    |                         DELLGON (Host OS)                             |
    |                    Windows 11 Home (8 GB RAM)                         |
    |                                                                       |
    |  +---------------------------+     +-------------------------------+  |
    |  |    Fuentes de Telemetría  |     |     Generación de Eventos     |  |
    |  | - Windows Event Logs      |     | - PowerShell Attack Scripts   |  |
    |  | - Sysmon (SwiftOnSecurity)| --> | - Atomic Red Team (Atómicos)  |  |
    |  +---------------------------+     +-------------------------------+  |
    |               |                                                       |
    |               v                                                       |
    |  +-----------------------------------------------------------------+  |
    |  |                  Recolector y Procesador                        |  |
    |  | - Agente de Recopilación (Wazuh Agent / Log Parser / Python)    |  |
    |  +-----------------------------------------------------------------+  |
    |               |                                                       |
    +---------------|-------------------------------------------------------+
                    |
                    v (Telemetría / Alertas)
    +-----------------------------------------------------------------------+
    |                     Análisis & Threat Hunting                         |
    |  - Consola SIEM / Indexador (Wazuh Cloud / Elastic / SIEM Local)      |
    |  - Asistencia con Inteligencia Artificial (Analista Táctico)          |
    |  - Reglas de Detección en formato SIGMA / Mapeo a MITRE ATT&CK        |
    +-----------------------------------------------------------------------+

---

## 📌 Fase 2: Configuración de Telemetría (Sysmon)

Implementación y despliegue de **Microsoft Sysmon64** utilizando la plantilla de configuración optimizada de [SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config).

* **Eventos clave monitoreados:**
  * `Event ID 1` (Process Create - Creación de procesos)
  * `Event ID 3` (Network Connection - Conexiones de red)
  * `Event ID 11` (File Create - Creación de archivos)
  * `Event ID 22` (DNS Query - Consultas DNS)
* **Evidencia de Ingesta en Tiempo Real:**
  ![Sysmon Event Viewer](docs/fase2-sysmon-visor-eventos.png)

  ---

## 📌 Fase 3: Simulación de Ataques y Telemetría

Ejecución controlada de actividades de reconocimiento (*Discovery*) utilizando comandos nativos de Windows (*LOLBins*) para validar la captura de eventos individuales en Sysmon sin comprometer la seguridad del equipo host.

* **Técnica simulada (MITRE ATT&CK):** 
  * `T1087` (Account Discovery) y `T1057` (Process Discovery).

### 1. Detección de Identidad y Cuentas (`whoami /all`)
* **Comando:** `whoami /all`
* **Evidencia Sysmon (Event ID 1):**
  ![Simulación Whoami](docs/fase3-simulacion-reconocimiento-whoami.png)

### 2. Detección de Cuentas de Usuario Locales (`net user`)
* **Comando:** `net user`
* **Evidencia Sysmon (Event ID 1):**
  ![Simulación Net User](docs/fase3-simulacion-reconocimiento-netuser.png)

### 3. Descubrimiento de Procesos Activos (`tasklist`)
* **Comando:** `tasklist`
* **Evidencia Sysmon (Event ID 1):**
  ![Simulación Tasklist](docs/fase3-simulacion-reconocimiento-taklist.png)
