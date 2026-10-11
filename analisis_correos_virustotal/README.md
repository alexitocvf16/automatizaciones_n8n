# 🛡️ Automatización de Análisis de Ciberseguridad con n8n y VirusTotal

Esta plantilla de n8n automatiza el análisis de seguridad de correos electrónicos entrantes. Monitoriza una bandeja de entrada, extrae tanto los archivos adjuntos como las URLs ocultas en el cuerpo del mensaje, los escanea utilizando la API v3 de VirusTotal, y envía un reporte consolidado al administrador.

Está diseñada con una **arquitectura de procesamiento en paralelo** para evitar condiciones de carrera (*race conditions*) y bucles infinitos en el servidor.

## 🚀 ¿Qué hace exactamente este flujo? (Paso a paso)

1. **Trigger de Gmail (Monitorización):** Revisa la bandeja de entrada cada minuto en busca de correos nuevos no leídos. Ignora automáticamente los propios reportes generados por el sistema para evitar bucles.
2. **Bifurcación en Paralelo (Control de estado):**
   - **Rama Rápida:** Marca el correo original como leído en menos de 1 segundo. Esto purga la cola y evita que el trigger vuelva a atrapar el mismo correo si el análisis de VirusTotal tarda varios minutos.
   - **Rama de Procesamiento:** Conserva el paquete de datos original (JSON y binarios) intacto para su análisis posterior.
3. **Filtro de Destinatario:** Asegura que solo se procesen los correos dirigidos a una cuenta específica (protegiendo el resto de alias o bandejas compartidas).
4. **Análisis de Adjuntos:** Si el correo tiene archivos, los envía a la API de VirusTotal, espera el tiempo de procesamiento necesario y recupera el conteo de detecciones maliciosas.
5. **Extracción y Análisis de URLs:** Mediante un script personalizado en JavaScript (Regex), rastrea recursivamente el cuerpo del correo buscando enlaces (`https://...`), los compila, elimina duplicados y los envía a VirusTotal para su análisis independiente.
6. **Consolidación y Reporte:** Une los resultados del análisis de archivos y de URLs en un único paquete de datos y envía un informe detallado por correo electrónico.

## ⚙️ Instalación y Configuración

Dado que esta plantilla ha sido sanitizada por motivos de seguridad, deberás asignar tus propias credenciales y variables tras importarla a tu entorno de n8n.

### 1. Importar la plantilla
Copia el contenido del archivo `.json` de este repositorio y pégalo directamente sobre el lienzo de tu instancia de n8n, o usa la opción de *Import from file* dentro de la interfaz.

### 2. Configurar Credenciales
Al importar el flujo, n8n te pedirá que conectes tus propias cuentas. Necesitarás:
* **Gmail OAuth2:** Para leer y enviar correos.
* **VirusTotal API:** Tu clave de la API (gratuita o de pago) obtenida en virustotal.com.

### 3. Ajustar Variables Locales
Dentro de los nodos, debes reemplazar los correos genéricos por tus direcciones reales:

* **En el nodo `If (Destinatario)`:**
  Cambia `tu_correo@ejemplo.com` por la dirección de correo que deseas monitorizar. Esto asegura que los análisis solo se apliquen a los correos dirigidos específicamente a esa cuenta.

* **En el nodo `Send a message`:**
  Cambia `correo_destinatario_reporte@ejemplo.com` (en el campo *Send To*) por el buzón donde quieres recibir las alertas de seguridad finales.

> **Nota sobre los IDs internos:** En el archivo JSON original, variables de entorno como los `webhookId`, `instanceId` o identificadores de credenciales han sido reemplazados por valores nulos o texto genérico (ej. `"tu_webhook_id"`). No necesitas modificar esto a mano. Cuando importes y actives el flujo en tu servidor, n8n generará automáticamente los identificadores únicos válidos para tu máquina local.

## 🛠️ Requisitos del Sistema
- n8n (versión 1.0+ recomendada).
- Cuenta de Google Cloud (para generar las credenciales OAuth2 de la API de Gmail).
- Cuenta en VirusTotal (API Key).


<img width="1656" height="754" alt="Captura de pantalla 2026-10-11 113821" src="https://github.com/user-attachments/assets/4144f37f-0165-4f2c-9cb0-fa36548a4785" />

