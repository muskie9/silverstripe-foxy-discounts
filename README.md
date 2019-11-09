# SilverStripe Foxy Discounts

Offer discounts on purchase conditions for your Foxy products.

[![Build Status](https://travis-ci.org/dynamic/silverstripe-foxy-discounts.svg?branch=master)](https://travis-ci.org/dynamic/silverstripe-foxy-discounts)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/dynamic/silverstripe-foxy-discounts/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/dynamic/silverstripe-foxy-discounts/?branch=master)
[![Build Status](https://scrutinizer-ci.com/g/dynamic/silverstripe-foxy-discounts/badges/build.png?b=master)](https://scrutinizer-ci.com/g/dynamic/silverstripe-foxy-discounts/build-status/master)
[![codecov](https://codecov.io/gh/dynamic/silverstripe-foxy-discounts/branch/master/graph/badge.svg)](https://codecov.io/gh/dynamic/silverstripe-foxy-discounts)

[![Latest Stable Version](https://poser.pugx.org/dynamic/silverstripe-foxy-discounts/v/stable)](https://packagist.org/packages/dynamic/silverstripe-foxy-discounts)
[![Total Downloads](https://poser.pugx.org/dynamic/silverstripe-foxy-discounts/downloads)](https://packagist.org/packages/dynamic/silverstripe-foxy-discounts)
[![Latest Unstable Version](https://poser.pugx.org/dynamic/silverstripe-foxy-discounts/v/unstable)](https://packagist.org/packages/dynamic/silverstripe-foxy-discounts)
[![License](https://poser.pugx.org/dynamic/silverstripe-foxy-discounts/license)](https://packagist.org/packages/dynamic/silverstripe-foxy-discounts)

## Requirements

* SilverStripe ^4.0
* SilverStripe Foxy ^1.0

## Installation

```
composer require dynamic/silverstripe-foxy-discounts
```

## License
See [License](license.md)

## Example configuration

Add the following extensions to your product classes:

```yaml

Dynamic\Foxy\Model\Setting:
  extensions:
    - Dynamic\Foxy\Discounts\Extension\FoxyAdminExtension

Dynamic\Products\Page\Product:
  extensions:
    - Dynamic\Foxy\Discounts\Extension\ProductDataExtension

PageController:
  extensions:
    - Dynamic\Foxy\Discounts\Extension\PageControllerExtension
  
```

Create a data extension to add a `many_many` relation to your base product class(es):

```php
<?

namespace {
    use SilverStripe\ORM\DataExtension;
    use Dynamic\Products\Page\Product;
    
    class DiscountDataExtension extends DataExtension
    {
        private static $many_many = [
            'Products' => Product::class,
        ];
        
        public function updateCMSFields(FieldList $fields)
        {
            if ($this->owner->ID) {
                // Products
                $field = $fields->dataFieldByName('Products');
                $config = $field->getConfig();
                $config
                    ->removeComponentsByType([
                        GridFieldAddExistingAutocompleter::class,
                        GridFieldAddNewButton::class,
                        GridFieldArchiveAction::class,
                    ])
                    ->addComponents([
                        new GridFieldAddExistingSearchButton(),
                    ]);
            }
        }
    }
}       

```

And apply to `Discount` in `foxy.yml`:

```yaml
Dynamic\Foxy\Discounts\Model\Discount:
  extensions:
    - DiscountDataExtension
```

## Maintainers
*  [Dynamic](http://www.dynamicagency.com) (<dev@dynamicagency.com>)
 
## Bugtracker
Bugs are tracked in the issues section of this repository. Before submitting an issue please read over 
existing issues to ensure yours is unique. 
 
If the issue does look like a new bug:
 
 - Create a new issue
 - Describe the steps required to reproduce your issue, and the expected outcome. Unit tests, screenshots 
 and screencasts can help here.
 - Describe your environment as detailed as possible: SilverStripe version, Browser, PHP version, 
 Operating System, any installed SilverStripe modules.
 
Please report security issues to the module maintainers directly. Please don't file security issues in the bugtracker.
 
## Development and contribution
If you would like to make contributions to the module please ensure you raise a pull request and discuss with the module maintainers.
