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

#### Using Whois gem

```ruby
1.9.3-p392 :008 > w = Whois::Client.new
 => #<Whois::Client:0x007fe7c22f0088 @timeout=10, @settings={}> 
1.9.3-p392 :009 > w.lookup('google.com')
```



