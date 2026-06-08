# 10 Claude Code Plugins You Must Install If You Are a Software Engineer

**Autor:** IAKH Studio  
**Fuente:** [Medium](https://ikh4ever.medium.com/10-claude-code-plugins-you-must-install-if-you-are-a-software-engineer-434f4279b3d3)  
**Fecha original:** 2026-05-25  
**Fecha de resumen:** 2026-06-08  
**Categoría:** ai-dev-tools

---

## Contexto

El ecosistema de plugins de Claude Code ha crecido hasta más de **9.000 plugins** distribuidos en tres plataformas: ClaudePluginHub, Claude-Plugins.dev y el Marketplace oficial de Anthropic. Out of the box, Claude Code tiene limitaciones reales: sin memoria entre sesiones, sin acceso web en tiempo real, sin testing de browser y sin guardarraíles contra cambios peligrosos. Los plugins cierran esas brechas.

---

## Los 10 plugins

### 1. Ralph Loop — Agente de coding autónomo
**57.000+ instalaciones**

Implementa un patrón de stop-hook que permite sesiones de coding largas y multi-tarea sin intervención humana. Se le pasa un PRD (Product Requirements Document), arranca el loop, y Claude implementa tarea a tarea, commitea, y empieza la siguiente con contexto limpio.

```bash
/plugin install ralph-loop@claude-plugins-official
```

Ideal para: generación de CRUD, migraciones, cobertura de tests y cualquier trabajo mecánico donde la especificación es clara.

> 💡 Consejo: cuanto más limpio el PRD, mejores los resultados. Requisitos vagos = Claude dando vueltas.

---

### 2. Context7 — Inyector de documentación en tiempo real
**71.800+ instalaciones**

Inyecta documentación viva y versionada antes de que Claude genere código. Elimina el problema clásico de Claude generando código con APIs deprecadas porque su conocimiento tiene fecha de corte.

Ideal para: cualquier proyecto que use librerías con versiones activas y documentación que cambia.

---

### 3. Firecrawl — Web scraping para agentes IA

Permite a Claude extraer, explorar y mapear contenido web sin escribir una sola línea de código de extracción.

```bash
/plugin install firecrawl@firecrawl-dev
/firecrawl:setup  # configurar API key
```

Comandos principales: `/firecrawl:scrape`, `/firecrawl:crawl`, `/firecrawl:search`, `/firecrawl:interact`, `/firecrawl:map`

Ideal para: investigación de mercado, análisis competitivo, agregación de documentación y cualquier workflow agentico que necesite datos web en tiempo real.

---

### 4. Playwright MCP — Testing de frontend en lenguaje natural
**28.100+ instalaciones**

Da a Claude control directo sobre una ventana de Chrome que se puede ver en tiempo real. En lugar de escribir scripts de test, se le dice a Claude en lenguaje natural: "testa el flujo de checkout" o "rellena el formulario de contacto y envíalo", y lo ejecuta.

```bash
/plugin install playwright@microsoft
```

Funcionalidad clave: se puede iniciar sesión manualmente en el navegador y que Claude tome el control desde esa sesión autenticada.

Ideal para: frontend engineers que necesitan testear aplicaciones complejas con autenticación sin escribir scripts.

---

### 5. Security Guidance — Guardarraíl de seguridad de código

Analiza el código generado por Claude antes de que llegue a producción, buscando vulnerabilidades, secretos expuestos y patrones peligrosos.

Ideal para: equipos que quieren un primer filtro de seguridad automático en todo código generado por IA.

---

### 6. Figma MCP — De diseño a código sin intermediarios

Da a Claude acceso de lectura directo a los ficheros Figma reales — no capturas, no descripciones, sino datos de diseño estructurados. Claude lee frames, componentes y datos de layout para generar código frontend que respeta el diseño.

```bash
/plugin install figma@figma
```

Ideal para: frontend engineers y full-stack developers que implementan UI a partir de ficheros de diseño y quieren reducir el trabajo manual de traducción.

---

### 7. Frontend Design — Cura para la estética genérica de la IA
**96.400+ instalaciones — el plugin más instalado de la lista**

La IA generativa tiene una estética reconocible: colores seguros, fuentes genéricas (Inter, Roboto), gradientes morados, componentes placeholder. Este plugin de skill empuja a Claude hacia elecciones de diseño más audaces e intencionadas: tipografía fuerte, identidad visual coherente, atmósferas visuales distintivas.

Ideal para: cualquier proyecto donde la UI generada por IA parezca... generada por IA.

---

### 8. Linear — El gestor de issues dentro de Claude

Integra Linear directamente en el flujo de trabajo de Claude Code: crea, actualiza y prioriza issues sin salir del terminal.

Ideal para: equipos que usan Linear como sistema de tracking y quieren que Claude pueda leer y actualizar el estado de las tareas en contexto.

---

### 9. Code Review — Revisión multi-agente de PRs

Proporciona un primer análisis estructurado de cada Pull Request antes de que llegue a un revisor humano. Múltiples subagentes atacan distintos aspectos (lógica, seguridad, tests, estilo).

```bash
/plugin install code-review@anthropic
```

Ideal para: engineers que quieren un fast-pass automático en cada PR.

---

### 10. Chrome DevTools MCP — Debug del browser desde Claude
**20.000+ instalaciones**

Da a Claude acceso completo al estado real del navegador usando la sesión de Chrome ya abierta: requests de red, errores de consola, métricas de rendimiento, estado del DOM.

```bash
/plugin install chrome-devtools@chrome
/chrome  # setup de la extensión
```

Preguntas que ahora se pueden hacer directamente: "¿por qué falló este request?" o "¿qué está bloqueando mi LCP score?".

Ideal para: frontend engineers depurando aplicaciones complejas con autenticación donde el análisis estático no llega.

---

## Recomendación de inicio

No hace falta instalar los 10 a la vez. El autor recomienda empezar con 3, que cubren las 3 mayores limitaciones de Claude Code por defecto:

| Plugin | Limitación que resuelve |
|--------|-------------------------|
| MemClaw (u otro de memoria) | Sin memoria entre sesiones |
| Playwright MCP | Sin capacidad de testing de browser |
| Security Guidance | Sin guardarraíles contra código peligroso |
