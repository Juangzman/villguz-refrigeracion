# VILLGUZ Refrigeración · villguzrefrigeracion.com

Contexto del proyecto para Claude Code. Lo escribió Claude a pedido de Juan Guzmán
para seguir el trabajo hecho en el chat de Claude. Mantenelo al día cuando cambie algo importante.

> Este archivo está en `.claude/` a propósito: GitHub Pages no publica las carpetas que empiezan
> con punto, así que no aparece en la web. El repositorio sí es público, así que acá nunca van
> contraseñas, tokens, datos de clientes ni datos bancarios.

## Cómo trabajar con Juan

- **Respondé siempre en español** (Argentina, voseo: "tenés", "hacé").
- Juan es gerente general y socio de VILLGUZ, y diseñador. No es programador: explicá en
  palabras simples qué cambió y qué tiene que hacer él, sin jerga.
- Actuá como diseñador UX/UI senior y desarrollador front: proponé, hacé el cambio, revisalo
  visualmente y publicalo. No pidas permiso para cada detalle; sí preguntá antes de cambios
  grandes de estructura o de reglas del negocio.
- Al terminar: una o dos frases con lo que cambió y qué revisar. Sin listas largas de pasos.

## Publicación

- Hosting: **GitHub Pages** desde la rama `main` (archivo `CNAME` → `www.villguzrefrigeracion.com`).
  El dominio está registrado en **Namecheap**.
- **No hay paso de build**: cada página es un único `.html` con su CSS y JS adentro.
  Hacer `push` a `main` = publicar. Tarda unos minutos en verse.
- Commits en español, describiendo el cambio para Juan (ej.: "Turnos: WhatsApp valida el número…").
- `robots.txt` bloquea `/panel/` y `/turnos/` (son internas). Si agregás otra página interna, sumala ahí
  y poné `<meta name="robots" content="noindex">`.

## Estructura

| Ruta | Qué es |
|---|---|
| `index.html` | Web pública. Secciones: `#marcas`, `#servicios`, `#consejos`, `#como-trabajamos`, `#nosotros`, `#historia`, `#turnos` (formulario de pedido de turno), `#contacto`. |
| `panel/index.html` | **Panel interno de liquidación** (con contraseña). Pestañas: Liquidación, Trabajos, Fletes, Ayudantes, Técnicos y personal, Ajustes. Importa el Excel del sistema TS con SheetJS (`xlsx` 0.18.5 de cdnjs). |
| `turnos/index.html` | **Gestor interno de turnos** (contraseña distinta a la del panel). Vistas: Inicio, Hoy, Agenda, Taller, Clientes, Fletes, Ajustes. |
| `img/` | Fotos de servicios (`.webp`), logo, favicon. `img/marcas/` tiene los logos de las marcas. |

### Backend: Google Apps Script + Google Drive

- Los datos de las páginas internas se guardan en **Google Drive/Sheets** de la cuenta de Google
  Workspace de la empresa, a través de **Apps Script** (web apps `script.google.com/macros/s/…/exec`).
  Hay tres: el formulario público de turnos (`TURNOS_URL` en `index.html`, guarda en la planilla y
  abre WhatsApp), el panel (`CLOUD` y `TURNOS_CLOUD` en `panel/`) y el gestor de turnos (`CLOUD` en `turnos/`).
- **El código de Apps Script no está en este repo**: vive en el editor de Apps Script de Google.
  Si un cambio necesita tocar el backend, escribí el código `.gs` completo y explicale a Juan paso a
  paso cómo pegarlo y cómo hacer "Implementar → Gestionar implementaciones → Nueva versión" para que
  la URL no cambie.
- La contraseña de cada página interna se valida en Apps Script. Nunca la escribas en el código.
- Cada página también guarda copia local en `localStorage` (claves `villguz_panel…`, `villguz_turnos_v3`,
  cola offline `villguz_turnos_q3`). Si cambiás la forma de los datos, **migrá** los datos viejos;
  no cambies esas claves a la ligera porque el equipo perdería lo que tiene guardado.

## Marca

- Colores (variables CSS en `index.html`): azul marino `--navy #003370`, `--navy-deep #00224D`,
  verde `--green #76B72A` (`--green-ink #4E8A14` para texto), tinta `--ink #0C1624`, `--mist #EEF3F8`,
  `--line #D6DFEA`, `--muted #4A5A70`. Hay **modo oscuro** (`prefers-color-scheme` y `data-theme`):
  todo cambio de color se revisa en los dos modos.
- Tipografías: **Bebas Neue** (títulos) y **Montserrat** (texto), desde Google Fonts.
- **Los logos de las marcas (Samsung, LG, Whirlpool, BGH, Eslabón de Lujo, Beko, Vondom) van en sus
  colores originales: es exigencia de fábrica.** No los recolorees ni los pases a monocromo en la web.
- Firma animada del logo (para videos): fondo `#003370`, la W verde entra grande y se corre a la
  izquierda, salen "ILLGUZ" y debajo "REFRIGERACIÓN". Archivo original `firmafinal.aep` en la compu de Juan.

## Datos públicos de contacto

- WhatsApp: `5493814900124` (links `wa.me/5493814900124`).
- Mail: `serviciotecnico@villguzrefrigeracion.com`.
- Instagram: `@villguz.refrigeracion` (la red principal; publican consejos de uso).
- Servicio técnico oficial en San Miguel de Tucumán. Marcas: Samsung, LG, Whirlpool, BGH,
  Eslabón de Lujo, Beko y Vondom.

## Equipo (quién usa cada cosa)

- **Juan Guzmán**: gerente general y socio. Facturación, balances, manuales. Usuario maestro.
- **Inés Villalba**: socia. Empresas/marcas, casos de fábrica, pedidos de repuestos.
- **Griselda Villalba**: socia. Atención telefónica y presencial, confirma y asigna turnos, logística de taller.
- **Melina Guzmán**: WhatsApp, caja, presupuestos, carga de órdenes en el sistema.
- Técnicos: **Gabi** (lavarropas, heladeras, aire), **Marcelo** (heladeras alta gama),
  **Jorge** (lavarropas, lavavajillas). **Matías**: ayudante. Los fletes los hace un fletero externo con el ayudante.
- Los turnos los cargan Melina, Griselda, Inés y Juan. **Usan poco la computadora y mucho el celular**:
  las páginas internas tienen que ser muy simples, con botones grandes y pensadas primero para celular.
- Los técnicos tienen teléfonos básicos y algunos no abren PDF: lo que se les manda es texto corto por WhatsApp.

## Reglas del negocio

### Sistema TS
- Es el sistema de gestión que ya usan (unos 10 años de clientes). Cada visita genera un **N° de orden
  nuevo y único**. Presupuestos y diagnósticos se hacen ahí. Su exportación a Excel **no siempre es
  confiable** (ej.: visitas sin garantía sin presupuestar figuran en $0): todo lo importado debe poder
  corregirse antes de usarse.

### Turnos
- Tipos de orden: **GTIA** (garantía de fábrica), **S/G** (sin garantía), **GTIA TALLER** (garantía de
  una reparación hecha por VILLGUZ: se vuelve a atender sin cargo), **ACEPTADO** (el cliente aceptó el
  presupuesto tras una visita; hay que volver a hacer el trabajo).
- Plazo desde que el cliente llamó (no desde que se cargó), en días hábiles: **S/G 3 días, garantía 7 días**.
  A veces el cliente pide una fecha puntual.
- Carga normal: **5 servicios por técnico por día**; 6–7 es mucho (amarillo); 8 o más, advertencia
  (se puede asignar igual). Horario de visitas: **9 a 17 h**.
- Por defecto el trabajo lo vuelve a hacer **el técnico que lo vio primero**; otro solo si ese no puede.
- Cliente con más de 3 visitas en 3 meses → advertencia de urgente. Se pueden marcar clientes como
  complicados o "no atender".
- Zonas: todo San Miguel de Tucumán es **"SMT"**; la zona no es obligatoria.
- Garantía de fábrica: se pide la factura. Garantía de taller: no.
- En espera de repuesto se sigue: repuesto y código, producto/marca/modelo, si se pidió o espera
  confirmación de fábrica, garantía o no, si el cliente pagó, y si se compra en fábrica o en Tucumán.
- El problema principal a evitar: **que se olviden de asignar y pasen semanas sin turno**.

### Fletes
- Tipos: retiro, entrega o ambos (y clientes que piden solo flete). Estados: para retirar, retirado,
  para entregar, entregado. Se cargan y se ven **solo en la sección Fletes**, no en las visitas.
- Las tarifas por zona y el reparto entre fletero y ayudantes se configuran en el panel (Ajustes);
  no los dejes fijos en el código.

### Liquidación a técnicos
- Se paga **semanal, de sábado a viernes**; no se acumulan órdenes de otras semanas.
- Los técnicos son **externos tercerizados**: la liquidación se imprime y la firman.
- Pago por porcentaje de la mano de obra, editable por técnico, por orden y según garantía o no.
  Algunos trabajos en garantía se pagan con tarifas fijas (configurables en el panel).
- **En lo que ve y firma el técnico no figuran el total cobrado al cliente ni el precio de repuestos**:
  solo N° de orden, fecha, cliente, concepto, subtotales y totales.
- "Sin garantía · solo visita": el cliente le paga la visita al técnico; se registra para saber cuánto
  se cobró en visitas, pero no suma a lo que paga VILLGUZ.

## Seguridad y privacidad (obligatorio)

- **Nunca publicar el flyer de datos de transferencia** de VILLGUZ ("Mesa de trabajo 4.jpg"): tiene el
  alias bancario y el CUIT. Tampoco escribir esos datos en el repo.
- Nunca commitear datos de clientes, exportaciones del sistema TS, planillas ni copias de seguridad.
- En capturas de pantalla de los sistemas internos, usar datos de ejemplo, no clientes reales.
- Contraseñas y tokens los crea y los pega Juan; no los inventes ni los guardes en archivos.

## Cómo revisar un cambio antes de publicar

- Abrir la página localmente (`python3 -m http.server`) y revisarla en **celular (390 px) y escritorio
  (1440 px)**, en **modo claro y oscuro**.
- Para las páginas internas, probar sin tocar los datos reales de Drive (simular las respuestas de
  Apps Script o probar sin conexión).
- Revisar que el formulario público de turnos siga enviando y abriendo WhatsApp.

## Proyectos relacionados

- Portfolio de Juan: repo `Juangzman/juanguzmanph` (juanguzmanph.com), con su propio contexto.
- Hay un proyecto aparte de app propia (escritorio y celular) basada en el sistema TS; no vive en este repo.
