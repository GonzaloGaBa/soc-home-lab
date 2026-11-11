# ⚙️ Configuración de Telemetría - Sysmon

Este laboratorio utiliza la plantilla optimizada de **SwiftOnSecurity** para la captura de eventos clave en el host Windows 11.

## 📌 Repositorio de Origen
* **Configuración Base:** [SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)
* **Archivo Principal:** `sysmonconfig-export.xml`

## 🎯 Event IDs Monitoreados en el Lab
* **Event ID 1:** Creación de Procesos (*Process Creation*).
* **Event ID 3:** Conexión de Red (*Network Connection*).
La captura depende de las reglas de inclusión y exclusión de la configuración aplicada. No se presupone el registro de todos los procesos ni de todas las conexiones de red. Los eventos observados se revisan en Microsoft-Windows-Sysmon/Operational, dentro del Visor de eventos.

## 🚀 Comando de Despliegue / Actualización
Para aplicar o actualizar la configuración en el host local:
`sysmon64.exe -i sysmonconfig-export.xml`
