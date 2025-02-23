
# Introduction to HTML and CSS
Her website html ve css den oluşur, js ise bu html ve css i değiştirmek için kullanılır.

# Elements and Tags 

Hmtl element means : tags + content

```html
<p>some text content</p>
```
most of the elements has both opeining and closing tags, some may be void that doesnt have closing tags,
they are called void since they dont have content init like `<br>` or `<img>`. 


# HTML Boilerplate 

This is the boilerplate for the html file :
```html
<DOCTYPE! html>
<html lang="en">
    <head>
        <meta >
        <title> Seen on the tab </title>
    
    </head>

    <body>
        <h1> Hello World! </h1>
    </body>
</html>
```

DOCTYPE declaration is for the browser understand which html version is this code should interpreted.

html is the root element

head and body are the necessary parts

head keeps the important meta-information about our webpages, and stuff required for our webpages to render correctly in the browser. 



---
[HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML) (HyperText Markup Language) is a _markup language_ that tells web browsers how to structure the web pages you visit.


Tags in HTML are not case-sensitive. This means they can be written in uppercase or lowercase. For example, a [`<title>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title) tag could be written as `<title>`, `<TITLE>`, `<Title>`, `<TiTlE>`, etc., and it will work. However, it is best practice to write all tags in lowercase for consistency and readability.

Anatomy of a html element :
![[v3vmz26q.bmp]]



Not all elements follow the pattern of an opening tag, content, and a closing tag. Some elements consist of a single tag, which is typically used to insert/embed something in the document. (content leri yok) Such elemets are called "void elements" : 

```js
<img src="https://raw.githubusercontent.com/mdn/beginner-html-site/gh-pages/images/firefox-icon.png" alt="Firefox icon" />
```



Attributes :
![[lqvq40pk.bmp]]

An attribute should have:

- A space between it and the element name. (For an element with more than one attribute, the attributes should be separated by spaces too.)
- The attribute name, followed by an equal sign.
- An attribute value, wrapped with opening and closing quote marks.


Boolean attributes larda vardır :
Boolean attributes can only have one value, which is generally the same as the attribute name. shorthand olarak sadece disabled şeklinde de yazılabilir (aşağıda)
```js

<!-- using the disabled attribute prevents the end user from entering text into the input box -->
<input type="text" disabled="disabled" />
<input type="text" disabled />

<!-- text input is allowed, as it doesn't contain the disabled attribute -->
<input type="text" />
```


Attribute valularındaki quote omit edilebilir, fakat bu önerilmez, bazı elementlerin bazı attributelarında bu kullanım sıkıntı çıkarmasa da çoğunda çıkarır. bu yüzden herzaman attribute valularında quote kullanılmalı.

Attribute valularında single veya duble quote kullanılabilir. Örneğin duble quote kullanılmışsa, single quote valuenun içinde yazılabilir.
```html
<a href="https://www.example.com" title="Isn't this fun?">
  A link to my example.
</a>
```


whitspaces in html are optional. these two are identical :
the HTML parser reduces each sequence of whitespace to a single space when rendering the code.
```html
<p id="noWhitespace">Dogs are silly.</p>

<p id="whitespace">Dogs
    are
        silly.</p>
```



In HTML, the characters `<`, `>`,`"`,`'`, and `&` are special characters. They are parts of the HTML syntax itself. bu karakterleri html element contentinde kullanmak istiyorsak bu karakterlerin character references larını kullanırız  :

|Literal character|Character reference equivalent|
|---|---|
|<|`&lt;`|
|>|`&gt;`|
|"|`&quot;`|
|'|`&apos;`|
|&|`&amp;`|
```html

<p>In HTML, you define a paragraph using the &lt;p&gt; element.</p>

```
çıktı : 
```
In HTML, you define a paragraph using the <p> element.
```


You don't need to use entity references for any other symbols, as modern browsers will handle the actual symbols just fine as long as your HTML's [character encoding is set to UTF-8](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML#specifying_your_documents_character_encoding)


Html commentleri şöyle konulur :
```html
<p>I'm not inside a comment</p>

<!-- <p>I am!</p> -->
```

---

html elementi root elementtir, diğer herşey bu elementin childi dır.
```html
<html> </html>
```
----

```html

<!doctype html>

<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <link href="styles/style.css" rel="stylesheet" />
    <title>My test page</title>
  </head>

  <body>
  
    <h1>Welcome to Guessing Game</h1> 
    
    <ul>
        <li>technologists</li>
        <li>thinkers</li>
        <li>builders</li>
        <li>ismail</li>
        <li>cakmak</li>
    </ul>
    
    <button type="button"> Player 1: Chris </button>
    <button type="button"> Player 2: Chris </button>
    <button type="button"> Player 3: Chris </button>

    <a href="https://tureng.com/en/turkish-english/mark%20up"> WEbsite of Building</a>
    <img src="images/apple-17.png" alt="My test image" />
    <script src="scripts/main.js"></script>
  </body>

</html>
```



---


The [head](https://developer.mozilla.org/en-US/docs/Glossary/Head) of an HTML document is the part that is not displayed in the web browser when the page is loaded. Head de tutulan bilgiler metadatadır.

h1 ile title elemetleri sıklıkla karıştırılır : 
- The [h1](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) element appears on the page when loaded in the browser — generally this should be used once per page, to mark up the title of your page content (the story title, or news headline, or whatever is appropriate to your usage.)
- The [`<title>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title) element is metadata that represents the title of the overall HTML document (not the document's content.)

tabı bookmarklamak istediğimizde bookmarkın görünen ismi, title oluyor.



<,meta> elementi, html e metadata eklemek için kullanılır. Örneğin This element specifies the document's character encoding — the character set that the document is permitted to use : 
```html
<meta charset="utf-8" />
```

`utf-8` is a universal character set that includes pretty much any character from any human language.


meta elementi ayrıca 2 attribute içerir, name and content. Bu sayede istediğimiz herhangi bir bilgiyi metadataya ekleyebiliriz. Genelde author ve site hakkında descption bilgisi verilir. 
Specifying a description that includes keywords relating to the content of your page is useful as it has the potential to make your page appear higher in relevant searches performed in search engines (such activities are termed [Search Engine Optimization](https://developer.mozilla.org/en-US/docs/Glossary/SEO), or [SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO).)

```html
<meta name="author" content="Chris Mills" />
<meta
  name="description"
  content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing websites and applications." />

```

you can (and really should) set the language of your page. This can be done by adding the [lang attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang) to the opening HTML tag :
```html
<html lang="en-US">
  …
</html>
```
Böylece
- search result larda bu dil aramasında websayfan gösterilecek
- screen-reader kullanan kullanıcılarda sıkıntı çıkmayacak, "six" hem ing hem fransızcada var örneğin.



You can also set subsections of your document to be recognized as different languages. For example, we could set our Japanese language section to be recognized as Japanese, like so:

 ```html
<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>
```











