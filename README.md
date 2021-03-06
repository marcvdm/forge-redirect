# Redirect all traffic with Laravel Forge
I often need a way do redirect all traffic from for example site domain.eu to domain.com. But I want domain.eu to work with a LetsEncrypt SSL certificate too so that site must be hosted somewhere. Since I use forge to host many sites it made sense to me to also create a site for domain.eu and redirect everything to the .com variant. So I created this very small PHP script and have it on github so I can easily deploy this every time I create a new site on forge just for redirecting purposes.
After installing all I need to do is to add the right domainname to the .env file which is easy to edit within Laravel Forge.

# Usage
Create a site on forge and add the GitHub Repository nickdekruijk/forge-redirect  
After installation create a LetsEncrypt certificate.  
Finally add at least REDIRECT_TO to your .env file or copy and edit this [.env.example](.env.example):
```php
# Redirect to this URL, may include subdomains or subfolders
REDIRECT_TO="https://redirecttodomain.com/"

# Append the server REQUEST_URI to REDIRECT_TO so old.com/test is redirected to new.com/test
# When false (default) all traffic will be redirected to the root of https://redirecttodomain.com/
REDIRECT_WITH_REQUEST_URI=false
```

# Frequently Asked Questions

## Why not use the built in redirect functions then?
If you redirect the root / to something else with Forge it breaks the LetsEncrypt ACME validation since that validation URL is being redirected too.

## Why not add old domain as an alias in the new site?
Then I would need to include all the domains that need to be redirected in the same SSL certificate and then everybody can see all the redirect domain names inside that certificate which might be bad for many reasons.
And because I don't want duplicate content on all the domains I need to manually redirect all domains to the main one from within the application too.

## How can I make some redirect exclusions? For example old.com/contact should go to new.com/about but not everything
Just use the Forge built in redirect functionality for that. They work well together!
