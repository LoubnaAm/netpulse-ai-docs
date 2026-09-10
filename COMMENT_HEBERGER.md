# Comment mettre ce site en ligne (gratuitement)

Ce dossier est un projet **Mintlify** prêt à l'emploi : c'est le même type d'outil qui fait tourner des sites de documentation comme docs.claude.com.

Mintlify héberge le site gratuitement, sans serveur à gérer. Voici les étapes les plus simples, sans ligne de commande si possible.

## Option A — La plus simple (via le site web, sans terminal)

1. Va sur **https://mintlify.com** et crée un compte (avec ton compte GitHub, c'est le plus rapide).
2. Crée un nouveau dépôt (repository) sur **GitHub** (par exemple `netpulse-ai-docs`), et mets-y tous les fichiers de ce dossier (`docs.json`, `index.mdx`, etc.) à la racine.
   - Sur GitHub, tu peux faire "Add file" → "Upload files" et glisser-déposer tous les fichiers, pas besoin de Git en ligne de commande.
3. Dans le tableau de bord Mintlify, clique sur **"New Project"** puis **connecte ce dépôt GitHub**.
4. Mintlify détecte automatiquement `docs.json` et publie le site.
5. Tu obtiens une adresse du type `https://ton-projet.mintlify.app`, modifiable ensuite avec un nom de domaine personnalisé si tu veux (ex. `docs.tonentreprise.com`).

À chaque fois que tu modifies un fichier `.mdx` sur GitHub, le site se met à jour automatiquement.

## Option B — Avec le terminal (pour prévisualiser en local d'abord)

```bash
npm install -g mintlify
cd netpulse-ai-docs        # ce dossier
mintlify dev
```

Ça ouvre une prévisualisation du site sur `http://localhost:3000`. Une fois content du rendu, pousse le dossier sur GitHub (étape 2 ci-dessus) et connecte-le sur mintlify.com pour l'hébergement.

## Personnaliser

- **Couleurs** : dans `docs.json`, section `"colors"`.
- **Logo** : remplace les fichiers dans `/logo` (facultatif, sinon Mintlify affiche un logo par défaut).
- **Contenu** : chaque page est un simple fichier `.mdx` (texte + composants visuels comme `<Card>`, `<Steps>`, `<Accordion>`) — modifiable sans coder.
