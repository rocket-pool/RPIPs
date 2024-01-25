# Rocket Pool Improvement Proposals (RPIPs)

Similar to the [Ethereum Improvement Proposals (EIPs) repository](https://github.com/ethereum/EIPs), this repository contains Rocket Pool Improvement Proposals (RPIPs).

For easy viewing of existing RPIPs, please see the [RPIP dashboard](https://rpips.rocketpool.net/).
The dashboard is rebuilt via a github action and is automatically published when the repo changes. Note that it is possible for this build to fail, so it should be checked when making changes. 


## Local Development

Follow these steps to setup a local server:

1. Fork the repo
1. Clone it locally
1. Install dependencies: `bundle install`
1. Start the local server: `bundle exec jekyll serve`
1. Go to <http://localhost:4400/> to view changes

To build the site use `bundle exec jekyll build`.

Notes:
* You want Ruby version 2.7 rather than 3+

Resources:

- [Jekyll Docs](https://jekyllrb.com/docs/)
- [Liquid Syntax](https://shopify.github.io/liquid/basics/introduction/)

