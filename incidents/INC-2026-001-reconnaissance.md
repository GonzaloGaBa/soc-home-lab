# 📄 Reporte de ejercicio simulado: INC-2026-001

**Fecha/hora de referencia del ejercicio:** 2026-10-20 15:24:41 UTC
**Clasificación:** Ejercicio de laboratorio; sin severidad de incidente real asignada.  
**Estado:** Ejercicio finalizado; sin incidente real  
**Analista Asignado:** Gonzalo Barrientos — autor y analista del ejercicio  

---

## 1. Resumen Ejecutivo
Se documentó un ejercicio autorizado de descubrimiento en un equipo Windows del laboratorio. Se ejecutaron comandos nativos y se revisó la telemetría disponible en Sysmon mediante el Visor de eventos. El objetivo fue practicar el análisis de evidencias y la documentación técnica. Las ejecuciones fueron simuladas y no constituyen evidencia de una intrusión real.

---

## 2. Detalles del Host y Usuario
* **Host del laboratorio:** `DESKTOP-KI9I8QM`
* **Dirección IP:** `192.168.1.68`
* **Usuario Activo:** `DESKTOP-KI9I8QM\Gonzalo`
* **Sistema Operativo:** Windows 11 Home

---

## 3. Evidencias y telemetría del ejercicio
Mediante la revisión local de eventos de Sysmon en el Visor de eventos de Windows, se documentaron las siguientes ejecuciones y conexiones:

| Event | Proceso | Ejecución o detalle | Mapeo principal |
| :--- | :--- | :--- | :--- |
| **1** | `whoami.exe` | `whoami /all` |  T1033 — Descubrimiento del propietario/usuario del sistema |
| **1** | `net.exe` | `net user` |  T1087.001 — Descubrimiento de cuentas locales |
| **1** | `tasklist.exe` | `tasklist` | T1057 — Descubrimiento de procesos |
| **3** | `nslookup.exe` | `UDP hacia 186.130.128.250:53` | Sin técnica asignada con esta evidencia |

---

## 4. Análisis de las evidencias

1. **Descubrimiento local:** Las ejecuciones autorizadas de `whoami /all` y `net user` permitieron estudiar la consulta de identidad del usuario y la enumeración de cuentas locales. Estos comandos no implican actividad maliciosa por sí mismos.

2. **Enumeración de procesos:** La ejecución de `tasklist` permitió listar los procesos activos. Por sí sola, no demuestra intención de identificar herramientas de seguridad ni procesos vulnerables.

3. **Actividad de red:** Se observó tráfico UDP al puerto 53 asociado a `nslookup.exe`, compatible con DNS. El Event ID 3 por sí solo no identifica el dominio consultado ni acredita una resolución exitosa.


---

## 5. Medidas de Mitigación y Recomendaciones
* **Regla de Detección Creada:** Se redactó una regla Sigma experimental relacionada con los comandos estudiados. Su conversión, despliegue y validación en un motor de detección quedan pendientes; este reporte no acredita la generación de alertas automáticas.
* **Aislamiento/Contención:** No se requiere aislamiento del host al tratarse de un ejercicio en entorno controlado (*Home Lab*).
* **Recomendaciones para un entorno real:** Contextualizar estas ejecuciones según usuario, proceso padre y actividad habitual del equipo antes de escalar una alerta. Evaluar controles de mínimo privilegio y de aplicaciones según las necesidades del entorno.
