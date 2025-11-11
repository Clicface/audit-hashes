# Clicface – Audit Hashes

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

## Remarques
- Ce dépôt ne contient **pas** les rapports eux-mêmes, uniquement leurs empreintes SHA-256.
- L'objectif est de pouvoir prouver l'**intégrité** et l'**antériorité** des rapports d'audit en s'appuyant sur un dépôt public et horodaté.
