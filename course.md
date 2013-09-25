# Ruby Open Source Intelligence (OSINT)

## Introduction


### Required Rubygems

#### gem install whois
```bash
$ gem install whois
```
#### gem install nokogiri
```bash
gem install nokogiri
```


#### Requiring gems

```ruby
1.9.3-p392 :002 > require 'resolv'
 => true 
1.9.3-p392 :004 > require 'nokogiri'
 => true 
1.9.3-p392 :007 > require 'whois'
 => true 
```

#### Whois lookup by domain

```ruby
1.9.3-p392 :008 > w = Whois::Client.new
 => #<Whois::Client:0x007fe7c22f0088 @timeout=10, @settings={}> 
1.9.3-p392 :009 > w.lookup('google.com')
 =>  ...
```
#### Whois lookup by IP Address
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
1.9.3-p392 :034 > doc = Nokogiri::HTML(open('http://www.linkedin.com/in/janedoes'))
```





