# Elasticsearch Report Plugin
##Elasticsearch Report Plugin to Generate Excel Report

There are a lot of companies(small scale to large scale companies) who use Elasticsearch to store massive amount of to data.Most of them find it hard to generate simple reports from Elasticsearch to get information out of it.  So this Elasticsearch plugin can be used to generate reports(Excel) with simple JSON input. This plugin can save the report to the server and also can send E-Mail to the configured recipients. We can also perform some basic operations on the fields to get a computed field in the Excel report.

####List of Operation: 

1. getValue -  Get the Elasticsearch field value
2. getDValue - Get custom value based on the Elasticsarch field value
3. Length - Get the length of Elasticsarch field value
4. Format Number Length - Format integer to given length
5. Sub String - Get substring of Elasticsarch field value
6. Character at index - Get the character at given index of Elasticsarch field value
7. Calculate - Perform arithmetic operation on Elasticsarch field values
8. Range - Get custom value based on range condition of Elasticsarch field value
9. Array indexOf(int value) - Get index of given integer value from an Elasticsarch array
10. Array indexOf(String value) - Get index of given string value from an Elasticsearch array 
11. Array valueAt(index) - Get the value of given index from an Elasticsarch array



| # | Operation | Syntax |
|---|-----------|---------|
|1|getValue|[0,ES_FIELD_NAME]|
|2|getDValue |[1,valueMappingKey,ES_FIELD_NAME]|
|3|Length |[2,ES_FIELD_NAME]|
|4|Format Number Length|[3,ES_FIELD_NAME,(int) length]|
|5|Sub String|[4,ES_FIELD_NAME,(int) from,(int) to]|
|6|Character at index |[5,ES_FIELD_NAME,(int) index]|
|7|Calculate |[6,(string) ARITHMETIC_EXPRESSION]|
|8|Range |[7,valueMappingKey,(int)]|
|9|Array indexOf(int value) |[8,ES_FIELD_NAME,(int) value]|
|10|Array indexOf(String value)|[9,ES_FIELD_NAME,(string) value]|
|11|Array valueAt(index) |[10,ES_FIELD_NAME,(int)index]|




