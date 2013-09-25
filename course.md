# Ruby Open Source Intelligence (OSINT)

## Introduction


### Requiring gems

```ruby
1.9.3-p392 :002 > require 'resolv'
 => true 
1.9.3-p392 :004 > require 'nokogiri'
 => true 
1.9.3-p392 :007 > require 'whois'
 => true 
```

### Whois lookup by domain

```ruby
1.9.3-p392 :008 > w = Whois::Client.new
 => #<Whois::Client:0x007fe7c22f0088 @timeout=10, @settings={}> 
1.9.3-p392 :009 > w.lookup('google.com')
 =>  ...
```
### Whois lookup by IP Address
```ruby
1.9.3-p392 :010 > w = Whois::Client.new
 => #<Whois::Client:0x007fe7c22f0088 @timeout=10, @settings={}> 
1.9.3-p392 :011 > w.lookup('8.8.8.8')
 =>  ...
```

#### Using Nokogiri

##### Fetching a webpage

```ruby
1.9.3-p392 :032 > require 'open-uri'
 => true 
1.9.3-p392 :033 > require 'nokogiri'
 => false 
1.9.3-p392 :034 > doc = Nokogiri::HTML(open('http://www.linkedin.com/in/janedoe'))
```

##### Retrieving Elements
```ruby
1.9.3-p392 :038 > doc.css('span.given-name').text
 => "Jane" 
1.9.3-p392 :039 > doc.css('span.family-name').text
 => "Doe"
1.9.3-p392 :048 > doc.css('span.locality').text.strip
 => "Brooklyn, New York" 
```

##### Retrieving PGP email addresses

First we create a ```fetch_pgp``` which fetches URL and places response in Nokogiri objects.
```ruby
 > def fetch_pgp(domain)
?>   Nokogiri::HTML(open("http://pgp.mit.edu:11371/pks/lookup?search=#{domain}&op=index&exact=on"
?> end

1.9.3-p392 :184 > doc = fetch_pgp('foo.com')
 => #<Nokogiri::HTML::Document:0x3ff3e115f148 name="document" ....
 ...
```

##### Iterate and Parse PGP Keys
```ruby
1.9.3-p392 :189 > doc.css('a').each do |link|
1.9.3-p392 :190 >     puts link.content
1.9.3-p392 :191?> end
```
###### Results
Notice that we have unwanted data since we only want email address.

```ruby
12345678
John Doe <john_doe@foo.com>
23456789
Jane Doe <jane_doe@foo.com>
```

###### Regex to grab foo related links
```ruby
1.9.3-p392 :193 >   doc.css('a').each do |link|
1.9.3-p392 :194 >     if link.content =~ /foo/
1.9.3-p392 :195?>       puts link.content
1.9.3-p392 :196?>     end
1.9.3-p392 :197?>   end
```
###### Results
```ruby
John Doe <john_doe@foo.com>
Jane Doe <jane_doe@foo.com>
```
###### gsub to cleanup email addresses

```ruby
1.9.3-p392 :210 > email = ' <john_doe@foo.com>'

1.9.3-p392 :210 > email.gsub(/<|>/, "")
 => "john_doe@foo.com" 
```

###### Create array of email addresses

```ruby
1.9.3-p392 :220 > emails = []
 => [] 
1.9.3-p392 :221 > doc.css('a').each do |link|
1.9.3-p392 :222 >     if link.content =~ /foo/
1.9.3-p392 :223?>       emails << link.content.split.last.gsub(/<|>/, "")
1.9.3-p392 :224?>     end
1.9.3-p392 :225?>   end
 => 0 
```

