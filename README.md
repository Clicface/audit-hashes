# Clicface – Audit Hashes

[![Empreintes SHA-256](https://img.shields.io/badge/empreintes-SHA--256-blue)](hashes/)
[![Dernier rapport publié](https://img.shields.io/github/last-commit/Clicface/audit-hashes?label=dernier%20rapport%20publi%C3%A9)](https://github.com/Clicface/audit-hashes/commits/main)
[![Taille du dépôt](https://img.shields.io/github/repo-size/Clicface/audit-hashes?label=taille)](https://github.com/Clicface/audit-hashes)
[![Contenu : empreintes uniquement](https://img.shields.io/badge/contenu-empreintes%20uniquement-lightgrey)](#remarques)

Ce dépôt publie les **empreintes SHA-256** des rapports d'audit de sécurité générés par Clicface (vérification des ports ouverts, scans ZAP, etc.).

Les fichiers de hachage sont stockés dans le répertoire :
- `hashes/`

Chaque fichier `.sha256` correspond à un rapport et contient une ligne au format :

```text
<empreinte SHA-256>  <nom-du-fichier-rapport>
```

Exemple :

```text
bd51f2f3d2f7b4a9bb4a1f0f3c9e...  2025-11-11_18-32-29_verification-des-ports-ouverts.pdf
```

## Vérification de l'intégrité d'un rapport

1. Télécharger le rapport depuis la source fournie (par exemple un PDF).
2. Vérifier qu'un fichier `.sha256` correspondant existe dans `hashes/`.
3. Calculer l'empreinte SHA-256 en local :

```bash
sha256sum 2025-11-11_18-32-29_verification-des-ports-ouverts.pdf
```

4. Comparer le résultat avec la valeur publiée dans le fichier :

```bash
cat hashes/2025-11-11_18-32-29_verification-des-ports-ouverts.pdf.sha256
```

Si les empreintes sont identiques, le rapport n'a pas été modifié depuis la publication de son haché.

## Vérification en une commande

Les fichiers `.sha256` suivent exactement le format de sortie de `sha256sum`, la comparaison peut donc être automatisée.

Définir d'abord trois variables : le répertoire contenant le rapport téléchargé, le nom du rapport, et le répertoire `hashes/` de ce dépôt cloné.

```bash
RAPPORTS_DIR="$HOME/Téléchargements"
RAPPORT="2025-11-11_18-32-29_verification-des-ports-ouverts.pdf"
HASHES_DIR="$HOME/audit-hashes/hashes"
```

Laisser ensuite `sha256sum` recalculer l'empreinte et la comparer à celle publiée. La commande s'exécute depuis le répertoire du rapport, d'où le sous-shell `cd` (qui n'affecte pas le shell courant) :

```bash
(cd "$RAPPORTS_DIR" && sha256sum -c "$HASHES_DIR/$RAPPORT.sha256")
```

Si le rapport est intègre :

```text
2025-11-11_18-32-29_verification-des-ports-ouverts.pdf: Réussi
```

S'il a été modifié depuis la publication de son haché :

```text
2025-11-11_18-32-29_verification-des-ports-ouverts.pdf: Échec
sha256sum: Attention : 1 somme de contrôle ne correspond pas
```

Le code de retour est `0` en cas de succès et non nul en cas d'échec, ce qui permet d'intégrer la vérification à un script.

## Remarques
- Ce dépôt ne contient **pas** les rapports eux-mêmes, uniquement leurs empreintes SHA-256.
- L'objectif est de pouvoir prouver l'**intégrité** et l'**antériorité** des rapports d'audit en s'appuyant sur un dépôt public et horodaté.
