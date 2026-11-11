# 📄 Reporte de ejercicio simulado: INC-2026-001

**Fecha/Hora del Incidente:** 2026-10-20 15:24:41 UTC  
**Severidad:** Media (Medium)  
**Estado:** Ejercicio finalizado; sin incidente real  
**Analista Asignado:** Gonzalo Barrientos — autor y analista del ejercicio  

---

## 1. Resumen Ejecutivo
Se documentó un ejercicio autorizado de descubrimiento en un equipo Windows del laboratorio. Se ejecutaron comandos nativos y se revisó la telemetría disponible en Sysmon mediante el Visor de eventos. El objetivo fue practicar el análisis de evidencias y la documentación técnica. Las ejecuciones fueron simuladas y no constituyen evidencia de una intrusión real.

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
* **Regla de Detección Creada:** Se redactó una regla Sigma experimental relacionada con los comandos estudiados. Su conversión, despliegue y validación en un motor de detección quedan pendientes; este reporte no acredita la generación de alertas automáticas.
* **Aislamiento/Contención:** No se requiere aislamiento del host al tratarse de un ejercicio en entorno controlado (*Home Lab*).
* **Acciones Preventivas:** Monitorear cuentas locales y limitar la ejecución de scripts no firmados mediante políticas de restricción de software.
