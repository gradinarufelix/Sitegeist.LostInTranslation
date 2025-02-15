# Sitegeist.LostInTranslation
## Automatic Translations for Neos via DeeplApi

Documents and Contents are translated automatically once editors choose to "create and copy" a version in another language.
The included DeeplService can be used for other purposes aswell. 

The initial development started as a pr for https://github.com/code-q-web-factory/CodeQ.DeepLTranslationHelper but developed
into a seperate package.

### Authors & Sponsors

* Martin Ficzel - ficzel@sitegeist.de

*The development and the public-releases of this package is generously sponsored
by our employer http://www.sitegeist.de.*

## Installation

Sitegeist.LostInTranslation is available via packagist. Run `composer require sitegeist/lostintranslation`.

We use semantic-versioning so every breaking change will increase the major-version number.

## How it works

By default all inline editable properties are translated using DeepL (see Setting `translateInlineEditables`). 
To include other `string` properties into the automatic translation the `options.translateOnAdoption: true` 
can be used in the property configuration. 

Some very common fields from `Neos.Neos:Document` are already configured to do so by default.

```
'Neos.Neos:Document':
  properties:
    title:
      options:
        translateOnAdoption: true
    titleOverride:
      options:
        translateOnAdoption: true
    metaDescription:
      options:
        translateOnAdoption: true
    metaKeywords:
      options:
        translateOnAdoption: true
```

## Configuration 

This package needs an authenticationKey for the DeeplL Api from https://www.deepl.com/pro-api. 
There are free plans that support a limited number but for productive use we recommend using a payed plan.  

```
Sitegeist:
  LostInTranslation:
    DeepLApi:
      authenticationKey: '.........................'
```

The translation of nodes can is configured via Setting:

```
Sitegeist:
  LostInTranslation:
    nodeTranslation:
      #
      # Enable the automatic translations of nodes while they are adopted to another dimension
      #
      enabled: true

      #
      # Translate all inline editable fields without further configuration.
      #
      # If this is disabled inline editables can be configured for translation by setting
      # `options.translateOnAdoption: true` for each property seperatly
      #
      translateInlineEditables: true

      #
      # The name of the language dimension. Usually needs no modification
      #
      languageDimensionName: 'language'
```

If a preset of the language dimension uses a locale identifier that is not compatible with DeepL the deeplLanguage can
be configured explicitly for this preset via `options.deeplLanguage`. If this value is set to null the language will neither
be used as source nor as target for translations.  

```
Neos:
  ContentRepository:
    contentDimensions:
      'language':
        presets:
        
          # 
          # Danish uses a different locale identifier then DeepL so the `deeplLanguage` has to be configured explicitly
          #
          'dk':
            label: 'Dansk'
            values: ['dk']
            uriSegment: 'dk'
            options:
              deeplLanguage: 'da'
              
          #    
          # The bavarian language is not supported by DeepL and is disabled
          # 
          'de_bar':
            label: 'Bayrisch'
            values: ['de_bar','de']
            uriSegment: 'de_bar'
            options:
              deeplLanguage: ~
```
## Performance

For every translated node a single request is made to the DeepL API. This can lead to significant delay when Documents with lots of nodes are translated. It is likely that future versions will improve this.

## Contribution

We will gladly accept contributions. Please send us pull requests.
