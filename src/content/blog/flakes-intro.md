---
title: 'Introducción a Nix Flakes'
description: 'Qué son los flakes, por qué usarlos y cómo configurar tu primer flake para gestionar NixOS'
pubDate: 2026-03-25
heroImage: '../../assets/covers/flakes-intro.svg'
tags: ['nixos', 'nix', 'flakes']
category: 'NixOS'
---

Los Flakes son la forma moderna de gestionar configuraciones en Nix. Proporcionan reproducibilidad, composición y un sistema de lock que garantiza que tus builds sean idénticos en cualquier máquina.

## Qué es un Flake

Un flake es simplemente un directorio con un fichero `flake.nix` que define:

- **inputs**: dependencias (nixpkgs, home-manager, otros flakes)
- **outputs**: lo que produce (configuraciones NixOS, paquetes, shells de desarrollo)

```nix
{
  description = "Mi configuración NixOS";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
  };

  outputs = { self, nixpkgs }: {
    nixosConfigurations.mi-maquina = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [ ./configuration.nix ];
    };
  };
}
```

## Por qué usar Flakes

**Sin flakes**, tu configuración depende del canal (`nixos-unstable`, `nixos-24.11`) que tengas configurado globalmente. Dos máquinas con el mismo `configuration.nix` pueden producir resultados distintos si están en diferentes versiones del canal.

**Con flakes**, el fichero `flake.lock` fija la versión exacta de cada input. El resultado es siempre el mismo, en cualquier máquina, en cualquier momento.

Otras ventajas:

- Composición fácil de múltiples fuentes (nixpkgs, overlays, módulos externos)
- CLI unificada: `nix build`, `nix develop`, `nix run`
- Evaluación pura: no depende de estado global

## Habilitar Flakes

Flakes todavía es una feature "experimental" (aunque la usa casi todo el mundo). Añade esto a tu configuración:

```nix
{
  nix.settings.experimental-features = [ "nix-command" "flakes" ];
}
```

## Estructura típica de un flake NixOS

```
~/nixos-config/
├── flake.nix          # Punto de entrada
├── flake.lock         # Versiones fijadas (autogenerado)
├── configuration.nix  # Configuración del sistema
├── hardware-configuration.nix
└── modules/
    ├── networking.nix
    ├── desktop.nix
    └── services.nix
```

## Ejemplo completo con Home Manager

```nix
{
  description = "NixOS + Home Manager";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    home-manager = {
      url = "github:nix-community/home-manager";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, home-manager }: {
    nixosConfigurations.mi-maquina = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [
        ./configuration.nix
        home-manager.nixosModules.home-manager
        {
          home-manager.useGlobalPkgs = true;
          home-manager.useUserPackages = true;
          home-manager.users.g4ng = import ./home.nix;
        }
      ];
    };
  };
}
```

## Comandos útiles

```bash
# Aplicar configuración NixOS desde un flake
sudo nixos-rebuild switch --flake .#mi-maquina

# Actualizar todos los inputs
nix flake update

# Actualizar solo nixpkgs
nix flake update nixpkgs

# Ver los inputs y sus versiones
nix flake metadata

# Entrar en un shell de desarrollo
nix develop

# Comprobar que el flake es válido
nix flake check
```

## Consejos

- Versiona tu flake con git. Nix solo evalúa ficheros que estén tracked por git.
- Usa `inputs.nixpkgs.follows` para que todos tus inputs compartan la misma versión de nixpkgs.
- No edites `flake.lock` manualmente — usa `nix flake update`.
- Si algo falla después de un update, puedes hacer `git checkout flake.lock` para volver a la versión anterior.
