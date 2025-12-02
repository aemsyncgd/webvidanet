# 📘 Project Best Practices

## 1. Project Purpose
Un sitio web estático de marketing (landing page) para VidaNet, un proveedor de servicios de internet por fibra óptica. Incluye secciones de héroe, ventajas, promociones, planes, testimonios y llamados a la acción; usa animaciones, un carrusel de planes y un interruptor de tema claro/oscuro.

## 2. Project Structure
- index.html: Documento principal que contiene:
  - Estructura HTML semántica de toda la landing.
  - Estilos embebidos (CSS extensivo) con variables, temas y responsive design.
  - Scripts embebidos (JS) para: AOS, Swiper, navegación, tema, gauge de velocidad.
- style.css: Archivo vacío reservado para extraer y modularizar estilos.
- javascript.js: Archivo vacío reservado para extraer y modularizar scripts.

Recomendación de roles/organización futura:
- css/: estilos modulares (base.css, components/*.css, utilities.css, themes.css, responsive.css).
- js/: scripts modulares (theme.js, nav.js, gauge.js, vendors-init.js, main.js).
- assets/: medios estáticos (imágenes, fuentes locales opcionales).
- vendors/: copias fijadas opcionales de librerías externas cuando sea necesario (o CDN con SRI).

## 3. Test Strategy
Proyecto estático sin framework de pruebas configurado actualmente.
- Smoke tests manuales: navegación, tema, interacción del menú, Swiper, AOS, CTA, accesibilidad básica.
- Visual/regresión manual: verificar responsive a 480/768/992/1200/desktop.
- Si se escala:
  - Integrar Playwright o Cypress para pruebas E2E de navegación, foco, atajos, acorde a heurísticas de accesibilidad.
  - Usar Lighthouse/Pagespeed para métricas de performance, PWA y SEO básico.
  - Usar HTMLHint, Stylelint, ESLint para linting de calidad.

## 4. Code Style
HTML
- Usar etiquetas semánticas (header, nav, main/section, footer) y roles ARIA donde apliquen.
- Mantener jerarquía de encabezados coherente (h1 por página, h2 por sección).
- Atributos de accesibilidad y seguridad: rel="noopener noreferrer" en enlaces externos, alt en imágenes, aria-labels descriptivos.

CSS
- Variables CSS (custom properties) centralizan tokens: colores, tipografías, radios, sombras, transiciones, gradientes.
- Temas: alternancia mediante clases light-theme/dark-theme en <html> o <body>; definir únicamente tokens temáticos en el selector del tema.
- Nomenclatura: preferir BEM para componentes nuevos (e.g., .header, .header__nav, .button--primary) para evitar colisiones.
- Responsivo: usar breakpoints consistentes (480, 768, 992, 1200). Evitar estilos inline; mover reglas al style.css y dividir por capas.
- Rendimiento: reducir sombras y blur en móviles; evitar animar propiedades no aceleradas por GPU (top/left); preferir transform/opacity.

JavaScript
- Mantener el JS en archivos separados (javascript.js) y módulos por feature (theme, nav, gauge), evitando mezclar lógica en HTML.
- Usar const/let, funciones puras cuando sea posible, y nombres descriptivos (camelCase para funciones/variables).
- Accesibilidad: manejar focus states, aria-expanded/aria-controls, y soporte para teclado (Enter/Espacio) en controles interactivos.
- Persistencia ligera: localStorage para tema ya está implementado; manejar errores con try/catch alrededor de APIs externas.
- No depender de elementos globales implícitos; resolver nodos con querySelector y validar null antes de usar.

## 5. Common Patterns
- Theming con CSS variables y toggle persistido en localStorage.
- Navegación responsive: menú hamburguesa que controla .nav-list.active con aria-expanded.
- IntersectionObserver para activar estados de navegación según sección visible.
- Integración de terceros por CDN: AOS (scroll animations), Swiper (carrusel de planes).
- Canvas gauge animado con easing y gradiente según variables CSS.

Patrones a reforzar
- Extracción a módulos (theme.js, nav.js, gauge.js) y estilos en archivos dedicados.
- Degradación progresiva: contenidos visibles sin JS; librerías externas opcionales.

## 6. Do's and Don'ts
✅ Do
- Centralizar tokens de diseño en :root y sobrescribir por tema.
- Usar ARIA y atributos semánticos correctos; actualizar aria-expanded/labels al cambiar estado.
- Cargar librerías con defer donde sea posible; inicializar después de DOMContentLoaded.
- Añadir SRI e integrity/crossorigin a recursos CDN cuando haya hash disponible.
- Minimizar DOM reflows; agrupar lecturas/escrituras; usar requestAnimationFrame para animaciones personalizadas.
- Optimizar imágenes (usar formatos modernos y tamaños adecuados); especificar width/height para evitar CLS.
- Añadir meta viewport, description y títulos claros (ya presente), y Open Graph/Twitter Cards si se comparte.

❌ Don’t
- No mezclar grandes bloques de CSS y JS inline en index.html; dificulta mantenimiento y caché.
- No animar propiedades de layout (width/height/top/left) en loops; evita timers imprecisos para animaciones.
- No depender de CDNs sin fallback o versionado; evita romper producción por cambios upstream.
- No dejar manejadores sin limpiar en componentes desmontables; si se usan listeners globales, documentarlos.

## 7. Tools & Dependencies
Librerías claves
- AOS (unpkg CDN): animaciones on-scroll.
- Swiper (jsdelivr CDN): carrusel responsivo.
- Font Awesome (cdnjs): íconos.
- Google Fonts: Poppins.

Setup recomendado
- Mover estilos a style.css y scripts a javascript.js; enlazar desde index.html con <link rel="stylesheet" href="./style.css"> y <script src="./javascript.js" defer></script>.
- Añadir un simple servidor estático para desarrollo (e.g., npx serve o VS Code Live Server) para probar rutas y CORS locales.
- Integrar linters: Stylelint y ESLint (si se adopta bundler, Vite/Parcel pueden facilitar).

## 8. Other Notes
- Idioma principal es español; mantener textos, aria-labels y copy coherentes.
- Mantener consistencia de breakpoints, sombras, radios y espaciados definidos en tokens.
- Si se versiona, fijar versiones de librerías (AOS/Swiper/Font Awesome) y usar SRI para seguridad.
- El canvas gauge depende de variables CSS; si cambian tokens, validar contraste/legibilidad.
- Considerar accesibilidad: contraste suficiente, foco visible, navegación por teclado, reducir animaciones si prefiere-reduce-motion.
