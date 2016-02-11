---
layout: post
title: ruby certificate verify failed on Windows
tags:
- certificate
- TLS
- ruby
- Windows
published: true
---
### Reproduce the error
To reproduce in base ruby and AWS connections on Windows, run `irb`:

    require open-uri
    open https://s3.amazonaws.com

You will see a similar stack trace to
<http://stackoverflow.com/questions/9199660/why-is-ruby-unable-to-verify-an-ssl-certificate>.

Newer versions of Amazon SDKs include [ca-bundle.crt](https://github.com/aws/aws-sdk-core-ruby/issues/166)
from <https://curl.haxx.se/docs/caextract.html>.

### Partial Solution
1. Download the bundled version from <https://github.com/aws/aws-sdk-ruby/blob/master/aws-sdk-core/ca-bundle.crt>
2. Add a
[Windows _system variable_](https://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/sysdm_advancd_environmnt_addchange_variable.mspx)
named `SSL_CERT_FILE`, and the value should be the path - `C:\....\ca-bundle.crt`.

### Trouble on the Stripe front
I could `open https://api.stripe.com/v1` just fine, but for some reason I started getting these same
SSL verify thangs from Stripe when using their gem. Turns out they bundle ...
<https://github.com/stripe/stripe-ruby/blob/master/lib/data/ca-certificates.crt>

What worked for me was dropping into Cygwin and overwriting the Stripe bundle with the ca-certificates package bundle:

    cp /etc/ssl/certs/ca-bundle.crt /cygdrive/c/Ruby21-x64/lib/ruby/gems/2.1.0/gems/stripe-1.35.1/lib/data/ca-certificates.crt
