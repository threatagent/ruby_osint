# Ruby Open Source Intelligence (OSINT)

## Course Setup

### Required Rubygems

#### Whois
```bash
$ gem install whois
```
#### Nokogiri
```bash
$ gem install nokogiri
```

#### Twitter

```bash
$ gem install twitter
```

#### Shodan

```bash
$ gem install shodan
```

### Registering for APIs

* [Bing API key](http://www.bing.com/dev/en-us/dev-center)
* [Twitter access token](https://dev.twitter.com/docs/api)
* [Shodan API key](http://www.shodanhq.com/api_doc)

### Bing API Setup

Once you logged into http://www.bing.com/dev/en-us/dev-center
- Click on "Search API" and navigate to "My Account" > "My Data".  
- From there you can click the "Use" link to navigate to https://datamarket.azure.com/dataset/explore/bing/search.  

### Environment variables (optional)
You can export environment variables multiple ways.
The easiest way is to create a `~/.bash_env` file that you source in your `~/.bashrc`.

```bash
# Source the bash env file if it's there
echo "if [ -f ~/.bash_env ]; then source ~/.bash_env; fi" >> ~/.bashrc

# Append each key and ensure that you *export* them
echo "export SHODAN_API_KEY='your_api_key'" >> ~/.bash_env
```

The two alternatives are to create a `environment.sh` file that exports the variables
using `source /path/to/your/environment.sh`. This is useful for test credentials that are shared among developers.
