# Ruby Open Source Intelligence (OSINT)

## Introduction to OSINT
OSINT gathering allows security professionals to perform security assessment on an organization. You can use Ruby to automate many OSINT gathering tasks such as:
  - Domain Lookups
  - Reverse Lookups
  - Web Scraping
  - Email Address Collection

The following code snippets are examples of the collecting this information.

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

### Reverse Whois Lookup
In the following example we do a DNS lookup for a domain name. We then create a method that does a Whois lookup based on the IP address associated with the domain.
```ruby
1.9.3-p392 :027 > Resolv.getaddress('google.com')
 => "74.125.227.129" 
1.9.3-p392 :028 > def reverse_lookup(domain)
1.9.3-p392 :029?>   puts Whois.query(Resolv.getaddress(domain))
1.9.3-p392 :030?> end
 => nil 
1.9.3-p392 :031 > reverse_lookup('google.com')
```


## Using Nokogiri

### Fetching a webpage

```ruby
1.9.3-p392 :032 > require 'open-uri'
 => true 
1.9.3-p392 :033 > require 'nokogiri'
 => false 
1.9.3-p392 :034 > doc = Nokogiri::HTML(open('http://www.linkedin.com/in/janedoe'))
```

### Retrieving Elements
```ruby
1.9.3-p392 :038 > doc.css('span.given-name').text
 => "Jane" 
1.9.3-p392 :039 > doc.css('span.family-name').text
 => "Doe"
1.9.3-p392 :048 > doc.css('span.locality').text.strip
 => "Brooklyn, New York" 
```

### Retrieving PGP email addresses

First we create a ```fetch_pgp``` which fetches URL and places response in Nokogiri objects.
```ruby
 > def fetch_pgp(domain)
?>   Nokogiri::HTML(open("http://pgp.mit.edu:11371/pks/lookup?search=#{domain}&op=index&exact=on"
?> end

1.9.3-p392 :184 > doc = fetch_pgp('foo.com')
 => #<Nokogiri::HTML::Document:0x3ff3e115f148 name="document" ....
 ...
```

### Iterate and Parse PGP Keys
```ruby
1.9.3-p392 :189 > doc.css('a').each do |link|
1.9.3-p392 :190 >     puts link.content
1.9.3-p392 :191?> end
```
#### Results
Notice that we have unwanted data since we only want email address.

```ruby
12345678
John Doe <john_doe@foo.com>
23456789
Jane Doe <jane_doe@foo.com>
```

### Regex to grab foo related links
```ruby
1.9.3-p392 :193 >   doc.css('a').each do |link|
1.9.3-p392 :194 >     if link.content =~ /foo/
1.9.3-p392 :195?>       puts link.content
1.9.3-p392 :196?>     end
1.9.3-p392 :197?>   end
```
#### Results
```ruby
John Doe <john_doe@foo.com>
Jane Doe <jane_doe@foo.com>
```
### gsub to cleanup email addresses

```ruby
1.9.3-p392 :210 > email = ' <john_doe@foo.com>'

1.9.3-p392 :210 > email.gsub(/<|>/, "")
 => "john_doe@foo.com" 
```

### Create array of email addresses

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

## Shodan

```ruby

require 'shodan'
require 'json'

keys = JSON.parse(File.read('keys.json'))

api = Shodan::WebAPI.new(ENV['SHODAN_API'])

result = api.search('cisco ios')

result['matches'].each do |host|
    puts "#{host['ip']}"
end
```

## Twitter

```ruby
require 'twitter'

client = Twitter.configure do |config|
  config.consumer_key        = ENV['CONSUMER_KEY']
  config.consumer_secret     = ENV['CONSUMER_SECRET']
  config.oauth_token        = ENV['OAUTH_TOKEN']
  config.oauth_token_secret = ENV['OAUTH_TOKEN_SECRET']
end

  search = client.user_search('microsoft')
  search.each do |user|
    puts "@#{user['screen_name']}"
  end
```

## Bing
```ruby
require 'net/http'

def search(query, offset)
  account_key = ENV['BING_KEY']

  # Old URL format
  url = "https://api.datamarket.azure.com/Data.ashx/Bing/SearchWeb/v1/Web?Query=%27#{query}%27&$top=50&$skip=#{offset}&$format=json"

  # New URL format
  #url = "https://api.datamarket.azure.com/Bing/Search/v1/Web?Query=%27#{query}%27&$top=50&$skip=#{offset}&$format=json"

  uri = URI.parse(url)
  req = Net::HTTP::Get.new(uri.request_uri)
  req.basic_auth '', account_key
  res = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https'){|http| http.request(req)}
  res.body
end
```


