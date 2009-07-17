NANO - jQuery Template Engine
=============================

***Basic Usage***

Assuming you have following JSON response:

<pre>
data= {
  user: {
    login: "tomek",
    first_name: "Thomas",
    last_name: "Mazur",
    account: {
      status: "active",
      expires_at: "2009-12-31"
    }
  }
}  
</pre>

you can make:

<code>
  $.nano("&lt;p&gt;Hello {user.first_name} {user.last_name}! Your account is &lt;strong&gt;{user.account.status}&lt;/strong&gt;&lt;/p&gt;", data)
</code>

and you get ready string:

<code>
  &lt;p&gt;Hello Thomas! Your account is &lt;strong&gt;active&lt;/strong&gt;&lt;/p&gt;
</code>

Simple huh?

***Advanced Usage***

If you want to replace some strings in the HTML already on the page, use nano like this:

<code>
  $("#template-element").nano(data)
</code>

You can also let nano load an external HTML-file to use as a template:

<code>
  $.nano.html("http://example.com/template.html", data)
</code>

This can be useful if your templates are a bit more complex or when you don't want to write the template code directly in your Java Script code. Nano even caches the template code for you, so there won't be unnecessary web requests.

By default nano works with &lt;div&gt; elements. If your remote template is used in place of, say, a &lt;tr&gt;-element, this can lead to trouble. But not to worry: just pass in the necessary element type with the options hash like so:

<code>
  $.nano.html("http://example.com/template.html", data, {element: "tr"})
</code>

***More Advanced Example***


Displaying list of twitter search results (JSONP API)

**html**

<pre>
  &lt;ul id=&quot;tweets&quot;&gt;&lt;/ul&gt;
</pre>

**javascript**

<pre>  
  var template = "&lt;li&gt;&lt;strong&gt;@{from_user}&lt;/strong&gt; {original_text}&lt;/li&gt;"
  var query = "beer OR vodka"
  var container = $("ul#tweets")

  $.getJSON("http://search.twitter.com/search.json?callback=?", {
      q: query
    }, function(data) {
      container.html("")
      $.each(data.results), function(i, item){
        container.append($.nano(template, item))
      }
    }
  }
</pre>
