---
title: 'Core Components'
visible: true
---

This page lists the main components, that provides Magento 2 import core functionality to import products, categories and attributes.

### General (Independent from Edition)

* [import-app-simple](https://github.com/techdivision/import-app-simple) - Application implementation that uses Symfony Console + DI as well as M2IF to provide Magento 2 CE/EE import functionality
* [import-configuration-jms](https://github.com/techdivision/import-configuration-jms) - A [JMS](https://github.com/schmittjoh/serializer) based M2IF configuration implementation

### Components for Community Edition (CE)

These are the M2IF core components for the Magento 2 Community Edition (CE).

* [import-product](https://github.com/techdivision/import-product) - Provides Product Import functionality
* [import-product-url-rewrite](https://github.com/techdivision/import-product-url-rewrite) - Provides Product URL Rewrite Import functionality
* [import-product-bundle](https://github.com/techdivision/import-product-bundle) - Provides Bundle Product Import functionality
* [import-product-link](https://github.com/techdivision/import-product-link) - Provides product relation import functionality
* [import-product-media](https://github.com/techdivision/import-product-media) - Provides Product Image Import functionality
* [import-product-variant](https://github.com/techdivision/import-product-variant) - Provides configurable Product Import functionality
* [import-category](https://github.com/techdivision/import-category) - Provides Category Import functionality
* [import-attribute](https://github.com/techdivision/import-attribute) - Provides Attribute Import functionality
* [import-customer](https://github.com/techdivision/import-customer) - Provides Customer Import functionality
* [import-customer-address](https://github.com/techdivision/import-customer-address) - Provides Customer Address Import functionality

> Components like import-attribute will also work with the EE, so there is not separate implementation.

### Components for Enterprise Edition (EE)

These are the M2IF core components for the Magento 2 Enterprise Edition (EE).

* [import-ee](https://github.com/techdivision/import-ee) - Provides core import functionality for Magento 2 EE
* [import-product-ee](https://github.com/techdivision/import-product-ee) - Provides product import functionality for Magento 2 EE
* [import-product-bundle-ee](https://github.com/techdivision/import-product-bundle-ee) - Provides bundle product import functionality for Magento 2 EE
* [import-product-link-ee](https://github.com/techdivision/import-product-link-ee) - Provides product import relation functionality for Magento 2 EE
* [import-product-media-ee](https://github.com/techdivision/import-product-media-ee) - Provides product import image functionality for Magento 2 EE
* [import-product-variant-ee](https://github.com/techdivision/import-product-variant-ee) - Provides configurable product import functionality for Magento 2 EE
* [import-category-ee](https://github.com/techdivision/import-category-ee) - Provides category import functionality for Magento 2 EE