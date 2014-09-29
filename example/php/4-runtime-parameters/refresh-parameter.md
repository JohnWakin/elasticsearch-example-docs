# PHP Runtime Refresh Parameter

Using `'refresh' => true` allows the Document to be Searchable immediately. However, note the performance hit. If you require just to use a `get` on the new Document, this is available instantly without forcing a refresh.

```php
$client = new \Elasticsearch\Client();

$params = array(
    'refresh' => true, // force refresh of index, data immediately available
    'index'   => 'index-name',
    'type'    => 'type-of-document',
    'body'    => array(
        'field' => 'value'
    )
);

$client->index($params);
```
