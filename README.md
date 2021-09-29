# haktrails

haktrails is a Golang client for querying SecurityTrails API data, sponsored by SecurityTrails.

<p align="center">
  <img height="300" src="./haktrails.png">
</p>

## Tool Features

- stdin input for easy tool chaining
- subdomain discovery
- associated root domain discovery
- associated IP discovery
- historical DNS data
- historical whois data
- [DSL queries](https://docs.securitytrails.com/docs/how-to-use-the-dsl) (currently a prototype)
- company discovery (discover the owner of a domain)
- whois (returns json whois data for a given domain)
- ping (check that your current SecurityTrails configuration/key is working)
- usage (check your current SecurityTrails usage)
- "json" or "list" output options for easy tool chaining
- "ZSH & Bash autocompletion"

## Installation

You will need a SecurityTrails API key to use this tool. If you're using it for bug bounties, I'd recommend checking out the [bug bounty hunter's toolkit](http://securitytrails.com/bug-bounty-hunters-toolkit?referral_code=LLDAK0F80M) if you're a bug bounty hunter. It provides access to the majority of data that you will need for a good price. See the details below.

<a href="http://securitytrails.com/bug-bounty-hunters-toolkit?referral_code=LLDAK0F80M">
<p align="center">
  <img src="./securitytrails-banner.jpg">
</p>
</a>

Once you have an API key, install golang, then:

```
go get github.com/hakluke/haktrails
~/go/bin/haktrails
```

I'd recommend adding `~/go/bin/` to your `$PATH` if you haven't already, then you can just run `haktrails`. 

## Autocompletion (optional)
ZSH and Bash autocompletion is available 
Just add this to your ~/.zshrc or ~/.bashrc 
```
source ~/go/src/github.com/hakluke/haktrails/haktrails-completion.zsh
or
source ~/go/src/github.com/hakluke/haktrails/haktrails-completion.bash
```
NOTE: If you are using a custom GOPATH location,
use it instead of the default one (which is ~/go)

## Usage

### Note

> Note: In these examples, domains.txt is a list of root domains that you wish to gather data on. For example:

```
hakluke.com
bugcrowd.com
tesla.com
yahoo.com
```

### Flags

- The *output* type can be specified with `-o json` or `-o list`. List is the default. List is only compatiable with subdomains, associated domains and associated ips. All the other endpoints will return json regardless.
- The number of threads can be set using `-t <number>`. This will determine how many domains can be processed at the same time. It's worth noting that the API has rate-limiting, so setting a really high thread count here will actually slow you down.
- The config file location can be set with `-c <file path>`. The default location is `~/.config/haktools/haktrails-config.yml`. A sample config file can be seen below.
- The lookup type for historical DNS lookups can be set with `-type <type>`, available options are a,aaaa,mx,txt,ns,soa.
- The DSL query can be set with `-query <query>`. See [here](https://docs.securitytrails.com/docs/how-to-use-the-dsl) for more details.

### Config file

You will need to set up a configuration file with your SecurityTrails key to use this tool. By default, the tool will look for the file in `~/.config/haktools/haktrails-config.yml`. If you wish to put the config file somewhere else, the location must be specified with the `-c` flag.

The format of the file is very simple, just copy paste this, and replace `<yourkey>` with your SecurityTrails API key:

```
securitytrails:
  key: <yourkey>
```

### Warning

> Warning: With this tool, it's very easy to burn through a lot of API credits. For example, if you have 10,000 domains in domains.txt, running `cat domains.txt | haktrails subdomains` will use all 10,000 credits. It's also worth noting that some functions (such as associated domains) will use multiple API requests, for example, `echo "yahoo.com" | haktrails associateddomains` would use about 20 API requests, because the data is paginated and yahoo.com has a _lot_ of associated domains.

### Gather subdomains

This will gather all subdomains of all the domains listed within domains.txt.

```
cat domains.txt | haktrails subdomains
```

Of course, a single domain can also be specified like this:

```
echo "yahoo.com" | haktrails subdomains
```

### Gather associated domains

"Associated domains" is a loose term, but it is generally just domains that are owned by the same company. This will gather all associated domains for every domain in domains.txt

```
cat domains.txt | haktrails associateddomains
```

### Gather associated IPs

Again, associated IPs is a loose term, but it generally refers to IP addresses that are owned by the same organisation.

```
cat domains.txt | haktrails associatedips
```

### Get historical DNS data

Returns historical DNS data for a domain.

```
cat domains.txt | haktrails historicaldns
```

### Get historical whois data

Returns historical whois data for a domain.

```
cat domains.txt | haktrails historicalwhois
```

### Run a DSL query

Runs a custom SecurityTrails DSL query. See [here](https://docs.securitytrails.com/docs/how-to-use-the-dsl) for more details.

```
haktrails dsl -query <query>
```

### Get company details

Returns the company that is associated with the provided domain(s).

```
cat domains.txt | haktrails company
```

### Get domain details

Returns all details of a domain including DNS records, alexa ranking and last seen time.

```
cat domains.txt | haktrails details
```

### Get whois data

Returns whois data in JSON format.

```
cat domains.txt | haktrails whois
```

### Get domain tags

Returns "tags" of a specific domain.

```
cat domains.txt | haktrails tags
```

### Usage

Returns data about API usage on your SecurityTrails account.

```
haktrails usage
```

### Ping

Pings SecurityTrails to check if your API key is working properly.

```
haktrails ping
```

### Banner

Shows a nice ascii-art banner :)

```
haktrails banner
```

## Not Yet Supported

Currently, some of the features of the SecurityTrails API are not yet supported. Pull requests are welcome!

- Scroll
- Domains Search
- Domains Statistics
- SSL Certificates (Stream)
- SSL Certificates (Pages)
- IP Neighbours
- IP Statistics
- IP Whois
- IP Useragents
- Domains feed
- Domains DMARC feed
- Domains subdomains feed
- Certificate transparency firehose

## SecurityTrails API Reference

The full API reference is [here](https://docs.securitytrails.com/reference).
