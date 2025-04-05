# Bind Shell en Assembly x86_64

Ce projet est un bind shell minimaliste écrit en assembleur pour Linux 64 bits. Il écoute sur un port local, accepte une connexion entrante, et fournit un shell (`/bin/sh`).

## Fonctionnalités
- Crée un socket TCP écoutant sur toutes les interfaces (`0.0.0.0`).
- Lie le socket au port 4444.
- Accepte une connexion entrante.
- Redirige stdin, stdout, stderr vers la connexion.
- Exécute `/bin/sh`.

## Prérequis
- Un système Linux 64 bits.
- `nasm` et `ld` pour la compilation.
- `netcat` (`nc`) pour tester la connexion.

## Compilation
1. Sauvegarde le code dans `bind_shell.asm`.
2. Assemble et linke avec :
   ```bash
   nasm -f elf64 bind_shell.asm -o bind_shell.o
   ld bind_shell.o -o bind_shell
   ```
3. L’exécutable `bind_shell` est prêt.

## Utilisation
1. Lance le bind shell :
   ```bash
   ./bind_shell
   ```
2. Connecte-toi depuis un autre terminal :
   ```bash
   nc 127.0.0.1 4444
   ```
3. Utilise le shell distant (ex. tape `whoami`).

### Configuration
- Le port est fixé à 4444. Pour le changer, modifie `mov word [rsi + 2], 0x5c11` avec ton port en big-endian (ex. `0x115c` pour 4444) et recompile.

## Limitations
- Port codé en dur (modifiable manuellement).
- Une seule connexion acceptée (pas de multi-threading ou fork).
- Pas d’authentification.

## Exemple
```bash
./bind_shell  # Sur la machine cible
nc 127.0.0.1 4444  # Depuis un client
whoami  # Réponse dans le terminal client
```

## Avertissement
Usage éducatif uniquement. Ne pas déployer sur des systèmes sans autorisation.
