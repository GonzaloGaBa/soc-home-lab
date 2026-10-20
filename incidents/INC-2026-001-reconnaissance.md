# 📄 Reporte de Incidente: INC-2026-001

**Fecha/Hora del Incidente:** 2026-10-20 15:24:41 UTC  
**Severidad:** Media (Medium)  
**Estado:** Cerrado / Mitigado  
**Analista Asignado:** Gonzalo Barrientos (SOC Analyst Tier 1/2)  

---

## 1. Resumen Ejecutivo
Se detectó la ejecución secuencial de comandos nativos de reconocimiento (*LOLBins*) en el host de un usuario final. El patrón observado sugiere una etapa inicial de descubrimiento de sistema y cuentas por parte de un actor no autorizado o una simulación de ataque. No se evidenció persistencia ni elevación de privilegios en el sistema.

---

## 2. Detalles del Host y Usuario
* **Host Afectado:** `DESKTOP-KI9I8QM`
* **Dirección IP:** `192.168.1.68`
* **Usuario Activo:** `DESKTOP-KI9I8QM\Gonzalo`
* **Sistema Operativo:** Windows 11 Home

---

## 3. Indicadores de Compromiso (IoCs) & Telemetría
A través de la ingesta de **Sysmon (Event ID 1 y Event ID 3)**, se identificaron los siguientes eventos clave:

| Event ID | Proceso Padre | Proceso Ejecutado | Línea de Comandos / Detalles | Mapeo MITRE ATT&CK |
| :--- | :--- | :--- | :--- | :--- |
| **1** | `cmd.exe` | `whoami.exe` | `whoami /all` | T1087 - Account Discovery |
| **1** | `cmd.exe` | `net.exe` | `net user` | T1087 - Account Discovery |
| **1** | `cmd.exe` | `tasklist.exe` | `tasklist` | T1057 - Process Discovery |
| **3** | `cmd.exe` | `nslookup.exe` | Destino: `186.130.128.250:53` (UDP) | T1016 - Network Config Discovery |

---

## 4. Análisis Táctico
1. **Triaje Inicial:** La ejecución de `whoami /all` seguida de `net user` en un intervalo de pocos segundos indica un comportamiento automatizado o manual orientado al mapeo del entorno local.
2. **Identificación de Procesos:** La ejecución de `tasklist` busca identificar herramientas de seguridad activas (AV/EDR) o procesos vulnerables.
3. **Pivoteo a Red:** Se identificó actividad UDP sobre el puerto 53 mediante `nslookup`, lo que confirma consultas de resolución de nombres de dominio salientes.

---

## 5. Medidas de Mitigación y Recomendaciones
* **Regla de Detección Creada:** Se implementó la regla SIGMA `detect-recon-discovery.yml` en la carpeta `detections/` para alertar automáticamente ante futuras ejecuciones de este patrón.
* **Aislamiento/Contención:** No se requiere aislamiento del host al tratarse de un ejercicio en entorno controlado (*Home Lab*).
* **Acciones Preventivas:** Monitorear cuentas locales y limitar la ejecución de scripts no firmados mediante políticas de restricción de software.
