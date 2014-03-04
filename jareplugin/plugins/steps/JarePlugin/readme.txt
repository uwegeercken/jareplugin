JaRE - Java Business Rule Engine

Pentaho PDI Plugin Step - Version 0.1

General:
========
Java based tool to check data against rules which are defined in XML file(s).
Checks for equality, greater/smaller, matching (regular expression),
uppercase/lowercase, numeric, is in list, length, soundex, and more.
Rules are organized in groups and subgroups and can be connected with 
and/or logic to a complex data validation task.

The rule engine will process all incomming rows and pass them onward to the
next step (main output step). the output rows will contain additional fields
indicating how many rules and rule groups failed or passed.

If you want detailed results of the rules you may optionally specify a second
output step. for each input row you will then get one output row per rule. so
if you have 1000 rows and 50 rules the step will receive 50000 rows, showing
in detailed which rules failed and why. this second step is labeled rule results
step.
You may also select how much information is passed onwards to the rule results
step: only passed ones or only failed ones.

The rule engine can use any Java object and method and run checks against data
as it is based on the Java reflection mechanism.

The rule engine can also run standalone outside PDI.

Use of plugin step:
===================
Provide the path and name of the rule file (e.g. /tmp/rules.xml). Or specify
a folder which contains rule files; in this case all rule files with the extension
.xml will be processed. Or put all rules files in a zip file and specify the zip
file name.
You may also use a placeholder in the form of ${myvariable}. it will be replaced
by a transformation parameter with the same name.

Specify the main output step. each input row will be output to the main output step
containing additional fields indicating the results of the rule engine run.

The plugin step runs the rule engine on each input row and checks the data
according to the rules defined in the xml file.
The step adds five fields to each output row:
- ruleengine_groups: the number of groups that are defined in the xml rule file
- ruleengine_groups_failed: number of groups that failed on the relavant input row
- ruleengine_rules: number of rules that are defined in the xml rule file
- ruleengine_rules_failed: number of rules that failed on the relavant input row
- ruleengine_actions: number of actions that are defined in the xml rule file

Specify a rule results step if you need details of the results of the rule engine
run.

Depending on how the logic is defined in the xml rule file, some rules might fail
and still the group as a whole passes the checks. So usually you want to check if
the ruleengine_groups_failed field is equal to zero, meaning the group as a whole
passed all checks.

You may also define actions in the rule files. an action will be triggered depending
if the rule group failed or passed. one action could be to concat fields, apppend or
prepend values to fields, do mathematics and then finally set antother field to the
resulting value of the action.
One can also write its own actions for doing more sophisticated operations.


Rule XML file:
==============
In the rule XML file in the "object" tag, of the rule, the class name is always set
to "com.datamelt.util.RowFieldCollection". This is the class that is passed from the
PDI step to the rule engine.

Available methods of this class are:
- getIntegerFieldValue: for fields of type integer
- getFieldValue: for fields of type string
- getBooleanFieldValue: for fields of type boolean
- getLongFieldValue: for fields of type long
- getDoubleFieldValue: for fields of type double
- getFloatFieldValue: for fields of type float

Set the attribute "type" of the "object" tag to: "integer", "string", "boolean", 
"long", "double" or "float", according to the field type. Set the "parameter" attribute
to the name of the field in PDI. The "parametertype" attribute will in this case be
"string".

The "execute" tag will contain the reference to one of the available checks available
in the business rule engine. E.g. "com.datamelt.rules.implementation.CheckIsGreaterOrEqual"
or "com.datamelt.rules.implementation.CheckIsUppercase". See the documentation for more
checks.

What the rule engine does:
==========================
The rule engine will get the value of the input field and compare it to the expected value
in the xml rule file. If there is no expected value (e.g. like for the check 
"CheckisUppercase") the rule engine will determine if the condition is true or false.

You may also compare fields to each other and the business rule engine may also update fields
depending on the result of a group of rules. See the documentation for more details.

The rule engine comes with a lot of predefined checks like greater than, smaller than, equals,
is uppercase, sounds like, check matches (regex comparison), check is in list, check is numeric,
check is null, check starts with, etc.

Logging:
========
If you set the logging level to Basic, the log will only show if the ruleengine sucessfully
initialized with the given rule file.
If you set the logging level to Detailed, the log will show, which groups of the rule file
failed.
If you set the logging level to Debugging, the log will show, which groups and subgroups of
the rule file failed.
If you set the logging level to Rowlevel, the log will show all details down to the individual
rule.

Documentation:
==============
Read the detailed Rule Engine documentation PDF file that comes with this plugin. There is also
one reference document which shows which checks are available.

The business rule engine and the plugin is published as open source. Sources
are availble on the web page.

Uwe Geercken
Datamelt

email: uwe.geercken@web.de
web:   www.datamelt.com

last update: 2014-03-04

