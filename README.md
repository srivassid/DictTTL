<h2>Dictionary with expiring keys</h2>

<h4>A dictionary with a ttl attached to the keys which disappear after a particular time. Includes dict(), OrderedDict() and defaultDict(list)</h4>



Can be installed by typing <b>pip install dictttl</b>



Regular dict and orderedDict have data format defined as <b>{key:(timestamp, value)}</b> whereas defaultDict(list) has it's data format defined as 
<b>{key:[(timestamp,[value1, value2])]}</b>

It it to be mentioned that once the TTL attached to the keys expire, they are not automatically removed, but are kept in memory. Only once <i>_purge()</i> or <i>len(dict)</i> is called are the expired items purged. This also happens while setting a new item, getting the keys and values and items.


<h3>How it works</h3>

To import regular dictionary with keys that have TTL, type
```
from DictTTL_class import DictTTL
```

To import OrdereDict with keys that have TTL, type
```
from OrderedDictTTL_class import OrderedDictTTL
```

To import defaultDict(list) with keys that have TTL, type 
```
from DefaultDictTTL_class import DefaultDictTTL
```


<h4>Add data</h4>

To add data to regular dict, type
```
data = {k: v for k, v in zip([random.randint(1, 10) for x in range(1, 10)], [random.randint(1, 10) for x in range(1, 10)])}
dict_ttl = DictTTL(30, data)
```

This will initialize the regular dict as following
```
<TTLDict@0x7fb427545400; ttl=30, Dict={3: (1600065116.1471431, 6), 9: (1600065116.1471522, 9), 2: (1600065116.1471546, 6), 4: (1600065116.1471567, 3), 1: (1600065116.1471596, 10), 5: (1600065116.1471632, 6)};>
```

To add data to OrderedDict, type 
```
data = {k:v for k,v in zip([random.randint(1,10000) for x in range(1,10000)],[random.randint(1,10000) for x in range(1,10000)])}
dict_ttl = OrderedDictTTL(20, data)
```
This will initialize the OrderedDict as following
```
<TTLDict@0x7fdc08a146a8; ttl=30, Dict=OrderedDict([(3, (1600065660.0748808, 8)), (1, (1600065660.074886, 3)), (8, (1600065660.074888, 7)), (7, (1600065660.0748894, 9)), (5, (1600065660.074891, 4)), (6, (1600065660.0748925, 8))]);>

```

To add data to defaultDict(list), type
```
dict_ttl = DefaultDictTTL(10)
[dict_ttl.append_values(k, v) for k, v in zip([random.randint(1, 5) for x in range(1, 10)], [random.randint(1, 5) for x in range(1, 10)])]
```
This will initialize the defaultDict(list) as following
```
<DefaultDictTTL@0x7f69b7f15348; ttl=3, Dict=defaultdict(<class 'list'>, {2: [(1600065724.8505137, [1, 3])], 3: [(1600065724.8505237, [5, 4, 1])], 4: [(1600065724.8505328, [2])], 5: [(1600065724.8505354, [5, 3])], 1: [(1600065724.8505404, [1])]});>

```

This syntax makes sure every key has the same TTL provided in the DictTTL(), OrderedDictTTL() or DefaultDictTTL() function.


<h4> Set and Get </h4>
To set a value for regular dict and OrderedDict, type
```
dict[key] = value
dict_ttl[2] = 10
```
For defaultDict(list), you can do so by
```
dict_ttl[2] = [10] # or [10,20]
```
Note that initializing a key with this does not remove the old value, it appends to it. 
To set a new value, delete the key first by ```del dict_ttl[2]``` and then set a new value.


To get the value for any type of dict, type
```
print(dict[key])
print(dict_ttl[2] = 10)
>>10
```


<h4> keys(), values() and items()</h4>
To get the keys of any type of dict, type
```
dict_ttl.keys()
```
The result would be (for regularDict, orderedDict and defaultDict(list), in order)
```
dict_keys([8, 3, 4, 9, 6, 5])
odict_keys([7, 8, 6, 9, 10])
dict_keys([4, 5, 1, 2])
```

To get the values of any type of dict, type 
```
dict_ttl.values()
```
This would give you the values with TTL of each respective key
```
[(1600066853.5221877, 2), (1600066853.522191, 9), (1600066853.5221927, 5), (1600066853.5221937, 10), (1600066853.5221949, 1), (1600066853.522196, 10), (1600066853.5221975, 9), (1600066853.5221984, 3)]
[(1600066853.522308, 4), (1600066853.5223105, 6), (1600066853.5223117, 5), (1600066853.5223129, 8), (1600066853.522314, 3), (1600066853.5223153, 2)]
[[(1600066826.5223906, [2])], [(1600066826.5223942, [2, 4, 2])], [(1600066826.5223973, [2, 3])], [(1600066826.5223985, [2, 5, 1])]]
```

To get the values of any type of dict without TTL values, type
```
dict_ttl.values_without_ttl()
```
This would give you
```
[1, 6, 7, 1, 6, 10, 9]
[10, 3, 6, 1, 8, 5]
[[1, 5], [2, 2], [5, 3, 4, 1], [4]]
```

To get the items (both keys and values) of any type of dict, type
```
dict_ttl.items()
```
This would give you the key, value pair without the timestamp.
```
[(1, 6), (5, 10), (8, 7), (6, 1), (3, 9), (7, 4), (2, 6)]
[(2, 5), (9, 2), (7, 8), (6, 3), (10, 8), (8, 7)]
[(2, [2, 1, 4]), (4, [1, 1]), (5, [1, 4, 4]), (3, [4])]
```

To get the complete representation of the dict, keys with ttl and values, type
```
dict_ttl.ttl_items()
```
This would give you
```
[(10, (1600067051.8508687, 3)), (8, (1600067051.8508747, 5)), (6, (1600067051.8508773, 6)), (7, (1600067051.850879, 8)), (4, (1600067051.8508801, 9)), (1, (1600067051.850882, 4))]
[(9, (1600067051.8510616, 9)), (1, (1600067051.8510656, 1)), (2, (1600067051.851068, 7)), (4, (1600067051.85107, 10)), (6, (1600067051.851072, 8))]
[(1, [(1600067024.8512177, [1, 3, 3])]), (5, [(1600067024.8512259, [5, 3, 2])]), (3, [(1600067024.8512323, [4, 1])]), (4, [(1600067024.851241, [5])])]
```

To loop over keys, items and values, you can do so by
```
for k,v in dict_ttl.items()
    print(k,v)
```
Same goes for keys and values.


<h4> set_ttl(), get_ttl(), is_expired() and expire_at()</h4>

To get the ttl of any key in any type of dict, type
```
dict_ttl.get_ttl(key)
dict_ttl.get_ttl(2)
```
This would give you
```
29.99999451637268
19.999989986419678
9.999979496002197
```

To set the ttl of any key of a dict, type
```
dict_ttl.set_ttl(key, ttl) # ttl in seconds
dict_ttl.set_ttl(2,10)
```

This gives you the output
```
[(1, (1600068321.1366003, 5)), (3, (1600068321.1366034, 6)), (2, (1600068301.136608, 9))]
[(3, (1600068311.1367247, 5)), (1, (1600068311.136729, 2)), (2, (1600068301.136734, 2))]
[(3, [(1600068301.1368098, [3, 1, 5, 3, 2])]), (1, [(1600068301.1368222, [4])]), (2, [(1600068301.1368265, [1, 1, 3])])]
```
Note that in the first output, for regular dict, key 2 has ttl 20 seconds less than other items. This is because the original ttl was 30, and not it is 10. Similarly, for orderedDict it is 10 seconds less as original was 20, and for defaultDict(list) it is the same. 


In the same manner, to manually set the ttl of a key in epoch, you can type
```
dict_ttl.expire_at(key, timestamp) #timestamp in epoch
dict_ttl.expire_at(2, time.time() + 10)
```

This gives the output
```
[(1, (1600069054.70139, 7)), (2, (1600069034.701402, 7)), (3, (1600069054.7013972, 4))]
[(2, (1600069034.701553, (1600069044.7015424, 7))), (1, (1600069044.7015464, 5)), (3, (1600069044.7015486, 1))]
[(3, [(1600069034.701688, [4, 4, 1])]), (1, [(1600069034.7016943, [5])]), (2, [(1600069034.7017086, [3, 2, 4, 1, 1])])]
```
As before with set_ttl, the ttl for regular dict is 20 seconds less, orderedDict is 10 seconds less, and defaultDict(list) is the same.


To check if an item has expired, you can use is_expired(). This returns the key if it has expired, and None if not.
```
dict_ttl.is_expired(key)

dict_ttl.set_ttl(2,10)
print(dict_ttl.is_expired(2))
print(dict_ttl.ttl_items())

time.sleep(11)

print(dict_ttl.is_expired(2))
print(dict_ttl.ttl_items())
```

This gives the output
```
None
[(3, (1600069461.3600748, 5)), (2, (1600069441.3600852, 2)), (1, (1600069461.3600812, 7))]
2
[(3, (1600069461.3600748, 5)), (1, (1600069461.3600812, 7))]
```
We set the ttl for key 2 to 10 seconds. Note that at first, before time.sleep(11), the key has not expired, so is_expired() returns none, and ttl_items() contain the key,value pair of 2.

But after time.sleep(10), is_expired() returns 2, and ttl_items() does not contain the key 2.


<h4>Additional functions</h4>

<b>Dict_union</b>
To perform a union of 2 dicts, you can use the method DictTTL().dict_union(dict1, dict2) (replace DictTTL() with OrderedDictTTL() or DefaultDictTTL()).
It will perform a union and make a new dict that contains elements of both dict1 and dict2.
Not available for DefaultDictTTL.

Example (for DictTTL and OrderedDictTTL):
```
data2 = {k: v for k, v in zip([random.randint(1, 5) for x in range(1, 10)], [random.randint(1, 10) for x in range(1, 10)])}
dict_ttl2 = DictTTL(20, data2)

dict_ttl3 = DictTTL(10,DictTTL(10).dict_union(dict_ttl,dict_ttl2))
print(dict_ttl3)
```

This will give you the output
```
<TTLDict@0x7f9fc1a77320; ttl=30, Dict={5: (1600070209.1329722, 1), 4: (1600070209.1329756, 4), 3: (1600070209.132977, 1), 1: (1600070209.132978, 4)};>
<TTLDict@0x7f9fc1a47da0; ttl=20, Dict={2: (1600070199.1329875, 10), 3: (1600070199.1329885, 3), 5: (1600070199.1329892, 4), 1: (1600070199.13299, 7)};>
<TTLDict@0x7f9fc1a525f8; ttl=10, Dict={5: (1600070189.1331093, 4), 4: (1600070189.1331117, 4), 3: (1600070189.1331134, 3), 1: (1600070189.1331143, 7), 2: (1600070189.133115, 10)};>

```

Note that first dict contains key value pair (5,1) and and second dict contains key value pair (5,4). In this case (5,4) is used in the union dict as dictionary cannot have duplciate keys.

Example for DefaultDictTTL:
```
dict_ttl2 = DefaultDictTTL(10)
[dict_ttl2.append_values(k, v) for k, v in zip([random.randint(1, 2) for x in range(1, 3)],[random.randint(1, 2) for x in range(1, 3)])]

result = DefaultDictTTL(10).dict_union(dict_ttl,dict_ttl2)
```

Output of previous code is 
```
<DefaultDictTTL@0x7f980e82c348; ttl=10, Dict=defaultdict(<class 'list'>, {1: [(1600071123.7458355, [1, 2])]});>
<DefaultDictTTL@0x7f980e82c3a8; ttl=10, Dict=defaultdict(<class 'list'>, {2: [(1600071123.7458715, [1, 1])]});>
{1: [(1600071123.7458355, [1, 2])], 2: [(1600071123.7458715, [1, 1])]}

```

<b>Dict Intersection</b>
This function will perform an intersection of two dicts, and return a dict that contains common elements from both. 

Example for DictTTL and OrderedDictTTL:

```
dict_ttl3 = DictTTL(10,DictTTL(10).dict_intersection(dict_ttl,dict_ttl2))
print(dict_ttl3)
```
Output of the code is

```
<TTLDict@0x7fd95599c320; ttl=30, Dict={2: (1600071434.6685352, 1), 1: (1600071434.6685383, 2)};>
<TTLDict@0x7fd95596cda0; ttl=20, Dict={2: (1600071424.6685476, 2), 1: (1600071424.6685486, 2)};>
<TTLDict@0x7fd9559775f8; ttl=10, Dict={1: (1600071414.6686475, 2)};>
```

Example for DefaultDictTTL:

```
dict_ttl3 = DefaultDictTTL(10).dict_intersection(dict_ttl,dict_ttl2)
print(dict_ttl3)
```

```
<DefaultDictTTL@0x7f9af0d06348; ttl=10, Dict=defaultdict(<class 'list'>, {1: [(1600071557.9203384, [1, 1])]});>
<DefaultDictTTL@0x7f9af0d063a8; ttl=10, Dict=defaultdict(<class 'list'>, {1: [(1600071557.9203734, [1, 1])]});>
defaultdict(<class 'list'>, {1: [(1600071557.9203734, [1, 1])]})
```


<h4> Following functions are not available for DefaultDictTTL() </h4>

<b>sort_by_value()</b>

This function sorts the dictionary by value. 
For example, 
```
print(dict_ttl.ttl_items())
print(dict_ttl.sort_by_value(reverse=False))
```

Output of the previous code is 
```
[(3, (1600071865.3093538, 5)), (5, (1600071865.3093593, 4)), (2, (1600071865.3093617, 3)), (1, (1600071865.3093634, 1))]
{1: (1, 1600071865.3093634), 2: (3, 1600071865.3093617), 5: (4, 1600071865.3093593), 3: (5, 1600071865.3093538)}

```

<b>invert_dict_map()</b>
This function inverts the key and value mapping, keys become the value and value becomes the key

Example:
```
print(dict_ttl.ttl_items())
print(dict_ttl.invert_dict_map())
```

Output of the previous code is 

```
[(5, (1600072079.488873, 3)), (3, (1600072079.4888773, 4)), (2, (1600072079.4888792, 1)), (4, (1600072079.4888806, 5)), (1, (1600072079.488882, 3))]
<TTLDict@0x7fafb9b74da0; ttl=30, Dict={3: (1600072079.4889877, 1), 4: (1600072079.4889824, 3), 1: (1600072079.4889843, 2), 5: (1600072079.488986, 4)};>
```


<h3>Benchmark</h3>
The results for appending data, search, delete and iteration are

```
4.850327831998584
0.000069
0.01623199600726366
0.08708046599349473

OrderedDictTTL timeit, append get delete iter
4.888802872999804
0.000074
0.0314204449969111
0.6668103310075821

DictTTL timeit, append get delete iter
4.799268881994067
0.000094
0.016524064994882792
0.6396878870000364
```

<h3>References</h3>
This work builds upon the following works

https://github.com/jvtm/ttldict

https://github.com/mobilityhouse/ttldict
