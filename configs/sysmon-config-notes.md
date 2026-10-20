# ⚙️ Configuración de Telemetría - Sysmon

Este laboratorio utiliza la plantilla optimizada de **SwiftOnSecurity** para la captura de eventos clave en el host Windows 11.

## 📌 Repositorio de Origen
* **Configuración Base:** [SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)
* **Archivo Principal:** `sysmonconfig-export.xml`

## 🎯 Event IDs Monitoreados en el Lab
* **Event ID 1:** Creación de Procesos (*Process Creation*).
* **Event ID 3:** Conexión de Red (*Network Connection*).
* **Event ID 11:** Creación de Archivos (*File Create*).
* **Event ID 22:** Consultas DNS (*DNS Query*).

## 🚀 Comando de Despliegue / Actualización
Para aplicar o actualizar la configuración en el host local:
`sysmon64.exe -i sysmonconfig-export.xml`
