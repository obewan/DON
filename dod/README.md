# Data Object Description
### ** 1.0-DRAFT **

Data Object Description (DOD) is the format for descriptive header of a Data Object Notation (DON) file, like the DTD of XML files.
It offer some dtd, xsd and sql constraints for DON documents. 


## Grammar
```
comment = ( "//", string ) | ( "/*", string, "*/" ) ;
donHeader = '[#DON ', version, encodage, schema?, include?, ']' ; (* DON header *)
dodHeader = '[#DOD ', version, encodage, ']' ; (* DOD header on an external DOD file *)
version = "version(", string, ")" ;
encodage = "encodage(", string, ")" ;
schema = "schema(", location*, ")" ; (* location of DOD external schemas with prefixes, like xsd:schema *)
location = prefix, "=", uri ; 
include = "include(",  uri, ")"; (* include an external DOD file *)

element = '[', name, list?, ']' ;
name = string ;
list = '(', expression*, ')' ;
expression = name, operator? ;
attribut = '[', name, type?, ']' ;

type = "(#", typeName, constraints? ')' ;
typeName = "boolean" | "float" | "integer" | "string" | "date" | "time" | enumeration ;

enumeration = "string(" , string, ("|", string)* , ")" ;

constraints = '(', constraint*, ')' ;
constraint = required, min, max, minExclusive, maxExclusive, range, notNull, whiteSpace, pattern, unique, default, totalDigits;

required = "required" ; (* mandatory value, can not be null or empty *)
min = "min(", integer | float, ")" ; (* inclusive minimum value for an integer or a float (using a float) and minimum length for a string. Can be min(0) for an unsigned int, and min(1) for a non-empty string. *)
max = "max(", integer | float , ")" ; (* inclusive maximum value for an integer or a float (using a float)  and maximum length for a string. Can be max(0) for a negative int. *)
minExclusive = "minExclusive(", integer | float, ")" ;
maxExclusive = "maxExclusive(", integer | float, ")" ;
range = "range(", integer | float, ",", integer | float, ")"; (* inclusive minimum and maximum range, for integer and floats. *)
notNull = "notNull" ; (* not null value constraint, for any type, like the sql "not null" *)
whiteSpace = "whiteSpace(", "preserve" | "replace" | "collapse", ")" ; (* preserve, replace or collapse the white spaces, like the xsd:whiteSpace restriction. *)
pattern = regExpr ; (* regular expression constraint, like the xsd:pattern restriction. *)
unique = "unique" ; (* unique value constrain within the element scope, like "xsd:unique" and sql "unique". *)
default = "default(", value, ")" ; (* default value for any type. *)
totalDigits = "totalDigits(", integer, ")" ; (* maximum digits allowed for float, like xsd:totalDigits restriction. *)

```

Example, DOD description in a DON file :
```
[#DON version(1.0-DRAFT) encodage(UTF-8)]
[document (menu*)]
[menu (id, value, menuItem*)]
[id (#integer(min(0))]
[value (#string(notNull, max(30))]
[menuItem (label, onClick?)]
[label (#string(notNull, whiteSpace(collapse)]
[onClick (#string)]

{ 
	document : { 
		menu : { 
			id : 1, 
			value : "file", 
			menuItem : { 
				label : "new", 
				onClick : "new()" 
			} 
			menuItem : { 
				label : "open", 
				onClick : "open()" 
			} 
		} //comment : end menu 1 
	}
}
```

