# Ruby Open Source Intelligence (OSINT)

## Course Setup

### Required Rubygems

#### gem install whois
```bash
$ gem install whois
```
#### gem install nokogiri
```bash
$ gem install nokogiri
```

### Registering for APIs

* [Bing API key](http://www.bing.com/dev/en-us/dev-center)
* [Facebook access token](http://developers.facebook.com/docs/reference/api/)
* [Twitter access token](https://dev.twitter.com/docs/api)
* [Shodan API key](http://www.shodanhq.com/api_doc)

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
