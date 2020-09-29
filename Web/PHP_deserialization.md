PHP_deserialization

## Theory 

- Serialization is when an object in a programming language (say, a Java or PHP object) is converted into a format that can be stored or transferred. Whereas deserialization refers to the opposite: it’s when the serialized object is read from a file or the network and converted back into an object.

- Insecure deserialization vulnerabilities happen when applications deserialize objects without proper sanitization. An attacker can then manipulate serialized objects to change the program’s flow.


https://vkili.github.io/blog/insecure%20deserialization/exploiting-php-deserialization/


https://www.youtube.com/watch?v=HaW15aMzBUM&feature=youtu.be

https://0xdf.gitlab.io/2020/01/18/htb-player.html