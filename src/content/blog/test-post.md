---
title: "Post de prueba"
pubDate: 2026-04-21
description: "Un post de test para verificar el layout, tipografía y estilos del blog"
author: "g4ng"
category: "meta"
tags: ["test", "astro", "blog"]
---

## Introducción

Este es un post de prueba para verificar que el layout funciona correctamente. Aquí puedes comprobar tipografía, espaciado, bloques de código y el resto de elementos visuales.

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation.

## Código

Un bloque de código inline: `const x = 42;`

Y un bloque completo:

```nix
{
  description = "Mi configuración de NixOS";

  inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";

  outputs = { nixpkgs, ... }: {
    nixosConfigurations.maquina = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [ ./configuration.nix ];
    };
  };
}
```

## Listas

- Elemento uno
- Elemento dos
- Elemento tres con **negrita** y _cursiva_

1. Primero
2. Segundo
3. Tercero

## Cita

> El software es como la entropía: difícil de capturar, pesa muy poco y obedece la segunda ley de la termodinámica.
> — Norman Augustine

## Conclusión

Si ves esto bien formateado, el blog está funcionando correctamente.
