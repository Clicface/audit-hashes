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

Comparer ensuite l'empreinte recalculée localement avec l'empreinte publiée :

```bash
diff <(sha256sum "$RAPPORTS_DIR/$RAPPORT" | awk '{print $1}') \
     <(awk '{print $1}' "$HASHES_DIR/$RAPPORT.sha256") \
  && echo "OK : rapport intègre"
```

- Aucune sortie suivie de `OK : rapport intègre` → le rapport est identique à celui publié.
- Un affichage des deux empreintes (et un code de retour `1`) → le rapport a été modifié.

La comparaison ne porte que sur l'empreinte (premier champ), afin que le chemin complet du rapport local n'interfère pas avec le nom de fichier nu stocké dans le `.sha256`.

Équivalent avec l'option de vérification intégrée à `sha256sum`, qui doit s'exécuter depuis le répertoire du rapport :

```bash
(cd "$RAPPORTS_DIR" && sha256sum -c "$HASHES_DIR/$RAPPORT.sha256")
```

```text
2025-11-11_18-32-29_verification-des-ports-ouverts.pdf: Réussi
```

## Remarques
- Ce dépôt ne contient **pas** les rapports eux-mêmes, uniquement leurs empreintes SHA-256.
- L'objectif est de pouvoir prouver l'**intégrité** et l'**antériorité** des rapports d'audit en s'appuyant sur un dépôt public et horodaté.
