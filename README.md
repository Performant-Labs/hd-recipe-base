## HeroDevs Base Recipe
Other recipes use this one for common features.

This recipe is designed to:
- Set up typical modules
- Install Drush
- Not have a specific theme (that's for downstream recipes).

## Documentation

[Instructions on Google](https://docs.google.com/document/d/1OVe_jwPZNUWt7ovWD2XXcSPmCf0YrnypocMIVIrIqrQ/edit)

## Installation Instructions

- Once Recipes goes into core, these instructions will become much simpler.
- Start with a fresh Drupal 10+ site (recipes are not guaranteed to work on existing sites):
```
composer create-project drupal/recommended-project your_new_site
```

- Allow composer.json to recognize recipes:
```
lando composer require oomphinc/composer-installers-extender
lando composer config allow-plugins.oomphinc/composer-installers-extender
lando composer config extra.installer-types --merge --json '["drupal-recipe"]'
lando composer config extra.installer-paths --merge --json '{"web/recipes/contrib/{$name}": ["type:drupal-recipe"]}'
lando composer config minimum-stability dev
```

- Allow patches and add the recipe patch to composer.json so that Drupal can work with recipes.
```
  lando composer require cweagans/composer-patches
  lando composer config allow-plugins.cweagans/composer-patches true
  lando composer config extra.patches --merge --json '{"drupal/core": {"Allow recipes to be applied": "https://git.drupalcode.org/project/distributions_recipes/-/raw/patch/recipe-10.2.x.patch"}}'
  lando composer update drupal/core
```

If you have a problem with patching, use:
brew install gpatch

- Add the unpack tool:
```
  lando composer config allow-plugins.ewcomposer/unpack true
  lando composer config repositories.recipe-unpack vcs https://gitlab.ewdev.ca/yonas.legesse/drupal-recipe-unpack.git
  lando composer require ewcomposer/unpack:1.x-dev@dev
```

- Allow composer.json to recognize the Github repo with the recipe:

```
  lando composer config repositories.hd-recipe-base vcs https://github.com/herodevs/hd-recipe-base
```
- Add the recipe to your site:
```
  lando composer require herodevs/hd-recipe-base
```
- Update dependencies:
```
  lando composer update
```

- Apply the recipe. Note that `core/scripts/drupal` must be
executable with `chmod +x`:
```
  cd web
  chmod +x core/scripts/drupal
```


```shell
  php core/scripts/drupal recipe recipes/contrib/hd-recipe-base
```

You should see:
```
[OK] HD Base applied successfully
```
Clear the cache (`drush cr` or use the UI) and visit the site.
