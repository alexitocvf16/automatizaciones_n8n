# ⚙️ Automatizaciones n8n - Catálogo de Plantillas

Bienvenido a mi repositorio principal de flujos y plantillas para **n8n**. 

Como administrador de sistemas (ASIR) y técnico de ciberseguridad, utilizo este espacio para documentar y compartir automatizaciones diseñadas para entornos de producción. El enfoque principal de estos flujos es la eficiencia del procesamiento, la arquitectura segura y la estabilidad en servidores privados (VPS).

## 🗂️ Catálogo de Automatizaciones

Este repositorio se irá actualizando periódicamente con nuevos flujos orientados a la administración IT, ciberseguridad, despliegues y operaciones web. 

## 🔐 Seguridad y Sanitización (Privacy by Design)

Todas las plantillas `.json` publicadas en este repositorio han sido previamente auditadas y sanitizadas para garantizar un uso público seguro:
* No contienen contraseñas, tokens de API reales ni claves privadas.
* Los identificadores internos de infraestructura (como `instanceId` o `webhookId`) no aparecerán.
* Los datos personales y direcciones de correo de origen se sustituirán por variables genéricas (ej. `tu_correo@ejemplo.com`).

Al importar cualquiera de estos flujos en tu propio entorno n8n, el sistema generará automáticamente los identificadores locales válidos para tu máquina y te solicitará conectar o crear tus propias credenciales de forma segura.

## 🚀 Cómo utilizar estas plantillas

1. Navega hasta la carpeta de la automatización que te interese dentro de este repositorio.
2. Copia el texto completo del archivo `.json` o descarga el fichero.
3. En tu propia instancia de n8n, abre un flujo en blanco y pega el código directamente sobre el lienzo (o utiliza la opción de *Import from file*).
4. Revisa el archivo `README.md` específico de cada automatización para ajustar las variables locales y asociar tus cuentas.

---
