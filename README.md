# A malloc-free JSON parser for Arduino

The library is an thin C++ wrapper around the *jsmn* tokenizer: http://zserge.com/jsmn.html

It's design to be very lightweight, works without any allocation on the heap (no malloc) and supports nested objects.

It has been written with Arduino in mind, but it isn't linked to Arduino libraries so you can use this library on any other C++ project.

## Example

    char* json = "{\"Name\":\"Blanchon\",\"Skills\":[\"C\",\"C++\",\"C#\"],\"Age\":32,\"Online\":true}";

    JsonParser<32> parser;

    JsonHashTable hashTable = parser.parseHashTable(json);

    if (!hashTable.success())
    {
        return;
    }

    char* name = hashTable.getString("Name");

    JsonArray skills = hashTable.getArray("Skills");

    int age = hashTable.getLong("Age");

    bool online = hashTable.getBool("Online");

## How to  use ?

### 1. Install the the library

Download the library and extract it to:

    <your Arduino Sketch folder>/libraries/ArduinoJonsParser

### 2. Import in your sketch

Just add the following line on the to of your `.ino` file:

    #include <JonsParser.h>
    
### 3. Create a parser

To extract data from the JSON string, you need to create a `JsonParser`, and specify the number of token you allocate for the parser itself:

    JsonParser<32> parser;
    
> #### How to choose the number of tokens ?

> First you need to know exactly what a token is. A token is an element af the JSON object: either a key, a value, an hash-table or an array.
> As an example the `char* json` on the top of this page contains 12 tokens (don't forget to count 1 for the whole object and 1 more for the array itself).

> The more tokens you allocate, the more complex the JSON can be, but also the more memory will be occupied.
> Each token takes 8 bytes, so `sizeof(JsonParser<32>)` is 256 bytes which is quite big in an Arduino with only 2KB of RAM.
> Don't forget that you also have to store the JSON string in memory and it's probably big.

> 32 tokens may seem small but it's very descent for an 8-bit processor, you wouldn't get better results with other JSON libraries.

### 4. Extract data

To use this library, you need to know beforehand what is the type of data contained in the JSON string, which is extremely likely.

#### Hash table

Consider we have a `char* json` pointing to the following JSON string:

    {
        "Name":"Blanchon",
        "Skills":[
            "C",
            "C++",
            "C#"],
        "Age":32,
        "Online":true
    }

In this case the root object of the JSON string is a hash table, so you need to extract a `JsonHashTable`:
   
    JsonHashTable root = parser.parseHashTable(json);
    
To check if the parsing was successful, you must check:

    if (!root.success())
    {
        // Parsing fail: could be an invalid JSON, or too many tokens
    }
    
And then extract the member you need:
    
    char* name = hashTable.getString("Name");

    JsonArray skills = hashTable.getArray("Skills");

    int age = hashTable.getLong("Age");

    bool online = hashTable.getBool("Online");
    
#### Array

Consider we have a `char* json` pointing to the following JSON string:

    [
        [ 1.2, 3.4 ],
        [ 5.6, 7.8 ]               
    ]

In this case the root object of the JSON string is an array, so you need to extract a `JsonArray`:
   
    JsonArray root = parser.parseArray(json);
    
To check if the parsing was successful, you must check:

    if (!root.success())
    {
        // Parsing fail: could be an invalid JSON, or too many tokens
    }
    
And then extract the content by its index in the array:
    
    JsonArray row0 = root.getArray(0);
    double a = row0.getDouble(0);
    
or simply:

    double a = root.getArray(0).getDouble(0);
   
## Code size

Theses tables has been created by analyzing the map file generated by AVR-GCC after adding `-Wl,-Map,foo.map` to the command line.

As you'll see the code size if between 1680 and 3528 bytes, depending on the features you use.

### Minimum setup

<table>
	<tr>
		<th>Function</th>
		<th>Size in bytes</th>
	</tr>
	<tr>
		<td>strcmp(char*,char*)</td>
		<td>18</td>
	</tr>
	<tr>
		<td>jsmn_init(jsmn_parser*)</td>
		<td>20</td>
	</tr>
	<tr>
		<td>JsonParser::parse(char*)</td>
		<td>106</td>
	</tr>
	<tr>
		<td>JsonObjectBase::getNestedTokenCount(jsmntok_t*)</td>
		<td>84</td>		
	</tr>
	<tr>
		<td>JsonObjectBase::getStringFromToken(jsmntok_t*)</td>
		<td>68</td>		
	</tr>
	<tr>
		<td>JsonArray::JsonArray(char*, jsmntok_t*)</td>
		<td>42</td>		
	</tr>
	<tr>
		<td>JsonArray::getToken(int)</td>
		<td>112</td>		
	</tr>
	<tr>
		<td>JsonArray::getString(int)</td>
		<td>18</td>
	</tr>
	<tr>
		<td>JsonHashTable::JsonHashTable(char*, jsmntok_t*)</td>
		<td>42</td>		
	</tr>
	<tr>
		<td>JsonHashTable::getToken(char*)</td>
		<td>180</td>		
	</tr>
	<tr>
		<td>JsonHashTable::getString(char*)</td>
		<td>18</td>
	</tr>
	<tr>
		<td>TOTAL</td>
		<td>1680</td>
	</tr>
</table>

### Additional space to parse nested  objects

<table>
	<tr>
		<th>Function</th>
		<th>Size in bytes</th>
	</tr>
	<tr>
		<td>JsonArray::getArray(int)</td>
		<td>42</td>
	</tr>	
	<tr>
		<td>JsonArray::getHashTable(int)</td>
		<td>64</td>		
	</tr>
	<tr>
		<td>JsonHashTable::getArray(char*)</td>
		<td>64</td>
	</tr>
	<tr>
		<td>JsonHashTable::getHashTable(char*)</td>
		<td>42</td>
	</tr>
	<tr>
		<td>TOTAL</td>
		<td>212</td>
	</tr>
</table>

### Additional space to parse `bool` values

<table>
	<tr>
		<th>Function</th>
		<th>Size in bytes</th>
	</tr>
	<tr>
		<td>JsonObjectBase::getBoolFromToken(jsmntok_t*)</td>
		<td>82</td>
	</tr>	
	<tr>
		<td>JsonArray::getBool(int)</td>
		<td>18</td>		
	</tr>
	<tr>
		<td>JsonHashTable::getBool(char*)</td>
		<td>18</td>
	</tr>
	<tr>
		<td>TOTAL</td>
		<td>130</td>
	</tr>
</table>

### Additional space to parse `double` values

<table>
	<tr>
		<th>Function</th>
		<th>Size in bytes</th>
	</tr>
	<tr>
		<td>strtod(char*,int)</td>
		<td>704</td>
	</tr>	
	<tr>
		<td>JsonObjectBase::getDoubleFromToken(jsmntok_t*)</td>
		<td>44</td>
	</tr>	
	<tr>
		<td>JsonArray::getDouble(int)</td>
		<td>18</td>		
	</tr>
	<tr>
		<td>JsonHashTable::getDouble(char*)</td>
		<td>18</td>
	</tr>
	<tr>
		<td>TOTAL</td>
		<td>796</td>
	</tr>
</table>

### Additional space to parse `long` values

<table>
	<tr>
		<th>Function</th>
		<th>Size in bytes</th>
	</tr>
	<tr>
		<td>strtol(char*,char**,int)</td>
		<td>606</td>
	</tr>	
	<tr>
		<td>JsonObjectBase::getLongFromToken(jsmntok_t*)</td>
		<td>56</td>
	</tr>	
	<tr>
		<td>JsonArray::getLong(int)</td>
		<td>18</td>		
	</tr>
	<tr>
		<td>JsonHashTable::getLong(char*)</td>
		<td>18</td>
	</tr>
	<tr>
		<td>TOTAL</td>
		<td>710</td>
	</tr>
</table>