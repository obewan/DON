# DON
### Data Object Notation 
### ** DRAFT **

DON, the Data Object Notation, take best parts of XML, DTD, XSD, JSON and SQL constraints, for a textual file format that offer data description, constraints, portability, easy use and reading. 

Features :

* descriptive header, in DOD format (Data Object Description), to add contraints on document and data, that can be include in an other file 
* json basic notation, so users who are familiar with Json will not be disoriented,
* dtd flavors, to describe the document, which is simpler and easier than xsd, can include external DOD file
* minimum, maximum, range constraints, 
* dates, comments (in Java format), 
* basic types (boolean, integer, float, string...), 
* some dtd constraints (require, default, enumerations...)
* some xsd constraints (whiteSpace, totalDigits, pattern...),
* some sql constraints (not null, unique...)

### Exemple of a Don file, with internal Dod header 
```
[#DON version(1.0-DRAFT) encodage(UTF-8)]
[document (menu*)]
[menu (id, value, menuItem*)]
[id (#integer(min(0), unique)]
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

### Links 
* [DOD grammar](dod/README.md) 

### Todo list
* a lex and yacc parser and validator
* some common langages parsing libraries (C, C++, C#, Java, JavaScript...)
* some tools
