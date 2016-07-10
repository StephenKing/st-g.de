---
layout: post
title:  "Doing Filename Checks Securely (in PHP)"
date:   2011-04-03 00:00:00 +0200
tags: [typo3, code]

comments: true
share: true
---

Recently a security issue in TYPO3 [has been fixed](http://typo3.org/teams/security/security-bulletins/typo3-sa-2010-022/), where it was possible to circumvent checks, which should ensure file names to match specific patterns (e.g. denying .php file extensions to be uploaded or renamed to).

As this problem is heavily caused by PHP's laxity, this blog entry aims to provide some explanations to you as developer to prevent you from placing similar security holes in your software or TYPO3 extensions.

Imagine you are programming a very basic file upload script and use the following piece of code:

{% highlight php %}
$filename = $_POST['filename'];
$contents = $_POST['contents'];
file_put_contents($filename, $contents);
{% endhighlight %}

It is obvious that this code enables attackers to place arbitrary files on the server. So passing `evil.php` as a file name and sending PHP code, an attacker can execute any PHP code on your server.

So you might want to prevent saving .php files and other executable extensions by checking the file extension: 

{% highlight php %}
if (0 === preg_match("/(.*)\.(php|php3|php4|php5)$/i", $filename) {
    file_put_contents($filename, $contents);
} else {
    die('Invalid file name given');
}
{% endhighlight %}

By using a whitelist approach, you can limit your upload script to allow only image file extensions: 

{% highlight php %}
if (1 === preg_match("/(.*)\.(jpg|gif|png)$/i", $filename) {
    file_put_contents($filename, $contents);
} else {
    die('Invalid file name given');
} 
{% endhighlight %}

What I also read very frequently in the web is that file inclusions using the following code can be treated as "safe": 

{% highlight php %}
include('./themes/' . $_GET['filename'] . '.php'); 
{% endhighlight %}

But this code is far from being secure. The code snippet does not only expose a [directory traversal](http://en.wikipedia.org/wiki/Directory_traversal) vulnerability (`../../../evil/more-evil`). Even the trailing `.php` will not secure the include. As arbitrary `.php` files placed on the server can be executed, you are already in trouble. Not only if you have `.php` files containing malicious code on your server, but also if `.php` files are included which are meant to be included in another context.

Still the list of possible ways to exploit this "safe" code is not finished. Also the code examples above which are supposed to do a proper validation of the user provided file name are vulnerable. Let me explain you why.

This is where control characters come in. The most well-known control characters are `\n` (carriage return) or `\t` (horizontal tab). Less frequently used (or known) in PHP is the `NUL(L)` character (or zero-byte char), which can be written as `0x00`, `chr(0)` or `"\0"` in PHP. In the first mentioned examples, `evil-file.php\0.jpg` could be submitted as value for `$_POST['filename']`.

Such a file name will pass the whitelist and the blacklist checks but `evil-file.php` will be used as target file name by `file_put_contents()`.

In the include example, an attacker could use `../avatars/user-1234.jpg\0rest-doesnt-matter` to execute the contents of the `user-1234.jpg` file. This file could have been previously uploaded as the attacker's profile image in your imaginary web application or TYPO3 extension. Of course, the file could not contain image data, but malicious PHP code.

The remaining question is: Why do some functions, like the mentioned `preg_match()` or `substring()` work as expected and treat the `\0` as a normal character whilst other functions treat it as end of the string?

The reason is that PHP itself is programmed in C, which treats the `\0` as string terminator and ignores all following characters. And PHP relies on C's (or the operating system's) file system functions. That's why only file system functions are affected by this problem.

A list of affected functions and some further reading can e.g. be found at [madirish.net](http://www.madirish.net/?article=436).

So keep in mind to also check for NUL character, when dealing with user-input, especially when passed to file system functions:

{% highlight php %}
$filename = str_replace(chr(0), '', $filename);
{% endhighlight %}

or using the `[[:cntrl:]]` class in [regular expressions](http://php.net/manual/de/regexp.reference.character-classes.php) 

{% highlight php %}
if (preg_match('/[[:cntrl:]]/', $filename)) {
    return FALSE;
}
{% endhighlight %}

The TYPO3 API provides two static functions in t3lib_div for file name and path checks: 

* [`validPathStr($filename)`](http://api.typo3.org/typo3v4/current/html/classt3lib__div.html#a186b8fa7d4513e1fad65c06464d738a5) checks a whole path to neither contain `'..'` (directory traversal) nor the `NUL` character
* [`verifyFilenameAgainstDenyPattern()`](http://api.typo3.org/typo3v4/current/html/classt3lib__div.html#a13ae2fa3e811f12987110f0d6b23e17b) verifies a single file name against the `fileDenyPattern`, which is `\.(php[3-6]?|phpsh|phtml)(\..*)?$|^\.htaccess$` by default.

Usually this behavior to not react on control characters is called "binary-safe". So you think that file systems function that are explicitly marked as binary-safe in PHP manual do not expose the mentioned problems? Unfortunately not. Especially `file_put_contents()` is explicitly denoted to be binary-safe. However, this only holds for the to-be-written file contents but not for the `$filename` parameter. This parameter has to be checked and secured by you as the programmer!

