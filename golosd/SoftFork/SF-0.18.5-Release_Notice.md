# GOLOS·CORE
# The new version SoftFork 0.18.5

***Golos·Core announced the SoftFork 0.18.5 version release which eliminated previously existing defects in the system.  
This page describes a detailed outline of the fixes.***  


**Reindexing:**  
***SF 0.18.5 requires reindexing from all previous versions except version SF 0.18.4.***  

### Defects fixed in this release  

Defect (Task№) | Abstract  
:------------------------------- |:----------------  
943                         | Problem of creating a list of operations for operation_history plugin  
936                                 | The `get_ops_in_block` method, unlike the `get_block` method, did not provide information about virtual operations in block  


<!-- toc -->  
****  
## Problem of creating a list of operations for operation_history plugin  
**Bug description**  
Some plugins, including `operation_history`, had difficulty setting options (keys).  

For example, executing a command of the form “./golosd --<some-key>=123” for keys `history-blocks`, `history-start-block` failed with the error message:  
“Error parsing command line: option --history-blocks is ambiguous and matches different versions of --history-blocks”.  

The error occurred when trying to run this command also with some other keys available in `program_options`.  


**Solution**  
The `add_option` method has been improved. Each of the plugins contains two objects `program_options`, this is either `cli` (command line options) or `cfg` (config file options). To declare a parameter, it has to be added to the `cfg` object with a required value.   

The error cause was an additional declaration of parameters in `cli`, which caused duplicates in general list of parameters. The solution was based on removing the parameter declaration from the `cli` object.  

The change is made to the following plugins:  
  * `account_history`;  
  * `debug_node`;  
  * `follow`;  
  * `market_history`;  
  * `operation_history`;  
  * `statsd`;  
  * `witness`.  



## The get_ops_in_block method, unlike the get_block method, did not provide information about virtual operations in block

**Bug description:**  

The `get_ops_in_block` method did not provide information about virtual operations in a block in the return value.

**Solution:**  

It was decided to generate the return value of the `get_ops_in_block` method similar to the return value of the `get_block` method. These two methods become identical in structure in this release. The `operation_history` plugin has been supplemented with the `get_block_with_virtual_ops` method.  


**Samples**  
1. The sample method request is as follows:  
```javascript
var func = function (err, result) {
    ...
};
golos.api.send("operation_history",{"method": 
    "get_block_with_virtual_ops", "params":[<block>]}, func);
```
The <block> parameter is a block number.  

2. The sample response of the `get_ops_in_block` method is as follows:
```javascript
"previous": "00...00",
"timestamp": "2018-09-01T00:01:22",
"witness": "",
"transaction_merkle_root": "00...00",
"extensions": [],
"witness_signature": "00...00",
"transactions": [],
"block_id": "0000000000000000000000000000000000000000",
"signing_key": "GLS1111111111111111111111111111111114T1Anm",
"transaction_ids": []
"_virtual_operations": [{
    "trx_in_block": 65535,
    "op_in_trx": 0,
    "virtual_op": 1,
    "op": [
        "producer_reward",
        …
    ]
}]
```
**Notes:**  
  * The `_virtual_operations` field is optional and can be omitted if here are no virtual operations in the block. There is no more difference between responses of the methods.  
  * Also in the plugin `database_api` modified method `set_block_applied_callback`. From now on, its response has the `_virtual_operations` field as well. This change affects API node only.  


****
