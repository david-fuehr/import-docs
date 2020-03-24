---
title: 'Product Links'
taxonomy:
    category:
        - docs
visible: true
---

M2IF provides the functionality to import product links as well as their positions. Actually *related*, *upsell* and *crosssell* links are supported. As M2IF also supports the import of *grouped* products, which are nothing else than the additional link type `super`, it is possible to import the position of the products that are linked to a grouped product.  

### Link Positions



### Magento 2 CE < 2.1.6

Magento 2 CE supports positions for product links, as well as Magento 2 EE. By default, up to version 2.1.6, importing product positions is **NOT** possible in the CE, because the database of the CE lack's of missing rows in the `catalog_product_link_attribute` table.

In case, that the rows are not available, the positions, defined in the CSV file's columns 

* `related_position`
* `crosssell_position`
* `upsell_position`

will be ignored.

To enable importing positions, add the following rows the Magento 2 CE database

```sql
INSERT INTO 
        `catalog_product_link_attribute` (
            `link_type_id`, 
            `product_link_attribute_code`, 
            `data_type`
        ) 
    VALUES
        (1,'position','int'),
        (4,'position','int'),
        (5,'position','int');
```

> Make sure, that the values are **NOT** already available, before adding them!