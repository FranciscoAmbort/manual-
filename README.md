# MANUAL DE USUARIO OFICIAL DEL SISTEMA
## Procesador de Deudores y Cuentas Corrientes

---

### FICHA DE CONTROL DEL DOCUMENTO
*(Conforme a estándares IEEE 1074 e ISO/IEC 26514 para Documentación de Software de Usuario)*

| Campo | Detalle |
| :--- | :--- |
| **Título del Sistema** | Chevroford - Procesador de Deudores y Conciliación de Cuentas Corrientes |
| **Organización Destino** | Chevroford Rafaela S.R.L. |
| **Audiencia Objetivo** | Personal Administrativo, Área de Cobranzas y Gerencia General |
| **Versión del Manual** | v1.0 |
| **Fecha de Publicación** | Noviembre 2026 |
| **Estado** | Versión Oficial de Producción |
| **Redacción Técnica** | Equipo de Ingeniería de Software |
| **Validación Funcional** | Administración y Cobranzas Chevroford Rafaela S.R.L. |

---

## 1. PREFACIO E INTRODUCCIÓN

### 1.1 Propósito del Sistema
El **Procesador de Deudores de Chevroford** es una aplicación web interna concebida para modernizar, agilizar y transparentar la gestión de cobranzas de la empresa **Chevroford Rafaela S.R.L.**.

Su función medular radica en procesar las exportaciones masivas de cuentas corrientes emitidas desde **Tango Software**, aplicar un algoritmo riguroso de imputación cronológica de pagos (FIFO reverso), aislar la mora real exigible según el plazo de vencimiento pactado con los clientes y permitir el envío directo de recordatorios personalizados vía **WhatsApp Web** con un solo clic.

### 1.2 Alcance del Sistema
El sistema cubre las siguientes capacidades operativas:
1. **Importación Inteligente:** Carga de planillas Excel de cuentas corrientes sin necesidad de depuración manual previa.
2. **Conciliación Cronológica Automática:** Cálculo del saldo real y determinación de comprobantes impagos legítimos, distinguiendo cancelaciones virtuales de deudas líquidas.
3. **Métricas de Control Financiero (KPIs):** Análisis visual inmediato del total adeudado, cantidad de facturas comprometidas, clientes morosos y extremos de deuda (mayor y menor).
4. **Directorio y Agenda Local:** Base de datos en SQLite para registrar números de teléfono y apodos/alias de trato informal con los clientes.
5. **Gestor de Mensajería y Plantillas:** Redacción de mensajes dinámicos con etiquetas automáticas (`{alias}`, `{total_deuda}`, `{lista_facturas}`, `{datos_bancarios}`) y apertura sincronizada en WhatsApp Web.
6. **Exportación Formal de Informes:** Generación de planillas Excel y reportes ejecutivos en PDF para auditoría y toma de decisiones.

### 1.3 Cómo Navegar este Manual
Este manual está estructurado cronológicamente según la secuencia natural del trabajo diario. Si es la primera vez que utiliza la herramienta, comience por la **Sección 2 (Requisitos)** y la **Sección 3 (Guía Rápida)**. Para casos particulares o inconsistencias en los datos, recurra a la **Sección 6 (Resolución de Problemas y FAQ)**.

---

## 2. REQUISITOS PREVIOS Y ACCESO

### 2.1 Requisitos del Puesto de Trabajo
Para operar fluidamente con el sistema, el puesto de trabajo administrativo solo requiere:
* **Navegador Web Moderno:** Google Chrome, Microsoft Edge, Mozilla Firefox o Brave (versiones actualizadas).
* **Sesión Activa de WhatsApp Web:** Tener vinculada la cuenta de WhatsApp corporativa o del sector de cobranzas en el mismo navegador para el despacho directo de mensajes.
* **Visor de Documentos:** Adobe Acrobat Reader (o visor integrado del navegador) para visualizar los informes en PDF, y Microsoft Excel / LibreOffice Calc para las planillas exportadas.

### 2.2 Puesta en Marcha y Acceso
1. En el equipo servidor o estación de trabajo local, inicie los servicios del sistema (backend en puerto `8000` y frontend en puerto `5173`).
2. Abra su navegador e ingrese a la dirección local:
   ```text
   http://localhost:5173
   ```
3. Verificará de inmediato la interfaz principal con la barra de navegación superior:
   * **Conciliaciones:** Pantalla de carga, estadísticas y tabla interactiva.
   * **Clientes:** Directorio de contactos, teléfonos y alias.
   * **Mensajes:** Configuración de plantillas de texto para WhatsApp.
   * **Ayuda / Manual:** Centro de consulta en línea rápido.

---

## 3. GUÍA RÁPIDA (QUICK START)

Flujo diario resumido en 4 acciones concretas:

```text
+-----------------------+     +------------------------+     +-----------------------+     +------------------------+
| 1. EXPORTAR Y SUBIR   | ==> | 2. SELECCIONAR UMBRAL  | ==> | 3. REVISAR ESTADOS    | ==> | 4. ENVIAR WHATSAPP     |
| Exportar de Tango     |     | Elegir 25, 30 o 60     |     | Inspeccionar KPIs,    |     | Clic en WhatsApp o     |
| (.xlsx) y arrastrarlo |     | dias de mora y pulsar  |     | tabla desplegable y   |     | exportar informe en    |
| a la zona de carga    |     | "Generar Reporte"      |     | filtros de fecha/pago |     | PDF / Excel            |
+-----------------------+     +------------------------+     +-----------------------+     +------------------------+
```

### Mapa de Controles Principales

| Control / Botón | Ubicación | Función Operativa |
| :--- | :--- | :--- |
| **Zona de Carga (Drag & Drop)** | Centro / Pestaña Conciliaciones | Permite arrastrar el archivo de Tango o buscarlo en la computadora. |
| **Filtro de Días (25 / 30 / 60)** | Bajo la zona de carga | Establece la antigüedad mínima de emisión para considerar la factura en mora. |
| **Generar Reporte** | Botón azul principal | Dispara el análisis contable y abre la vista de resultados. |
| **Ayuda / Manual** | Encabezado superior | Abre el manual interactivo flotante para resolver dudas sin salir de la pantalla. |
| **Nuevo Reporte** | Encabezado superior | Limpia el archivo actual para iniciar una nueva carga. |
| **Exportar PDF / Excel** | Costado derecho superior | Descarga el informe consolidado con formato corporativo. |
| **Ícono Flecha (> / v)** | Primera columna de la tabla | Despliega u oculta el detalle de facturas individuales del cliente. |
| **Ícono WhatsApp (Verde)** | Columna "Reclamar" de la tabla | Abre el chat de WhatsApp Web con el reclamo formateado. |
| **Ícono Agendar (Gris)** | Columna "Reclamar" de la tabla | Redirige al alta de contacto cuando el cliente no tiene teléfono guardado. |

---

## 4. PASO A PASO OPERATIVO POR MÓDULO

### Módulo A: Exportación desde Tango Software y Carga de Archivo

#### 1. Exportación desde Tango Software
1. Acceda al módulo de **Ventas / Cuentas Corrientes** en Tango Software.
2. Emita el informe de cuentas corrientes incluyendo detalle de comprobantes.
3. Guarde la exportación en formato de libro de Microsoft Excel (`.xlsx` o `.xls`).

> **REQUISITO ESTRICTO DE COLUMNAS:**  
> Para que el procesador identifique los datos, la planilla debe contener de forma obligatoria las siguientes columnas en su fila de encabezados:
> - `COD_CLI`: Código único del cliente asignado en Tango.
> - `RAZON_SOC`: Nombre fiscal o Razón Social del cliente.
> - `SALDO`: Saldo contable acumulado de la cuenta corriente.
> - `ESTADO`: Estado del comprobante (`PEN` para pendientes, `CAN` para cancelados).
> - `FECHA_EMIS`: Fecha de emisión original del comprobante.
> - `N_COMP`: Número de factura o comprobante (ej: `0001-00012345`).
> - `DEBE`: Importe emitido del comprobante.

#### 2. Carga en el Procesador
1. En la pestaña **Conciliaciones**, ubique el recuadro que indica *"Arrastra un archivo Excel aquí o seleccioná uno"*.
2. Arrastre el archivo exportado desde su explorador de archivos o haga clic sobre *"seleccioná uno"* para buscarlo.
3. Una vez cargado, el sistema mostrará el nombre del archivo y su tamaño en KB, junto a un botón rojo para quitarlo si se equivocó de planilla.

![Captura 1: Pantalla de carga de Excel](./img/01_carga_excel.png)  
*Instrucciones de referencia:*  
*① Zona de soltado y selección de archivo `.xlsx`.*  
*② Selector de umbral de días de mora.*  
*③ Botón "Generar Reporte" para procesar.*

---

### Módulo B: Selección de Umbral de Días e Interpretación de Estadísticas

#### 1. Elección del Plazo de Vencimiento
Debajo del cuadro de carga, seleccione el umbral de tolerancia:
* **25 días:** Ideal para cierres mensuales anticipados y pre-avisos preventivos.
* **30 días:** Plazo estándar de condición de venta mensual.
* **60 días:** Detección de cuentas con mora grave y deudas acumuladas de segundo ciclo.

Presione el botón azul **"Generar Reporte"**. El sistema tardará apenas fracciones de segundo en procesar miles de registros.

#### 2. Comprensión del Criterio Contable FIFO Reverso (Imputación Cronológica)
Una de las mayores fuentes de conflicto en cobranzas es reclamar facturas viejas que el cliente considera pagadas porque hizo entregas parciales no imputadas puntualmente en Tango Software.

El sistema resuelve esto mediante una regla de tres pasos:
1. **Comprobación del Saldo Real:** Si la cuenta corriente en Tango tiene saldo igual o menor a cero (saldo a favor), el sistema **descarta al cliente de plano**. No hay deuda real exigible.
2. **Prioridad a lo Reciente:** El saldo deudor actual se cubre primero con las facturas más recientes hacia atrás.
3. **Absorción de lo Viejo:** Las facturas más antiguas que queden cubiertas por pagos pasados o notas de crédito se consideran virtualmente canceladas, evitando reclamos erróneos. Solo se reportan como exigibles las facturas que efectivamente sostienen el saldo deudor actual y superan el umbral de días configurado.

#### 3. Tarjetas KPI de Resumen Financiero
Al completarse el análisis, se despliegan en pantalla los indicadores consolidados:
* **Total Adeudado:** Monto líquido total de la deuda vencida analizada.
* **Cant. Facturas:** Número absoluto de facturas en situación de mora real.
* **Clientes con Deuda:** Total de cuentas comerciales afectadas.
* **Mayor Deuda:** Cliente con la posición deudora más elevada y su monto correspondiente.
* **Menor Deuda:** Cliente con la deuda mínima registrada.

#### 4. Gráfico de Ranking de Deudores
El gráfico de barras interactivo muestra a los 15 mayores deudores ordenados de mayor a menor. Al posar el puntero del ratón sobre cualquiera de las barras, se visualiza una ventana emergente (*tooltip*) con la Razón Social completa y el importe exacto.

![Captura 2: Panel de KPIs y Gráfico de Deudores](./img/02_kpis_graficos.png)  
*Instrucciones de referencia:*  
*① Tarjetas métricas de Total Adeudado, Facturas y Clientes en Mora.*  
*② Monitores de extremos: Mayor y Menor deuda del período.*  
*③ Gráfico de barras interactivo con ranking de cobranza prioritaria.*

---

### Módulo C: Uso de la Tabla de Clientes

La tabla principal presenta la lista de deudores ordenada jerárquicamente por el monto de mora.

![Captura 3: Tabla Interactiva de Deudores](./img/03_tabla_deudores.png)  
*Instrucciones de referencia:*  
*① Buscador universal por nombre, código o factura.*  
*② Filtros complementarios: importe mínimo y rango de fechas (Desde/Hasta).*  
*③ Flecha de expansión para visualizar comprobantes agrupados.*  
*④ Acciones directas de contacto y cobranza.*

#### 1. Búsqueda y Filtros Combinados
* **Buscador de Texto:** Escriba cualquier fragmento del nombre del cliente, su código o el número de comprobante. La tabla filtrará en tiempo real.
* **Importe Mínimo ($):** Permite enfocarse en deudas significativas (por ejemplo, clientes que deban más de `$ 100.000`), excluyendo diferencias menores o saldos residuales.
* **Rango de Fechas (Desde / Hasta):** Permite aislar facturas emitidas en un rango específico con formato `DD/MM/AAAA`.
* **Botón Restablecer Filtros:** Vuelve la tabla a su visualización total sin recargar la página.

#### 2. Agrupamiento Jerárquico y Detalle de Facturas
1. Cada fila representa a un cliente con su **Cód. Cliente**, **Razón Social**, **Comprobantes**, **Saldo Cta Cte** y **Total Vencido**.
2. Haga clic en el ícono de la **flecha (`>`)** a la izquierda del cliente para desplegar el acordeón.
3. Se abrirá una sub-tabla con el detalle pormenorizado:
   * **N° Factura Vencida:** Número del comprobante.
   * **Fecha Emisión:** Día original de emisión.
   * **Días de Mora:** Días corridos transcurridos desde su facturación.
   * **Importe Original:** Valor nominal inicial de la factura.
   * **Saldo Pendiente:** Saldo real que resta cobrar de dicho comprobante.
   * **Acción Individual:** Botón verde para reclamar puntualmente solo esa factura.

---

### Módulo D: Gestión de Contactos y Envío por WhatsApp

El sistema se integra de forma directa con WhatsApp Web para eliminar la necesidad de redactar manualmente los mensajes de cobro o copiar y pegar números en el celular.

![Captura 4: Modal de Clientes y Envío por WhatsApp](./img/04_clientes_whatsapp.png)  
*Instrucciones de referencia:*  
*① Botón de WhatsApp para reclamo global del cliente.*  
*② Botón "Agendar Cliente" para registrar teléfonos faltantes.*  
*③ Vista previa del mensaje con datos bancarios oficiales autocompletados.*

#### 1. Cómo Funciona el Botón de WhatsApp
* **Si el cliente está agendado:** Se muestra un botón verde con el logotipo oficial de WhatsApp. Al presionarlo:
  1. Se compila automáticamente el mensaje con el nombre o alias del cliente, la lista numerada de facturas impagas, el total consolidado y los datos bancarios de Chevroford.
  2. Se abre una pestaña nueva en su navegador directamente en WhatsApp Web dirigiéndose al número del destinatario con el texto cargado en la caja de redacción.
  3. Usted solo debe presionar la tecla **Enter** o el botón de enviar en WhatsApp para despacharlo.

* **Reclamo de Factura Individual:** Si un cliente tiene varias facturas pero usted solo desea recordarle una en particular (por ejemplo, una que venció hace más de 90 días), abra el acordeón del cliente y haga clic en el botón de WhatsApp ubicado en el renglón de esa factura específica.

#### 2. Cómo Agendar o Modificar un Contacto
* Si un cliente en la tabla no tiene teléfono registrado, aparecerá un botón gris con el ícono de **Usuario ("Agendar Cliente")**.
* Al hacer clic, el sistema lo llevará automáticamente a la pestaña **Clientes**, abriendo el formulario con la **Razón Social** ya completada.
* Ingrese el **Número de Teléfono** (puede incluir código de área sin 0 ni 15, espacios o guiones; el sistema limpia el formato automáticamente).
* Ingrese un **Alias o Apodo** (opcional): Por ejemplo, si la razón social es *"Transportes San Cristóbal S.A."*, pero la persona de contacto en tesorería es *"Mariano"*, guarde *"Mariano"*. El mensaje saludará: *"Hola, Mariano..."* en lugar del frío nombre societario.
* Haga clic en **"Guardar Cliente"**. Al volver a la tabla de conciliaciones, el botón verde de WhatsApp ya estará habilitado.

#### 3. Personalización de Plantillas de Mensajes
En la pestaña superior **Mensajes**, usted puede crear distintas alternativas de comunicación (por ejemplo: *"Recordatorio Amable"*, *"Aviso Segundo Vencimiento"*, *"Gestión Pre-Judicial"*).

Puede insertar las siguientes etiquetas inteligentes que el sistema reemplazará al vuelo:
* `{alias}`: Nombre informal o Razón Social.
* `{razon_social}`: Nombre formal de la empresa.
* `{total_deuda}`: Monto final a pagar debidamente formateado en pesos.
* `{lista_facturas}`: Listado detallado de facturas, fechas e importes.
* `{datos_bancarios}`: Bloque oficial de transferencia bancaria (Banco ICBC, CUIT, CBU y Alias `CHEVROFORD-ICBC`).

---

### Módulo E: Exportación de Informes

Para reuniones de gerencia, respaldo de auditoría o entrega al equipo contable externo, el sistema permite exportar los resultados procesados en dos formatos estándar:

#### 1. Exportar a PDF (Reporte Ejecutivo)
* Presione el botón rojo **"Exportar PDF"** ubicado en el bloque superior derecho.
* Se generará un documento formal con membrete institucional, resumen de métricas clave, detalle ordenado por cliente y comprobante, paginado y con fecha/hora de emisión garantizada para control documental.

#### 2. Exportar a Excel (Planilla de Trabajo)
* Presione el botón verde **"Exportar Excel"** ubicado junto al botón de PDF.
* Se descargará un archivo `.xlsx` estructurado con la totalidad de los registros resultantes del filtro, listo para ser trabajado con tablas dinámicas, cruces en Power BI o impresiones locales.

---

## 5. MARCADORES PARA RECURSOS MULTIMEDIA

*(Espacio asignado para la inserción de capturas de pantalla de la interfaz gráfica)*

```text
========================================================================================
[CAPTURAS DE PANTALLA REQUERIDAS - DIRECTORIO: docs/img/]
========================================================================================

1. docs/img/01_carga_excel.png
   -> Captura del área de arrastre de Excel, selector de días y botón 'Generar Reporte'.

2. docs/img/02_kpis_graficos.png
   -> Captura de las 5 tarjetas estadísticas y el ranking de barras interactivo.

3. docs/img/03_tabla_deudores.png
   -> Captura de la tabla principal con filtros superiores y una fila de cliente desplegada.

4. docs/img/04_clientes_whatsapp.png
   -> Captura de la pestaña de Clientes/Mensajes y la ventana de WhatsApp Web enlazada.
========================================================================================
```

---

## 6. RESOLUCIÓN DE PROBLEMAS (TROUBLESHOOTING) Y FAQ

### 1. "Al presionar 'Generar Reporte' el sistema muestra un mensaje de error en el formato del archivo."
* **Causa:** El archivo subido no contiene las columnas requeridas o los encabezados fueron renombrados.
* **Solución:** Abra el archivo en Excel y corrobore que en la primera fila existan exactamente las columnas: `COD_CLI`, `RAZON_SOC`, `SALDO`, `ESTADO`, `FECHA_EMIS`, `N_COMP`, `DEBE`. Asegúrese de que no contenga filas vacías previas al encabezado.

### 2. "Un cliente tiene una factura vencida hace 90 días en Tango pero no aparece en la tabla del sistema."
* **Causa:** Criterio de imputación cronológica (FIFO). Si el cliente entregó valores posteriores, pagos a cuenta o notas de crédito por un monto que cubrió esa factura, la deuda ya no es exigible sobre ese comprobante específico. El sistema protege a la empresa de cometer el error de reclamar algo ya saldado.
* **Solución:** Verifique el saldo actual de la cuenta corriente en la columna `SALDO`. Si el saldo es inferior a la factura vieja, el sistema actuó de forma correcta.

### 3. "Hago clic en el botón de WhatsApp pero la página no se abre."
* **Causa:** El navegador bloqueó las ventanas emergentes (*pop-ups*).
* **Solución:** En la barra de direcciones del navegador, busque el ícono de ventana bloqueada (al costado derecho de la URL), haga clic y elija *"Permitir siempre ventanas emergentes de este sitio"*.

### 4. "Al presionar el botón de WhatsApp, el chat abre un número inexistente o da error en la aplicación."
* **Causa:** El número de teléfono fue cargado con caracteres especiales o falta la característica.
* **Solución:** Ingrese a la pestaña **Clientes**, busque al cliente, presione el ícono del **Lápiz (Editar)** y asegúrese de que el número incluya el código de área (ejemplo para Rafaela: `3492123456`). No es necesario agregar el `+54` ni el `9`; el sistema lo estandariza automáticamente.

### 5. "El buscador de la tabla no encuentra un comprobante."
* **Causa:** Es posible que tenga activo un filtro restrictivo de fechas (Desde/Hasta) o un monto mínimo que deja afuera a dicho comprobante.
* **Solución:** Presione el botón **"Restablecer Filtros"** para limpiar los campos y vuelva a escribir el número en el buscador.

---

## 7. GLOSARIO DE TÉRMINOS

* **Saldo Cta Cte (Saldo de Cuenta Corriente):** Posición monetaria neta final del cliente con la empresa al momento de la exportación de Tango Software. Si es positivo indica deuda a cobrar; si es cero o negativo indica que el cliente está al día o posee saldo a favor.
* **Total Vencido Real:** Sumatoria de las deudas exigibles del cliente que no han sido compensadas por pagos a cuenta y cuya fecha de emisión supera el umbral de mora seleccionado (25, 30 o 60 días).
* **Criterio FIFO Reverso (First In, First Out inverso):** Método financiero de imputación contable que aplica los fondos disponibles a cubrir cronológicamente las facturas, garantizando que el saldo deudor corresponda exclusivamente a los compromisos no cancelados.
* **Nickname / Alias:** Nombre de pila, apodo o referente de tesorería del cliente registrado en el sistema para entablar una comunicación empática y comercialmente cercana a través de WhatsApp.
* **Comprobante PEN:** Estado en Tango Software que denota un documento con saldo pendiente formal de registración contra recibo.
* **Datos Bancarios Institucionales:** Constante bancaria oficial de **CHEVROFORD RAFAELA SRL** integrada en cada comunicación (Banco ICBC, Cuenta Corriente, CUIT, CBU y Alias oficial).

---
*Manual oficial de operaciones. Chevroford Rafaela S.R.L. Prohibida su distribución fuera de la organización.*
