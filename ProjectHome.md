## Overview ##
Lua binding for the SHA-256/384/512 BSD-licensed C implementation by Aaron Gifford[[1](http://www.aarongifford.com/computers/sha.html)].<br>Also contains the generic HMAC implementation per <a href='http://tools.ietf.org/html/rfc2104'>rfc2104</a> plus HMAC-SHA256/384/512 variants, and HMAC-MD5 (requires <a href='http://www.keplerproject.org/md5/'>md5</a>).<br>
<br>
<h2>Status</h2>
<ul><li>v0.2<br>
</li><li>for Lua 5.1<br>
</li><li>SHA tested with original test vectors on Win32 and 32bit and 64bit Linux<br>
</li><li>HMAC-SHA tested with rfc4231 test vectors on Win32 and 32bit and 64bit Linux</li></ul>

<h2>Changelog</h2>
<ul><li>v0.2 - (26 Nov 2010) added HMAC modules and hex output variants<br>
</li><li>v0.1 - (24 Nov 2010) initial release</li></ul>

<h2>Build & Install</h2>
<ul><li>Windows 32bit: install precompiled binary<br>
<ul><li><code>&gt; luarocks install sha2</code></li></ul></li></ul>

<ul><li>Windows 32bit: build using VC++ via <code>LuaRocks</code>
<ul><li>copy <a href='http://msinttypes.googlecode.com/svn/trunk/inttypes.h'>inttypes.h</a> to your VC++ include dir<br>
</li><li>open up a VC++ command-line environment<br>
</li><li><code>&gt; luarocks install sha2</code></li></ul></li></ul>

<ul><li>Windows 32bit: build with MinGW<br>
<ul><li>checkout the sources: <code>&gt;git clone https://cosmin.apreutesei@code.google.com/p/sha2/</code>
</li><li>edit <code>build-mingw.bat</code> and change LUA_INC and LUA_LIB<br>
</li><li><code>&gt; build-mingw.bat</code>
</li><li>copy <code>sha2lib.dll</code> somewhere in your <code>package.cpath</code></li></ul></li></ul>

<ul><li>Linux 32bit & amd64: build with GCC via <code>LuaRocks</code>
<ul><li><code>$ luarocks install sha2</code></li></ul></li></ul>

After installation, please run <code>test_sha2.lua</code> and <code>test_hmac.lua</code> (the later needs <code>LuaSocket</code> and <code>md5</code>).<br>
<br>
<h2>Usage</h2>
<pre><code>local function bintohex(s)<br>
  return (s:gsub('(.)', function(c)<br>
    return string.format('%02x', string.byte(c))<br>
  end))<br>
end <br>
<br>
require "sha2"<br>
<br>
print(sha2.sha256hex("And so we say goodbye to our beloved pet, Nibbler, who's gone to a place where I, too, hope one day to go. The toilet."))<br>
-- prints 3c4ba860b4917a85b075f5e0c8cebe65bd1646d0d5ac3326a974ae965a44a5e1<br>
<br>
require "hmac.sha2"<br>
<br>
print(bintohex(hmac.sha256("what do ya want for nothing?", "Jefe"))) --Jefe is the key<br>
-- prints 5bdcc146bf60754e6a042426089575c75a003f089d2739839dec58b964ec3843<br>
<br>
require "hmac"<br>
require "hmac.md5" --get md5 from LuaRocks<br>
<br>
print(bintohex(hmac.md5("what do ya want for nothing?", "Jefe"))) --Jefe is the key<br>
-- prints 750c783e6ab0b503eaa86e310a5db738<br>
</code></pre>

<h2>Reference</h2>
<pre><code>   require "sha2"<br>
<br>
   sha2.sha256(message) -&gt; SHA256 binary hash string<br>
   sha2.sha384(message) -&gt; SHA348 binary hash string<br>
   sha2.sha512(message) -&gt; SHA512 binary hash string<br>
<br>
   sha2.sha256hex(message) -&gt; SHA256 hex hash string<br>
   sha2.sha384hex(message) -&gt; SHA348 hex hash string<br>
   sha2.sha512hex(message) -&gt; SHA512 hex hash string<br>
<br>
   require "hmac"<br>
<br>
   hmac.compute(key, message, hash_function, blocksize, [opad], [ipad]) -&gt; HMAC string, opad, ipad<br>
   hmac.new(hash_function, block_size) -&gt; function(message, key) -&gt; HMAC string<br>
<br>
   require "hmac.sha2"<br>
<br>
   hmac.sha256(message, key) -&gt; HMAC-SHA256 binary string<br>
   hmac.sha348(message, key) -&gt; HMAC-SHA348 binary string<br>
   hmac.sha512(message, key) -&gt; HMAC-SHA512 binary string<br>
<br>
   require "hmac.md5"<br>
<br>
   hmac.md5(message, key) -&gt; HMAC-MD5 binary string<br>
</code></pre>

<h2>TODO</h2>
<ul><li>if a function is passed instead of a string, it will be called repeatedly and it's output will be consumed until it returns nil or nothing (that is, make it work with a LTN12 source).<br>
</li><li>talk about merging with md5 library.</li></ul>

<h2>Feedback</h2>

<ul><li><code>cosmin.apreutesei@gmail.com</code>
</li><li>feedback welcome! use the Issues tab for bug reporting, thanks!</li></ul>

<h2>Alternative implementations</h2>
<ul><li><a href='http://lua-users.org/wiki/SecureHashAlgorithm'>SHA256 in pure-Lua</a>, for Lua 5.2, using the new bit32 built-in library<br>
</li><li><a href='http://www.gammon.com.au/mushclient/mushclient.htm'>mushclient</a> contains a wrapper of a GPL'ed C implementation<br>
</li><li><a href='http://luacrypto.luaforge.net/'>LuaCrypto</a> can do SHA-2 as it binds to OpenSSL (not tested)<br>
</li><li><a href='http://www.eder.us/projects/lcrypt/'>lcrypt</a> can do SHA-2 as it binds to <a href='http://libtom.org/'>LibTom</a> (not tested)<br>
</li><li><a href='http://www.tecgraf.puc-rio.br/~lhf/ftp/lua/#lmd5'>lmd5</a> can do SHA-2 as it binds to OpenSSL (not tested)<br>
</li><li><a href='http://lua-users.org/files/wiki_insecure/users/tnodir/'>luamhash</a> can do SHA-2 as it binds to <a href='http://mhash.sourceforge.net/'>mhash</a> (not tested; Lua 5.0)