### What is serialization ?

It is the process of converting complex data structures, such as objects and their fields, into a flatter format that can be sent and received as a sequential stream of bytes. 

Serializing data makes it much simpler to - 

1. write complex data to inter-process memory, a file or a database.
2. send complex data, for example, over a network, between different components of an application, or in an API call.

### What is deserialization ?

It is the process of restoring the byte-stream to a fully functional replica of the original object, in the exact state as when it was serialized. 

The website's logic can then interact with this deserialized object, just like it would with any other object.

Many programming languages offer native support for serialization.

Serialization may be referred to as marshalling (Ruby) or pickling (Python).

### What is insecure deserialization ?

**Insecure deserialization** is when a user-controllable data is deserialized by a website. This potentially enables an attacker to manipulate serialized objects in order to pass harmful data into the application code.

It is even possible to replace a serialized object with an object of an entirely different class. Alarmingly, objects of any class that is available to the website will be deserialized and instantiated, regardless of which class was expected. For this reason, insecure deserialization is sometimes known as an **object injection** vulnerability.

An object of an unexpected class might cause an exception. By this time, however, the damage may already be done. Many deserialization-based attacks are completed **before** deserialization is finished. This means that the deserialization process itself can initiate an attack, even if the website's own functionality does not directly interact with the malicious object. For this reason, websites whose logic is based on strongly typed languages can also be vulnerable to these techniques.

### What is the impact of insecure deserialization ?

The impact of insecure deserialization can be very severe because it provides an entry point to a massively increased attack surface. It allows an attacker to reuse existing application code in harmful ways, resulting in numerous other vulnerabilities, often remote code execution.

Even in cases where remote code execution is not possible, insecure deserialization can lead to privilege escalation, arbitrary file access, and denial-of-service attacks.

# Continued ...
