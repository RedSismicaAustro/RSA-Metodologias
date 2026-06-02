# Guía para Agentes de IA en RSA-Metodologias

Este repositorio es el **sitio de documentación institucional** de la Red Sísmica del Austro (RSA). Contiene metodologías, guías técnicas, el índice federado del exocortex y los Architecture Decision Records (ADR).

Está construido con **Jekyll** (tema Just the Docs) y se despliega en **GitHub Pages** en https://redsismicaaustro.github.io/RSA-Metodologias.

---

## 📂 Estructura del Repositorio

```text
RSA-Metodologias/
├── docs/                    # Guías institucionales (Jekyll) — entornos, git, redes, pcb
├── conocimiento/            # Índice de contextos técnicos (links a proyectos)
├── decisiones/              # Architecture Decision Records (ADR)
├── indice/                  # Índice federado del exocortex
│   ├── indice_tematico.md   # Fuente única de verdad del conocimiento RSA
│   └── catalogo_contribuidores.md
├── _config.yml              # Configuración Jekyll
└── index.md                 # Página principal del sitio
```

---

## 🛡️ Reglas de Comportamiento para este Repositorio

1. **No borres ni sobrescribas entradas del índice temático.** Solo añade o actualiza.
2. **El directorio `docs/` sigue las convenciones Jekyll** (front matter YAML con `title`, `parent`, `nav_order`).
3. **Los ADRs son inmutables una vez en estado "Aceptado"**. Para revertir una decisión, crea un nuevo ADR que la reemplace y cambia el estado del anterior a "Reemplazado por ADR-NNN".
4. **Formato de commits:** usa minúsculas (`feat:`, `docs:`, `fix:`).
5. **Idioma:** todo el contenido en español (excepto código).

---

## ⚙️ Skills Disponibles

Los siguientes skills del `RSA-Agent-Toolkit` interactúan con este repositorio:

- **`volcado_bitacora`**: Actualiza `indice/indice_tematico.md` al hacer volcado de sesiones.
- **`consulta_historica`**: Lee el índice para enrutar consultas históricas.
- **`generar_contexto`**: Actualiza `indice/indice_tematico.md` al generar contextos técnicos.
- **`extraer_adr`**: Crea archivos en `decisiones/` y actualiza el índice.

---

## 📝 Agregar Contenido

### Nueva guía en `docs/`
Sigue las convenciones Jekyll: archivo `.md` con front matter `title`, `parent`, `nav_order`.

### Nuevo contexto técnico
Usa el skill `generar_contexto`. El contexto vive en el proyecto fuente; el índice aquí se actualiza automáticamente.

### Nuevo ADR
Usa el skill `extraer_adr`. Nunca crees ADRs manualmente para asegurar el formato correcto.
