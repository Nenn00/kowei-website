# kowei-website

Site officiel de KOWEI : un site statique Jekyll, hébergeable sur GitHub Pages.

- Domaine principal : `kowei.app`
- Domaine secondaire : `kowei.fr` (redirection prévue vers `kowei.app`)
- Aucun JavaScript, aucun cookie, aucune ressource externe, aucun outil de suivi.

## Pages

| URL            | Langue | Fichier source                    |
|----------------|--------|-----------------------------------|
| `/`            | FR     | `index.html`                      |
| `/legal/`      | FR     | `_legal/fr/mentions-legales.md`   |
| `/privacy/`    | FR     | `_legal/fr/confidentialite.md`    |
| `/terms/`      | FR     | `_legal/fr/cgu.md`                |
| `/en/`         | EN     | `en/index.html`                   |
| `/en/legal/`   | EN     | `_legal/en/legal-notice.md`       |
| `/en/privacy/` | EN     | `_legal/en/privacy.md`            |
| `/en/terms/`   | EN     | `_legal/en/terms.md`              |
| `/404.html`    | FR/EN  | `404.html`                        |

## Structure

```
_config.yml          Configuration Jekyll
_data/i18n.yml       Libellés d'interface FR / EN
_layouts/            Gabarits (default, legal)
_includes/           En-tête, pied de page, <head>
_legal/fr|en/        Textes juridiques (un fichier Markdown par page)
assets/              CSS et favicon
CNAME                Domaine personnalisé (pris en compte à la publication)
```

## Modifier un texte juridique

1. Ouvrir le fichier concerné dans `_legal/fr/` ou `_legal/en/`.
2. Remplacer les mentions **[À COMPLÉTER : …]** / **[TO BE COMPLETED: …]**.
3. Ne pas modifier `permalink` ni `alt_url` : ils fixent l'URL de la page et le lien vers sa traduction.
4. Facultatif : décommenter `updated: AAAA-MM-JJ` pour afficher la date de mise à jour.

Lister les emplacements restant à compléter :

```sh
grep -rn "À COMPLÉTER\|TO BE COMPLETED\|TO BE CONFIRMED" _legal
```

## Statut provisoire des pages juridiques

Chaque page juridique contient `provisional: true` dans son en-tête. Tant que cette valeur est présente :

- un bandeau « Document provisoire » / « Provisional document » est affiché ;
- la page porte `<meta name="robots" content="noindex, nofollow">`.

### À faire avant la publication officielle

Pour chacun des six fichiers de `_legal/`, et seulement après relecture du texte définitif :

- [ ] vérifier qu'il ne reste aucun emplacement « À COMPLÉTER » / « TO BE COMPLETED » / « TO BE CONFIRMED » ;
- [ ] remplacer `provisional: true` par `provisional: false` ;
- [ ] renseigner `updated:` avec la date de la version définitive.

Le fichier `robots.txt` ne bloque volontairement pas ces pages : les moteurs doivent pouvoir les lire pour voir la balise `noindex`.

## Aperçu en local

Prérequis : Ruby ≥ 3.0 et Bundler. Le Ruby fourni avec macOS (2.6) est trop ancien. Par exemple : `brew install ruby`.

```sh
cd kowei-website
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve
```

Puis ouvrir <http://127.0.0.1:4000/>.

## Publication (ne pas faire sans validation)

1. Créer le dépôt GitHub `kowei-website`, puis faire le premier push.
2. Dans GitHub, aller dans **Settings → Pages → Source** : branche `main`, dossier `/ (root)`.
3. Dans **Settings → Pages → Custom domain**, saisir `kowei.app`. Le fichier `CNAME` est déjà présent.
4. Côté OVH, dans la zone DNS de `kowei.app`, ajouter :
   - des enregistrements `A` pour `@` vers `185.199.108.153`, `185.199.109.153`, `185.199.110.153` et `185.199.111.153` ;
   - des enregistrements `AAAA` pour `@` vers `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153` et `2606:50c0:8003::153` ;
   - un enregistrement `CNAME` pour `www` vers `<utilisateur-github>.github.io.`

   Ces valeurs sont à revérifier dans la documentation GitHub Pages au moment de la configuration.
5. Une fois le certificat émis, cocher **Enforce HTTPS**.
6. Recommandé : vérifier le domaine dans les paramètres GitHub (**Settings → Pages → Verified domains**).

### kowei.fr

GitHub Pages n'accepte qu'un domaine personnalisé par dépôt. `kowei.fr` devra être redirigé en 301 vers `https://kowei.app`, via OVH ou un autre service. La prise en charge du HTTPS sur `kowei.fr` est à vérifier selon la solution retenue.
