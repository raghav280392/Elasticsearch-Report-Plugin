# Elasticsearch Report Plugin
##Elasticsearch Report Plugin to Generate Excel Report

There are a lot of companies(small scale to large scale companies) who use Elasticsearch to store massive amount of to data.Most of them find it hard to generate simple reports from Elasticsearch to get information out of it.  So this Elasticsearch plugin can be used to generate reports(Excel) with simple JSON input. This plugin can save the report to the server and also can send E-Mail to the configured recipients. We can also perform some basic operations on the fields to get a computed field in the Excel report.

####List of Operation : 

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


####Operation Syntax and Object Type : 

| # | Operation | Syntax | Object Type |
|---|-----------|---------|------------|
|1|getValue|[0,ES_FIELD]|[0,String]|
|2|getDValue |[1,valueMappingKey,VALUE]|[1,String,String]|
|3|Length |[2,VALUE]|[2,String]|
|4|Format Number Length|[3,VALUE,FORMAT_LENGTH]|[3,String,int]|
|5|Sub String|[4,VALUE,from,to]|[4,String,int,int]|
|6|Character at index |[5,VALUE,index]|[5,String,int]|
|7|Calculate |[6, ARITHMETIC_EXPRESSION]|[6,String]|
|8|Range |[7,valueMappingKey,VALUE]|[7,String,String]|
|9|Array indexOf(int value) |[8,ES_ARRAY_FIELD, value]|[8,String,int]|
|10|Array indexOf(String value)|[9,ES_ARRAY_FIELD,value]|[9,String,String]|
|11|Array valueAt(index) |[10,VALUE,index]|[10,String,int]|

#### Syntax Description :
|Operation | Description|
|---|-----------|
|ES_FIELD| A field in a Elasticsearch document|
|ES_ARRAY_FIELD| A field in a Elasticsearch document|
|valueMappingKey| A field in input JSON given by user |
|ARITHMETIC_EXPRESSION| Any string representing conditional or arithmetic expression |
|VALUE| Can be either string given by user or any other OPERATION. i.e. we can nest the above operations to whatever level we want. In other words, we can perform multi-level OPERATION on the Elasticsearch fields |


####Detailed Example with Sample Data :

__Sample Input Data__
<pre>
{
   "name":"Ramu",
   "gender":"Male",
   "register_number": "5723"
   "marks":{
      "computer_science":78,
      "data_mining":80,
      "dbms":75
      },
   "marksArrayInt":[78,80,75]
   "marksArrayString":["78","80","75"]
}
</pre>

__Sample Input for Plugin__
<pre>
{
	"reportTitle":"Student Details Report",
	"reportName": "Student Details",
	"reportAccess": {
		"fileName": "Student Details Report",
		"ftp": {
			"filePath": "F:\\tmp\\"
		},
		"email": {
			"subject": "Student Report - Detailed Report",
			"deliverTo": [ "E-Mail-ID" ]
		}
	},
	"description": "The attachment contains detailed report of students.",
	"batchSize": 250,
	"host":"localhost",
	"index":"student",
	"type":"details",
	"routing":"",
	"valueMappingKey":{
	   "genderMappingKey":{
         "Male":"M",
         "Female":"F"
      },
      "isPassMappingKey":{
         "x>=40":"PASS",
         "x<40":"FAIL"
      },
      "courseList":{
      	"5":"Bachelor of Technology (B.Tech)",
      	"6":"Master of Technology (M.Tech)",
      	"default":"NA"
      },
      "branchList":{
      	"7":"Computer Science",
      	"8":"Information Technology"
      }
	}
	"statement": {
      "query": {
         "match_all": {
         }
      },
      "fields":[
      	"name",
      	"gender",
      	"register_number",
      	"marks.computer_science",
      	"marks.data_mining",
      	"marks.dbms",
      	"marksArrayInt",
      	"marksArrayString"]
   },
	"config": [
      {
		"title": "Name",
		"format":"[0,name]"
	   },
	    {
		"title": "Name (String Length)",
		"format":"[2,[0,name]]"
	   },
	   {
		"title": "Gender",
		"format":"[0,gender]"
	   },
	   {
		"title": "Register Number",
		"format":"[0,register_number]"
	   },
	   {
		"title": "Computer Science",
		"format":"[0,marks.computer_science]"
	   },
	   {
		"title": "English",
		"format":"[0,marks.data_mining]"
	   },
	   {
		"title": "Economics",
		"format":"[0,marks.dbms]"
	   },
	   {
		"title": "Course Code",
		"format":"[5,register_number,0]"
	   },
	   {
		"title": "Course",
		"format":"[1,courseList,[5,register_number,0]]"
	   },
	   {
		"title": "Branch Code",
		"format":"[5,register_number,1]"
	   },
	   {
		"title": "Branch",
		"format":"[1,courseList,[5,register_number,1]]"
	   },
	   {
		"title": "Roll No",
		"format":"[4,register_number,2,3]"
	   },
	   {
		"title": "Total Marks",
		"format":"[6,[0,marks.computer_science]+[0,marks.data_mining]+[0,marks.dbms]]"
	   },
	   {
		"title": "Average",
		"format":"[6,[0,marks.computer_science]+[0,marks.data_mining]+[0,marks.dbms]/3]"
	   },
	   {
		"title": "Pass/Fail",
		"format":"[7,isPassMappingKey,[6,[0,marks.computer_science]+[0,marks.data_mining]+[0,marks.dbms]/3]]"
	   }
	]
}
</pre>


####Operation and Example :
| # | Operation | Column | Example | Output|
|---|-----------|---------|---------|
|1|getValue|Name<br>Gender<br>Register Number <br>Mark in Computer Science<br>Mark in Data Mining<br>Mark in DBMS|[0,name]<br>[0,gender]<br>[0,register_number]<br>[0,marks.computer_science]<br>[0,marks.data_mining]<br>[0,marks.dbms]|Ramu<br>Male<br>5723<br>78<br>80<br>75|
|2|getDerivedValue |Gender (Short Form)<br>Course<br>Branch|[1,genderMappingKey,Male]<br>[1,courseList,[5,register_number,0]]<br>[1,branchList,[5,register_number,1]]|M<br>Bachelor of Technology (B.Tech)<br>Computer Science|
|3|Length |Name|[2,Ramu]|4|
|4|Format Number Length|-|[3,632,5]|00632|
|5|Sub String|Roll No|[4,[0,register_number],2,3]|23|
|6|Character at index |Course Code<br>Branch Code|[5,register_number,0]<br>[5,register_number,1]|5<br>7|
|7|Calculate |Total<br>Average|[6,[0,marks.computer_science]+[0,marks.data_mining]+[0,marks.dbms]]<br>[6,[0,marks.computer_science]+[0,marks.data_mining]+[0,marks.dbms]/3]|233<br>77.66|
|8|Range |Pass/Fail|[7,isPassMappingKey,85]<br>[7,isPassMappingKey,35]|FAIL|
|9|Array indexOf(int value) |Index of Value 80<br>Index of value 78|[8,marksArrayInt, 80]<br>[8,marksArrayInt, 78]|1<br>0|
|10|Array indexOf(String value)|Index of value 80<br>Index of value 75|[9,marksArrayString,80]<br>[9,marksArrayString,75]|1<br>2|
|11|Array valueAt(index) |Value at index 2<br>Value at index 0 |[10,marksArrayInt,2]<br>[10,marksArrayInt,0]|75<br>78|



