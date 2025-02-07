# discourse-locale-override-de

Simple plugin/boilerplate to override some of translations keys in Discourse and persist them between Docker rebuilds.
Can be used by translators, early adopters of young locales or communities with specific slang.

This repository is based on lidel/discourse-locale-override, history was rewritten to squash some commits not relevant to this fork, which is `de`-only.

## Installation on top of Docker image

Add to your `/var/discourse/containers/app.yml`:

```ruby
hooks:
  after_code:
    - exec:
        cd: $home/plugins
        cmd:
          - git clone https://github.com/discourse/docker_manager.git
          - git clone https://github.com/fbender/discourse-locale-override-de.git
```

Rebuild Discourse: `/var/discourse/launcher rebuild app`

## FAQ

### How can I use this to override locale X?

1. Fork this repository
2. Remove `config/locales/*.de.yml` files *(optional)*
3. Add `config/locales/*.xx_XX.yml` ones    
  **Warning:** `xx_XX` should be the language code you want to *override*.    
  Files with this locale code must be already present upstream at  [discourse/config/locales](https://github.com/discourse/discourse/tree/master/config/locales).    
  This is important: adding non-existing locale codes will *break your instance*.
4. **Crucial:** make sure your `.yml` files are valid.    
  Test them at the [YAML Validator](http://www.yamllint.com) before you commit them to your repository.
5. Update repository URL in Docker hook to point to your fork

### Where do I get original `.yml` files from?

There are two places I am aware of:
- Github: https://github.com/discourse/discourse/tree/master/config/locales
- Transifex: https://www.transifex.com/projects/p/discourse-org/

If you care about latest translations use Transifex.
Github is updated with Transifex changes about once a week.

### Can I override entire locale?

Yes, if your `.yml` file has translation for all keys the entire locale with be overridden.

### Can I override only one label?

Yes, but make sure you understand YAML and all key segments used in label key are present.
For example, to override `pl_PL.site_settings.enable_emoji` make sure that `config/locales/server.pl_PL.yml` has these three lines (one for each segment):

```
pl_PL:
  site_settings:
    enable_emoji: "Włącz wyświetlanie Emoji"
```
