# Opine Script Documentation
###### version: 0.2.0

## Blocks

Every script has an if: block, and where necessary, an elseif: and else: blocks.
The content of each block ends with end: and the entire if block ends with endif:

This is the general form of a block:

```
if <condition> :
  <actions>
  end:
endif:
```

This is how it looks like with elseif and else

```
if <condition> :
   <actions>
   end:
elseif <conditon>:
   <actions>
   end:
else:
   <actions>
   end:
endif:
```

You can have multiple if: ... endif: blocks in a single script.
```
if <condition> :
   <actions>
   end:
endif:
if <condition> :
   <actions>
   end:
else:
   <actions>
   end:
endif:
```

## Conditions

At the moment we only do conditions with functional expressions. Meaning, we don't use symbols like ==, >, < ...etc.

Instead, we use functions of the form: fn(<term>,<term>).

Example: to check if the value of variable varX is equal to the value of variable varY, we can use the function eq(varX, varY).

Other comparison functions available include: not equal to !eq,  greater than gt, less than lt, greater than or equal gte and less than or equal lte.
You can combine comparison functions using && for and, and || for or, to create more complex conditions

An example of a complex condition: var1 equals the string "Accra" AND var2 is less than the number 2, OR the value of var1 equals the value of varX:

`(eq(var1,"Accra") && gte(var2, 3) ) || lte(var2, varX)`

Use parenthesis as needed to get the logical operations to be evaluated as required. External parenthesis must not be added around compound expressions

```
if (lt(SKUCounted, 7) && gt(SoKlin30gCount,3)):
set: SoKlin30gScore = 3
end:
endif: 
if (gte(SKUCounted, 7) || (lt(SKUCounted, 7) && lte(SoKlin30gCount,3))):
set: SoKlin30gScore = {SoKlin30gCount} x 1 
end:
endif:
```

## Terms

A term can be a
• variable (more formally, a project variable), e.g. varA
• an option of a variable (multi response variables only), e.g. varB[opt1], where varB is a multi response and opt1 is the name of one of its options;
• a number or a string, e.g. 100, "some test"
• a user defined variable (always prefixed with $), e.g. $myvar
• a function e.g.
`concat(varA, varB), wsget(http://api.opine.com/test)*`.
Other functions include: `*max`, `min`, `avg`, `randomint`, `randomstr`



## Actions

There are two types of actions:
• UI Actions: affect how the variable and its options are rendered on the UI.
• Data Actions: affect how data is manipulate or stored.

Actions are expressed as <action_name> : <expression> and are delimited by line endings (i.e. one action per line).
Action names are designed to be intuitive.

Examples of UI actions:
• hide: varA hide varA when displayed in a list view, OR hide all options of varA when displayed in normal view
• hide: varA[opt1] hide opt1 of variable varA
• show: varB show varB if it is hidden in a list view
• show: varB[opt2] show opt2 of variable varB
• disable: food  disable all options OR input field of the variable food
• disable: food[rice] disable the rice option of the variable food

• enable: food  enable all options OR input field of the variable food

• enable: food[banku] enable the banku option of the variable food
• check: food[rice]check/select the rice option of variable food
• uncheck: food[rice] unckeck/deselect the rice option variable food
• goto OP-727 CLOSED : formD skip to formD
• terminate: 0 stop the data capture

recall:

Examples of Data actions:
• set: varA = 2 assign the value 2 to varA
• set: food[rice] = 1 assign 1 to the rice option of the multi response variable food. This will effectively result in the rice option being selected in the stored data, but not in the UI.
• set: $myvar = "something" assign the string "something" to the user-defined variable $x. This action both defines and assigns to the user-defined variable.
• set: varB = randomint(100, 150) generates a random number between 100 and 150 (inclusive) and assigns to varB
• set: $newId = randomstr(7) generates a random alpha-numeric string of length 7 and assigns to the user-defined variable $newId
• set: $webresult = wsget(https://api.opine.com/test) make a GET request to the url and assign the result to the variable $webresult
• set: varD = $webresult assign the value of the variable $webresult to varD 

Usage Notes
Also note the use of the UI actions and Data actions in preconditions and post conditions.
All the UI actions must be used in precondition scripts only. goto: and terminate: must be placed,That's because they are executed before the UI for the current variable/form is displayed.
Data actions manipulate data in variables and do not influence what is displayed on the UI. Hence they can be used in both pre and post condition scripts.



## Functions
SMS
GPS
Lookup
Image


## UPDATES:
When multiple goto are encountered, in a list of actions, only the last goto is executed.
terminate  is the last action to be executed form a list of actions, regardless of when it is called in relation to other actions in a pre or post condition.
hide: varA hide varA when displayed in a list view, OR hide all options of varA when displayed in normal view
hide: varA[opt1] hide opt1 of variable varA
show: varB[opt2] show opt2 of variable varB
 When dealing with a form in LIST or Table Mode, the entire logic for the form must be in the pre/post condition of the first variable. Since the table / list will be rendered as a unit.
MATH NOTE: All references to variables on the right hand side of an = must be in { } if used in a math expression
Lookup Example GPS: https://api.opine.world/lookups/places/geo/gh?projectid=health-checker&recordid_field=ccid&recordid_value=111&searchvalue=kumasi&gpsvariable=location
Lookup ExampleSearch: https://api.opine.world/lookups/project?pid=food-vendors-survey&searchcol=StoreID&returncol=StoreName&searchvalue={storeID}
Images Download: https://storage.googleapis.com/opine-world/media/<project-id-here>/captured/<filename-here>

