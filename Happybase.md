# Happybase

HappyBase is a developer-friendly Python library to interact with Apache HBase. HappyBase is designed for use in standard HBase setups, and offers application developers a Pythonic API to interact with HBase. Below the surface, HappyBase uses the Python Thrift library to connect to HBase using its Thrift gateway, which is included in the standard HBase 0.9x releases.

how to install happybase into the MacOS:
>sudo pip install happybase

```php
➜ /Users/fanlj/Desktop/hbase-0.98.20-hadoop2/bin >./hbase-daemon.sh start thrift
```

```php
import happybase

connection = happybase.Connection('mf')
table = connection.table('tt')

table.put('row-key', {'ff:qual1': 'value1',
                      'ff:qual2': 'value2'})

row = table.row('row-key')
print row['ff:qual1']  # prints 'value1'

for key, data in table.rows(['row-key-1', 'row-key-2']):
    print key, data  # prints row key and data for each row

for key, data in table.scan(row_prefix='row'):
    print key, data  # prints 'value1' and 'value2'

# row = table.delete('row-key')
```

Test the port service on 9090 where the thrift run

```php

netstat -nl | grep 9090

```