# Site de Grosso Modo

Site statique, sans dépendance ni étape de build, publié sur GitHub Pages :
**https://leriom.github.io/grossomodo-site/** (dépôt public `LERIOM/grossomodo-site`).
La source est le dossier `site/` du dépôt de l'app ; le dépôt public n'en est qu'une copie.

| Fichier | Rôle |
|---|---|
| `index.html` | Accueil : logotype, sous-titre, 3 arguments, bouton App Store, capture |
| `confidentialite.html` | Politique de confidentialité (URL exigée par App Store Connect et liée dans l'app) |
| `support.html` | FAQ et contact (« URL d'assistance » d'App Store Connect) |
| `style.css` | Seule feuille de style, clair/sombre via `prefers-color-scheme` |
| `img/` | `fourchette.png` (capture App Store 01 réduite), icônes |
| `.nojekyll` | Sert les fichiers tels quels (sans Jekyll, qui ignorerait `.well-known/`) |
| `.well-known/apple-app-site-association` | Liens universels (voir plus bas) |
| `_headers` | En-têtes pour Cloudflare Pages ou Netlify ; sans effet sur GitHub Pages |

## Publier

```bash
scripts/publish_site.sh --dry-run   # affiche ce qui serait fait, sans rien envoyer
scripts/publish_site.sh             # crée le dépôt public au besoin, pousse site/, active Pages
```

Le script est idempotent : on le relance après chaque modification de `site/`. Il attend que
`confidentialite.html` réponde 200 (jusqu'à 5 min) puis affiche les URLs.

## À faire quand l'app sera publiée

- `index.html` : remplacer le paragraphe « Bientôt sur l'App Store » par le lien commenté juste au-dessus,
  et décommenter la balise `apple-itunes-app` (identifiant App Store).
- Si des publicités sont ajoutées : réécrire la section conditionnelle de `confidentialite.html` au présent
  et changer la date (voir aussi `docs/ADMOB.md` et STORE.md §5).

## Liens universels : seulement sur grossomodo.app

`.well-known/apple-app-site-association` déclare l'app `HBYP7C9D4H.com.grossomodo.app` pour `/play`, `/d`
(duel) et `/archive` (docs/UNIVERSAL_LINKS.md). **Il ne sert à rien sur `leriom.github.io/grossomodo-site/`** :
iOS ne lit ce fichier qu'à la racine d'un domaine (`https://<domaine>/.well-known/…`), et l'app ne déclare
que `grossomodo.app`. Il ne servira qu'une fois le domaine `grossomodo.app` acheté et pointé sur ce site
(fichier `CNAME` contenant `grossomodo.app` et DNS chez le registraire, ou hébergement Cloudflare Pages /
Netlify, qui appliquent `_headers` et servent ce fichier en `application/json`), avec l'entitlement
« Associated Domains » côté app. Penser alors à remplacer les URLs `leriom.github.io` dans
`Estime/App/AppLinks.swift`, STORE.md et App Store Connect (avec un domaine personnalisé, GitHub redirige
l'adresse `leriom.github.io` vers lui : les anciens liens continuent de fonctionner).
