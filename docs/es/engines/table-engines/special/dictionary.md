---
machine_translated: true
machine_translated_rev: 72537a2d527c63c07aa5d2361a8829f3895cf2bd
toc_priority: 35
toc_title: Diccionario
---

# Diccionario {#dictionary}

El `Dictionary` el motor muestra el [diccionario](../../../sql-reference/dictionaries/external-dictionaries/external-dicts.md) datos como una tabla ClickHouse.

Como ejemplo, considere un diccionario de `products` con la siguiente configuración:

``` xml
<dictionaries>
<dictionary>
        <name>products</name>
        <source>
            <odbc>
                <table>products</table>
                <connection_string>DSN=some-db-server</connection_string>
            </odbc>
        </source>
        <lifetime>
            <min>300</min>
            <max>360</max>
        </lifetime>
        <layout>
            <flat/>
        </layout>
        <structure>
            <id>
                <name>product_id</name>
            </id>
            <attribute>
                <name>title</name>
                <type>String</type>
                <null_value></null_value>
            </attribute>
        </structure>
</dictionary>
</dictionaries>
```

Consultar los datos del diccionario:

``` sql
SELECT
    name,
    type,
    key,
    attribute.names,
    attribute.types,
    bytes_allocated,
    element_count,
    source
FROM system.dictionaries
WHERE name = 'products'
```

``` text
┌─name─────┬─type─┬─key────┬─attribute.names─┬─attribute.types─┬─bytes_allocated─┬─element_count─┬─source──────────┐
│ products │ Flat │ UInt64 │ ['title']       │ ['String']      │        23065376 │        175032 │ ODBC: .products │
└──────────┴──────┴────────┴─────────────────┴─────────────────┴─────────────────┴───────────────┴─────────────────┘
```

Puede usar el [dictGet\*](../../../sql-reference/functions/ext-dict-functions.md#ext_dict_functions) función para obtener los datos del diccionario en este formato.

Esta vista no es útil cuando necesita obtener datos sin procesar o cuando `JOIN` operación. Para estos casos, puede usar el `Dictionary` motor, que muestra los datos del diccionario en una tabla.

Sintaxis:

``` sql
CREATE TABLE %table_name% (%fields%) engine = Dictionary(%dictionary_name%)`
```

Ejemplo de uso:

``` sql
create table products (product_id UInt64, title String) Engine = Dictionary(products);
```

      Ok

Echa un vistazo a lo que hay en la mesa.

``` sql
select * from products limit 1;
```

``` text
┌────product_id─┬─title───────────┐
│        152689 │ Some item       │
└───────────────┴─────────────────┘
```

[Artículo Original](https://clickhouse.tech/docs/en/operations/table_engines/dictionary/) <!--hide-->
