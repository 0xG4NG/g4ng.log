# g4ng · lab notebook

Blog personal y cuaderno técnico. Construido con Astro, desplegado en [g4ng.dev](https://g4ng.dev).

## Stack

- **Framework:** [Astro](https://astro.build) (SSG) con MDX
- **Estilos:** CSS personalizado (sistema de diseño propio, sin frameworks)
- **Sintaxis:** [Shiki](https://shiki.style) — tema `github-dark-dimmed`
- **Entorno:** [Nix Flakes](https://nixos.wiki/wiki/Flakes) para reproducibilidad
- **Feeds:** RSS + Sitemap automático

## Estructura

```
src/
├── components/   # Componentes Astro reutilizables
├── content/
│   └── blog/     # Posts en Markdown / MDX
├── layouts/      # BlogPost, BaseLayout
├── pages/        # Rutas (index, blog/[slug], about, rss)
└── styles/
    └── global.css  # Design system completo
```

## Desarrollo

### Con Nix (recomendado)

```sh
nix develop        # Entra al shell con Node incluido
npm install
npm run dev        # http://localhost:4321
```

### Sin Nix

Requiere Node >= 22.12.0.

```sh
npm install
npm run dev
```

## Comandos

| Comando           | Acción                                  |
| :---------------- | :-------------------------------------- |
| `npm run dev`     | Servidor de desarrollo en `:4321`       |
| `npm run build`   | Build de producción en `./dist/`        |
| `npm run preview` | Preview del build antes de desplegar    |
| `npx astro check` | Validación de tipos TypeScript          |

## Nuevo post

Crea un archivo en `src/content/blog/<slug>.md` con este frontmatter:

```yaml
---
title: "Título del post"
pubDate: 2026-01-01
description: "Resumen breve para SEO"
author: "g4ng"
tags: ["nix", "astro"]
---
```

## Licencia

Contenido © g4ng. Código bajo [MIT](https://opensource.org/licenses/MIT).
