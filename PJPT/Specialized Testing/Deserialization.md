# Specialized Testing: Deserialization
- This is not part of the Practice Ethical Hacker Course, but I found it relevant to preparing for the PJPT
## Course Overview
- Find and input location for serialized objects
- Identify deserialization processor
- Try specific attack parser functionality
- Attack object
  - Modify properties
  - Modify class name
  - Serialize functions
- Attack other objects/framework
- What insecure deserialization is
- How to perform property injection
- What gadget chains are
- What property-oriented programming is

Prerequisites:
- Security and penetration testing
- Software development

## Understanding Insecure Deserialization
### Course Introduction
- Deserialization is the process of converting or hydrating a serialized object, a flattened object, using a processor, into an object again
- This process can be insecure and exploited

### Serialization and Deserialization Primer
- Serialization is converting an object, using a process, into a bytestream or a serialized form
- Used to store or transport an object

Serialized formats:
- Text
  - JSON
  - YAML
  - XML
  - Native PHP
- Binary
  - Native Java
  - Native Python
  - Native .NET
  - Protobuf

- Insecure deserialization is when you are able to modify the input and the object will still be deserialized
- This is deserializing untrusted user input
- Modifying the input, modifies the output
- The process itself can also be attacked
- The process can trigger other processes

Objects:
- Properties
- Magic methods
- Automatically executed on certain occasions
  - Instantiation
  - Deserialization
  - Garbage collection
  - String representation

Attacking Process Using Magic Methods:
- Serializing class definitions by defining/overwriting magic methods
- Using function variables by defining/overwriting

### Demo: Abusing Native Deserialization Process
- Define a custom "deserialize" magic method
- Serialize the object to native format
- Deserialize the object
- Using a Python script

deserialize.py
- Either read a file or read from the stdin file descriptor
- Then tries to deserialize it's contents
- No class definition, it is part of the serialize file

- Create a serialized object called object
- Then deserialize the object
- Remove rogue file "REDUCE"

serialize.py
- Imports `myobject` where the object definition is
- Similar functions to deserialize.py
- Either writes to a file, the serialized object, or it cats it to stdout

myobject.py
- Contains class definition
- Everything is being serialized, including the magic method `__reduce__`
- This is the magic method that will automatically be executed as soon as an object is being deserialized
- It defines a subprocess function, "touch", "REDUCE"
- This overwrites the built-in magic method
- Creating our own object and overwriting the magic methods exploits the Python deserialize process

### Demo: Abusing YAML Deserialization Process
- Even a plaintext format like YAML can be susceptible to insecure deserialization
- Define magic method in object
- Serialize object to YAML
- Deserialize YAML using PyYAML

serialize.py
- Loads the pickle module, native Python deserialization and serialization format
- Imports yaml module, to read and write YAML files
- Takes optional argument called format, uses pickle if excluded
- Uses yaml format if the supplied value is yaml

- Serialize an object with the yaml format
- object has valid yaml syntax
- Deserialize the object with yaml format
- Created the "REDUCE" file
- Overwrote the magic methods defined in myobject.py file

### Module Summary
- Modified the input
- Generated out own object made up of a class
- Used that to attack the process
- Effectively modified the object by modifying the input
- If you can supply class definition, then you can attack the process

## How to Find and Test for Insecure Deserialization Vulnerabilities
### Detecting Serialized Objects
- To test for insecure deserialization vulnerabilities, it is important to know where to find those serialized objects
- The file system, network transport, and database are where you can detect serialized objects

Detection:
- Identify serialized objects
- Normalize the object
  - Often Base64 encoded
  - Often GZipped
  - Binary format is often the normalized format
- Identify serialization format
  - Binary
  - Text (JSON, YAML)
- Identify deserialization processor
  - Language (Java, PHP)
  - Library (Pickle)
  - Framework
- Need an entry point first detect where serialized objects are used to test for them serialized

### Demo: Fingerprinting Serialized Objects
- Serialize a Java object
- Use utility file to classify the serialization data: `file object.java`
- Correctly identifies it as Java serialization data
- Use `xxd` to create a hex dump of the file to see what it looks like in binary format `xxd object.java`
- The fist few bytes you see in the top left: `aced 0005` is a common fingerprint to determine whether something is a Java serialized object

- Serialize a PHP object
- `file object.php` say it is a Solitaire Image Recorder format, obviously incorrect
- Use custom script to identify serialization format: `detect_type.py object.php` which correctly identifies it

- Serialize a Python object
- `file` again incorrectly identifies the file
- The helper script correctly identifies it
- Using `xxd` we can see the top left `**04` which stands for serialization version 4 and the dot at the end represented by `2e`

### Generic Test and Attack Methodology
Systematic Approach:
- Find an input location for serialized objects
- Identify deserialization processor
- Attack parser by trying specific parser functionality
- Attack object
  - Modify properties
  - Modify class name
  - Serialize functions
- Attack other objects/framework by finding classes that execute "properties" 

### Demo: Modify Object Using Property Injection
- Serialize object
- Inspect deserialization source code
- Modify object name
- Modify and inject properties
- Deserialize modified object

- Serialize a PHP object
- Echo file and pipe it through deserialization process
- Deserialization proves doesn't recognize class
- Only 1 property, boolean admin property


- Property injection attack words best when you have access to the source code
  
deserialize.php
- It either reads the contents of a file from stdin or a filename
- It includes the class definition, deserializeobject.php

 deserializeobject.php
- The class name, DeserializeObject,
- 2 properties, admin and username
- Defines a magic function, which is automatically executed when the object is created
- Instead of being able to pass the magic function in a serialized file, it is hard coded and uses properties specified by the object

- The object name is encoded in the serialized file
- Set the string length to 17 and the name to DeserializeObject
- Now it recognizes the object as DeserializeObject
- No change the property value to 1, switching it to true
- Add another property to the object
- Change the number of properties to 2
- Define a new string called 'username' and set the value to 'BOB'
- Passing this to our function injects the property 'username', modified the property 'admin', and changed the class name
- Modifying the serialized file, we modified the object


### Module Summary
Detecting serialized object
- Normalize the object
- Fingerprint the format
- Identify the deserialization processor

Generic test and attack methodology
- Find input location
- Attack parser functionality
- Modify class name and properties
- Serialize function, especially magic methods

## Advanced Insecure Deserialization Exploits
### Module Introduction
- Gadget chains
- Property-oriented programming
- Creating gadget exploit
- Exploitation tools
- Using tools

### Gadget Chains
- The chain that brings you from the initial starting point to where it executes our function or a function with our arguments or variables
- The beginning of a chain in our object and the magic method starts a chain
  - Object creation
  - Garbage collection
- An object can invoked another object when it is created
  - Setting properties of objects
- Look for a function that serves as a sink, an endpoint of the chain
  - If an object doesn't have an endpoint or a sink, then we can allow that object to instantiate another object
  - We want to execute a function, usually with a property we supplied with the initial object

PHP example:
- Initial class
  - Has a private property to hold second class, 'Secondary'
  - Defined a magic method, '__wakeup' that executes a function in the secondary class when this object is instantiated
- Secondary class
  - Has a private property to hold a payload
  - The sink function, executed by the Initial class evaluating this payload
-  If you can instantiate an Initial class and specify a Secondary class, including the payload, then you can execute arbitrary functions
-  Even if you can't define functions, supplying specific properties can execute your own code

### Demo: Create Custom Gadget Chain
- Inspect the deserialization source
  - Usually when creating gadget chains, you need to have access to the deserialization source code
- Generate "exploit" class
- Serialize that exploit
- Feed that exploit to our deserialization function to see if it works

deserialize_chain.php
- This function deserializes a file using class definitions
- Defines 2 classes
- To test for insecure deserialization vulnerabilities, look for a place, a sink, where we can execute our own function
- We need to instantiate an object, Initial, 
- Initial instantiates an object, Secondary
- Then we set the payload in order to execute it

serialize_chain.php
- Helper script
- Same class definitions
- Creates a first Initial object
- Create a Secondary object
- Set the payload of the secondary object to our function, our real payload, in this case, phpinfo function
- Set the Secondary property to our object that we just generated
- Lastly, serialize our custom gadget
- The included "serialize_function.php" takes care of writing it to stdout or a file
- Running it as an error because it can't access the private property payload
- Copying the class definition to our own script doesn't work

serialize_chain_self.php
- Improved version of our gadget chain creator
- Replace private properties with public counterparts
- We're free to define our own classes
- Running it creates a PHP serialized object
- Pipe it through the deserialization script using its own class definitions
- Executed our own specified function, phpinfo, creating our own gadget class
- Set the private property of a secondary class that was instantiated by the Initial class
- Known as property-oriented programming

### Property-Oriented Programming
- An attack methodology
- Attacker controls the properties of objects
- Magic method starts the chain
- Sink executes function using an object property
- Classes only need to be available to app, don't need to be used
- Researching gadget chains is very time-consuming

Exploitation tools:
- ysoserial is the first exploitation too that came out around 2015
- It generates Java gadget chains
- YSoSerial.Net is similar but creates .NET gadget chains
- PHPGGC, PHP Generic Gadget Chains

### Demo: Create Gadget Chain Using ysoserial
- Create a gadget chain using ysoserial
- Inspect it with SerializationDumper

- Run ysoserialusing the java tag to generate a serialized object containing a gadget chain which can run your payload
- If you're testing for insecure deserialization vulnerabilities, and if you have found a location where you can input a serialized file, which one can be deserialized, if it uses one of those frameworks, then you can use this tool to generate a gadget chain for you
- Run SerializationDumper to find what kind of classes are serialized and other information about the serialization data
- Use it to look inside the exploit file to see what kind of classes are actually serialized and execute the shell command
- Then use xxd to convert the binary file into hexadecimal format
- Pipe the output through tr -d and remove the new line characters
- This converts the binary into hex so SerializationDumper understands it
- The bottom of the serialized object is the sink, or where the payload is being executed, id command
- Scrolling up shows the different class definition that are being instantiated by the initial class in order to execute the payload

### Course Summary
- Insecure deserialization is when we can modify input and that the deserialization process processes our modified input or untrusted input
- By modifying the input, we were able to modify the object
- Gadget chains: a sequence of operations that lead to the execution of a payload, and how to abuse them
- Property oriented programming: executing functions based on properties you set in an object
- Find input locations for serialized objects, a file, across the network, or in a database
- Fingerprint the underlying framework, important when generating your own gadget chains
- Modify properties of an object
- Generate "custom" classes, defining the object which gets deserialized by the process
