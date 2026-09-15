# PROMPT MAESTRO — App de Menú Interactivo E-commerce (Electronics Parts) · GitHub + Supabase + Vercel

> Copia TODO el bloque de abajo y pégalo en un chat nuevo de Claude Opus 4.8.
> Ya trae el WhatsApp real y la paleta bloqueada al logo de la marca.

---

```
Actúa como diseñador de producto senior + ingeniero frontend especializado en
interfaces web de conversión y e-commerce ligero. Vas a construir un APLICATIVO
WEB COMPLETO tipo catálogo/menú interactivo de productos con carrito, pasarela
de pago SIMULADA y redirección a WhatsApp, listo para indexar en GitHub, con
datos en Supabase y despliegue en Vercel.

No inventes un estilo distinto: replica la metodología de diseño que te doy más
abajo y respeta la paleta EXACTA de la marca (ya te la doy, no la cambies).
Método obligatorio: construyes, renderizas, MIRAS la captura real, verificas
desborde en 5 anchos, corriges, y solo entonces sigues. No entregues nada que
no hayas visto renderizado. La consola debe quedar sin errores.

═══════════════════════════════════════════════════════════
0. MARCA Y CONTEXTO DE NEGOCIO
═══════════════════════════════════════════════════════════
Negocio: Electronics Parts (electronicspartscol)
Qué hace: Venta al por mayor y detal de accesorios para celulares, celulares
  nuevos y servicio técnico en Ibagué. Cargadores, vidrios templados, forros,
  audífonos, parlantes, protectores de cámara, cases personalizados, Fire TV
  Stick, AirPods, etc.
Ciudad: Ibagué, Tolima, Colombia
WhatsApp de ventas: 3223980005
  → El link SIEMPRE debe formarse como https://wa.me/573223980005?text=...
  → Guárdalo como constante única en JS (WHATSAPP = "573223980005"). No lo
    escribas suelto en varios sitios.
Instagram: @electronicspartscol · TikTok: @electronicspartscol

─── PALETA OBLIGATORIA (extraída del logo real de la marca) ───
El logo es una estrella/ráfaga geométrica multicolor sobre fondo blanco, con un
anillo degradado tipo Instagram alrededor. NO inventes colores: usa estos.

  Azul cian (rayo superior, ACENTO PRINCIPAL de acción): #2BB8E6
  Azul navy profundo (cuerpo de la estrella, BASE OSCURA de marca): #1A3A5C
  Naranja (rayo lateral, OFERTAS / deseo): #E8622A
  Rojo (rayo inferior, ERRORES / urgencia / agotado): #D62F2F
  Negro carbón (contornos): #1C1C1E
  Anillo degradado de firma (rosa → magenta → amarillo): usar SOLO como toque
    decorativo (halo del logo, borde de avatar), nunca en fondos amplios.

  Asignación de tokens de marca (deriva los 6 niveles a partir del cian y navy):
    --brand-500 = #2BB8E6  (cian, acción principal)
    --brand-700 = #1A3A5C  (navy, confianza / fondos oscuros / botón primario)
    --brand-600 = tono intermedio entre cian y navy (para el degradado del botón)
    --brand-50 / --brand-100 / --brand-300 = versiones muy claras del cian para
      fondos de eyebrow, chips y hovers suaves.
    --accent-warm = #E8622A (badge OFERTA / precios rebajados)
    --danger      = #D62F2F (errores reales, stock agotado)

  Regla: acción principal en CIAN, superficies oscuras premium en NAVY, ofertas
  y urgencia en NARANJA, errores en ROJO. Cada color significa lo mismo en todo
  el sitio. La estrella multicolor del logo es el ELEMENTO FIRMA (ver punto 2.5).

DIRECCIÓN ESTÉTICA: interfaz moderna de gadgets, hero oscuro premium en NAVY con
luces ambientales cian, tarjetas de producto limpias con badge (Nuevo/Oferta),
CTA de acento único por pantalla en cian, franja de garantías (envío/garantía/
servicio técnico), sección de estadísticas y testimonio. Sensación "tienda tech
premium de Ibagué", NO plantilla genérica.

IMPORTANTE: los productos reales los cargo yo después desde el panel admin.
Arranca con 8-12 productos de ejemplo (semilla) coherentes con el catálogo real
(cargadores 20W, vidrios templados, forros, audífonos, AirPods, Fire TV Stick,
cases personalizados, protectores de cámara). Que se puedan borrar de un clic.

═══════════════════════════════════════════════════════════
1. ALCANCE FUNCIONAL — QUÉ DEBE HACER LA APP
═══════════════════════════════════════════════════════════

A) VISTA PÚBLICA (cliente):
   - Hero con propuesta de valor + CTA principal (cian).
   - Buscador en vivo (filtra mientras se escribe).
   - Filtro por categoría (chips) y orden (precio ↑↓, nuevos, más vendidos).
   - Grid de productos: imagen, nombre, precio (tabular-nums), badge de estado,
     stock, botón "Agregar". Estado agotado en rojo deshabilita el botón.
   - Ficha de producto (modal o vista): imagen grande, descripción, precio,
     selector de cantidad, agregar al carrito.
   - CARRITO lateral (drawer): items, cantidad editable, subtotal, eliminar,
     total con tabular-nums. Persiste en localStorage.
   - CHECKOUT con pasarela de pago SIMULADA: formulario (nombre, WhatsApp,
     ciudad, dirección/nota, método de pago simulado: efectivo / transferencia /
     tarjeta-demo). Validación en tiempo real (input y blur, nunca solo al
     enviar). Al confirmar: guarda la orden en Supabase, muestra resumen y
     REDIRIGE A WHATSAPP con mensaje pre-armado que incluya: lista de productos,
     cantidades, precios, total, datos del cliente y método de pago. Formato:
     https://wa.me/573223980005?text=[mensaje URL-encoded]
   - Franja de garantías, estadísticas y un testimonio (estilo tienda tech).
   - Footer con redes reales (IG y TikTok @electronicspartscol) y WhatsApp.

B) PANEL ADMIN (gestión, protegido):
   - Acceso por login (Supabase Auth: email + contraseña). Nada de contraseña
     en el HTML.
   - CRUD completo de productos: crear, editar, eliminar, activar/desactivar.
   - Campos por producto: nombre, categoría, precio, precio_oferta (opcional),
     stock, descripción, imagen (URL o subida a Supabase Storage), badge
     (nuevo/oferta/más vendido/ninguno), activo (bool), orden.
   - Editar precios EN LÍNEA (inline edit) con guardado optimista.
   - Gestión de categorías (crear/renombrar/color por categoría).
   - Los cambios se reflejan en la vista pública EN TIEMPO REAL (Supabase
     realtime) con respaldo a localStorage si cae la conexión.
   - Los 4 estados en toda vista con datos: cargando (skeleton con la forma
     real), vacío (ilustración + acción), error (mensaje humano + reintentar),
     con datos.

C) EXTRAS de conversión (implementa al menos 2):
   - Botón flotante de WhatsApp (56px, circular, cian/navy).
   - Contador de items en el ícono del carrito.
   - "Pedido mínimo mayorista" opcional configurable.
   - Compartir producto (copia link / abre WhatsApp).

═══════════════════════════════════════════════════════════
2. SISTEMA DE DISEÑO (replicar exacto)
═══════════════════════════════════════════════════════════

2.1 TODO EN VARIABLES CSS en :root. Nunca un color/radio/sombra directo en el
código. Modo claro y oscuro con toggle persistente (redefine tokens bajo
html[data-theme="dark"], guarda la preferencia). Nunca #000 ni #FFF en áreas
grandes: usa versiones teñidas del navy.
  --canvas:#F7F9FB; --surface:#FFFFFF; --border:#E2E8F0;
  --brand-50/100/300/500/600/700  (derivados de cian #2BB8E6 y navy #1A3A5C)
  --ink-900:#0F172A / --ink-700 / --ink-500 / --ink-300:#94A3B8
  --success --warning --danger(#D62F2F) --info --accent-warm(#E8622A)
    (cada uno con su -50 de fondo)
  --r-sm:8px --r-md:14px --r-lg:20px --r-xl:28px --r-full:999px
  --e1:0 2px 8px rgba(15,23,42,.05) / --e2:0 8px 28px rgba(15,23,42,.08) /
  --e3:0 20px 56px rgba(15,23,42,.13)   ← sombras SIEMPRE teñidas del texto
  --ease:cubic-bezier(.22,.61,.36,1)

2.2 TIPOGRAFÍA — 3 familias con función:
  Display (titulares): geométrica, peso 600-800, letter-spacing -.02em,
    line-height 1.13, clamp(1.7rem,3.3vw,2.4rem).
  Cuerpo: neutra legible, 400-600, line-height 1.55.
  Tabular/mono: PRECIOS, cifras, cantidades → font-variant-numeric:tabular-nums
    OBLIGATORIO en todo dinero. Sin eso los números bailan y se ve amateur.
  Eyebrow: MAYÚSCULAS, letter-spacing .11em, fondo --brand-50, color --brand-700,
    border-radius 100px.

2.3 DEGRADADOS — 4 usos, nunca decorativos:
  a) Botón primario: linear-gradient(135deg,--brand-700,--brand-600) [navy→cian],
     hover translateY(-2px)+sombra más profunda.
  b) Hero: 3 paradas casi imperceptibles en navy para dar profundidad.
  c) Luces ambientales: círculos radiales cian difuminados en pseudo-elementos,
     pointer-events:none, overflow:hidden en el contenedor, filter:blur(58px).
     Esto da la sensación premium.
  d) Bloques de énfasis: tarjeta navy oscura con degradado diagonal (sección
     stats / "listo para actualizar tu tech").
  El degradado rosa→magenta→amarillo del anillo del logo SOLO en el halo del
  elemento firma. PROHIBIDO: degradados de colores opuestos o de +3 paradas en
  fondos amplios.

2.4 ANIMACIONES — solo 5 patrones, anima SOLO transform y opacity:
  1) Hover = las 3 cosas juntas: translateY(-4px) + sombra mayor + border-color
     cian (+ opcional línea 3px que se dibuja con scaleX(0→1), origin left).
  2) Aparición al scroll con IntersectionObserver (sin librerías):
     .rv{opacity:0;transform:translateY(20px);transition:.55s ease} .rv.in{...}.
     Escalonado 60ms con .rv-stagger.
  3) Marquesina infinita (franja de garantías/marcas): duplica contenido,
     desplaza -50%, pausa al hover, degradados laterales.
  4) Flotación de elementos destacados con animation-delay NEGATIVOS distintos
     (-1s,-3.5s,-5.5s) para que no se muevan sincronizados.
  5) Entrada al cambiar de vista: @keyframes fade.
  Duraciones: .13-.18s táctil · .25-.35s cambio de vista · .55s apariciones.
  OBLIGATORIO: @media (prefers-reduced-motion:reduce){*{animation/transition:
  .01ms!important}}

2.5 COMPONENTES CON CARÁCTER:
  Píldora de estado con punto currentColor. Eyebrow monoespaciada. Campos de
  formulario con borde 1.5px, sombra interior sutil, select con flecha SVG
  propia, foco con anillo 3px cian. Tarjetas de categoría con borde izquierdo
  4px del color de la categoría + fondo degradado tenue + chip. Botón flotante
  56px scale(1.08) al hover.
  ELEMENTO FIRMA (pieza visual única): recrea en SVG la estrella/ráfaga
  geométrica multicolor del logo (rayos en cian #2BB8E6, navy #1A3A5C, naranja
  #E8622A y rojo #D62F2F). Colócala en el hero con: halo conic-gradient
  (rosa→magenta→amarillo, el del anillo) girando detrás, dos anillos concéntricos
  girando en direcciones opuestas, y píldoras de datos flotando alrededor
  (Envíos en Ibagué · Garantía · Servicio técnico). NO reproduzcas logos de
  terceros; para marcas de producto usa fichas con iniciales + su color.

2.6 VALIDACIÓN DE FORMULARIOS en tiempo real (checkout): 3 estados con color de
  FONDO además del borde. :focus anillo 3px cian al 9% · .good verde + fondo
  verde claro · .bad rojo + fondo rojo claro. Valida en input y blur. Mensajes
  específicos: "El WhatsApp debe tener 10 dígitos", nunca "campo inválido".

2.7 RESPONSIVE mobile-first. Cortes 1279/1023/767/479px. Grid de productos que
  colapsa. Barra lateral → pestañas inferiores (máx 5). Área táctil 44×44px.
  inputmode correcto (tel/numeric/email). safe-area-inset en iOS.
  VERIFICACIÓN en 360/390/768/1366/1440px:
  scrollWidth - clientWidth === 0. Cualquier otro número es desborde.
  Trampas ya conocidas: backdrop-filter en padre rompe position:fixed de hijos;
  overflow-y:visible no funciona junto a overflow-x:auto (usa padding vertical
  en el carril); paneles con translateX(105%) generan scroll fantasma (usa
  visibility:hidden + overflow-x:hidden en html); usa selector hijo directo
  .card>b, no .card b.

2.8 JERARQUÍA DE COLOR: navy=acción/confianza/superficie oscura · cian=UNA
  llamada de acción por pantalla · naranja=ofertas/deseo · rojo=solo errores/
  agotado · verde=éxito/confirmación. Sin excepción.

2.9 NUNCA: sombras negras puras · +2 display · animar width/height/top/left ·
  bordes 1px en tarjetas interactivas · colores en el HTML · emojis como iconos
  de UI (usa SVG; emojis solo en contenido) · reproducir logotipos de marcas
  registradas (usa fichas con iniciales + color oficial).

═══════════════════════════════════════════════════════════
3. ARQUITECTURA TÉCNICA Y REPOSITORIO
═══════════════════════════════════════════════════════════

Estructura ARCHIVOS SEPARADOS (para poder crecer):
/
├── index.html          (tienda pública)
├── admin.html          (panel de gestión, tras login)
├── assets/
│   ├── css/  tokens.css · componentes.css · tienda.css · admin.css
│   ├── js/   supabase-client.js · datos.js · carrito.js · tienda.js · admin.js · auth.js
│   └── img/
├── vercel.json
└── README.md

Stack: HTML/CSS/JS vanilla (sin framework, sin build). Supabase JS por CDN.
Sin dependencias pesadas. Gráficas (si las hay) dibujadas en SVG a mano.

Supabase:
  - Cliente por CDN, URL y anon key en supabase-client.js como constantes
    claramente marcadas para que yo las reemplace.
  - Entrégame el SQL COMPLETO del esquema listo para pegar en el SQL Editor:
    · tabla products (id, nombre, categoria_id, precio, precio_oferta, stock,
      descripcion, imagen_url, badge, activo, orden, created_at)
    · tabla categories (id, nombre, color, orden)
    · tabla orders (id, cliente_nombre, cliente_whatsapp, ciudad, direccion,
      metodo_pago, items jsonb, total, estado, created_at) — registra cada
      pedido antes de mandarlo a WhatsApp
  - Row Level Security: lectura pública SOLO de products/categories activos;
    escritura SOLO para usuarios autenticados (admin). Dame las políticas RLS
    exactas. Advertencia en el README: la anon key queda visible; por eso el
    admin va con Supabase Auth real y RLS desde el día uno.
  - Realtime: la tienda pública se suscribe a cambios de products/categories.
  - Respaldo a localStorage si la conexión falla (la tienda no debe quedar en
    blanco nunca).

vercel.json (cache — evita el "sitio roto" que en realidad es caché):
  headers: /(.*)\.(css|js|json|html) → Cache-Control public, max-age=0,
    must-revalidate; /assets/img/(.*) → max-age=31536000, immutable.
  cleanUrls:true, trailingSlash:false.
Versiona los enlaces: <link ...?v=1>, <script ...?v=1 defer>. Súbeme el número
en cada entrega y recuérdamelo.

═══════════════════════════════════════════════════════════
4. ENTREGABLES (en este orden)
═══════════════════════════════════════════════════════════
1. Los HEX finales de los 6 niveles de marca derivados de cian #2BB8E6 y navy
   #1A3A5C (muéstrame la escala).
2. SQL completo del esquema + políticas RLS, listo para pegar.
3. Todos los archivos del repo, completos y funcionales, con datos semilla.
4. README con: cómo crear el proyecto Supabase, dónde pegar URL+anon key, cómo
   correr el SQL, cómo crear el usuario admin, cómo subir a GitHub y desplegar
   en Vercel (sin framework, sin build command, sin output dir), y el checklist
   de subida (subir assets completo, subir el ?v=, verificar con Ctrl+Shift+R).
5. Confirmación de que verificaste: 0 desborde horizontal en los 5 anchos,
   consola limpia, flujo carrito→checkout→WhatsApp (573223980005) probado de
   punta a punta, y CRUD admin probado creando/editando/borrando un producto.

Los 3 detalles que más se notan y NO puedes olvidar:
- animation-delay NEGATIVOS en los elementos que flotan.
- hover con las 3 cosas a la vez (mover + sombra + borde).
- tabular-nums en todas las cifras de dinero.

Empieza por el punto 1 y 2, espera mi OK con las llaves de Supabase, y luego
construye todo.
```
