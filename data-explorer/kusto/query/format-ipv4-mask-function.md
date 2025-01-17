---
title: format_ipv4_mask() - Azure Data Explorer
description: Learn how to use the format_ipv4_mask() function to parse the input with a netmask and return a string representing the IPv4 address in CIDR notation.
ms.reviewer: alexans
ms.topic: reference
ms.date: 12/12/2022
---
# format_ipv4_mask()

Parses the input with a netmask and returns a string representing the IPv4 address in CIDR notation.

```kusto
print format_ipv4_mask('192.168.1.255', 24) == '192.168.1.0/24'
print format_ipv4_mask(3232236031, 24) == '192.168.1.0/24'
```

## Syntax

`format_ipv4_mask(`*Expr* [`,` *PrefixMask*`])`

## Arguments

* *`Expr`*: A string or number representation (in big-endian order) of the IPv4 address as CIDR notation.
* *`PrefixMask`*: (Optional) An integer from 0 to 32 representing the number of most-significant bits that are taken into account. If argument isn't specified, all bit-masks are used (32).

## Returns

If conversion is successful, the result will be a string representing IPv4 address as CIDR notation.
If conversion isn't successful, the result will be an empty string.

## See also

* [format_ipv4()](format-ipv4-function.md): For IPv4 address formatting without CIDR notation.
* [IPv4 and IPv6 functions](scalarfunctions.md#ipv4ipv6-functions)

## Examples

<!-- csl: https://help.kusto.windows.net/Samples -->
```kusto
datatable(address:string, mask:long)
[
 '192.168.1.1', 24,          
 '192.168.1.1', 32,          
 '192.168.1.1/24', 32,       
 '192.168.1.1/24', long(-1), 
]
| extend result = format_ipv4(address, mask), 
         result_mask = format_ipv4_mask(address, mask)
```

|address|mask|result|result_mask|
|---|---|---|---|
|192.168.1.1|24|192.168.1.0|192.168.1.0/24|
|192.168.1.1|32|192.168.1.1|192.168.1.1/32|
|192.168.1.1/24|32|192.168.1.0|192.168.1.0/24|
|192.168.1.1/24|-1|||
