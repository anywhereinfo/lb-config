## VIP Configuration For Internet Facing Traffic



> > ### VIP (or Host ?): roos-ext-lb 
> >
> > ```
> > On: External A10
> > Protocol: SSL
> > LB: Layer 3 load balancing
> > VIP: <<public domain name for shopping & turnkey>>
> > Members:
> > roos-ext-apache-1
> > roos-ext-apache-2
> > roos-ext-apache-3
> > VIP: security.roosevelt.oidc
> > roos-sAuth-oidc-1
> > roos-sAuth-oidc-2
> > Inspects domain name and routes the request to either Apache members or Secure Auth members
> > ```
> >
> > > ### Host: roos-ext-apache-1
> > >
> > > ```
> > > On: External Apache
> > > Protocol: HTTPS
> > > Port: 443
> > > LB : Layer 7 Load balancing
> > > VLAN: Toolkit_Web_<ENV>_External
> > > SSL Termination: Yes, using THWATE certs
> > > SSL Origination: Yes, using Rendom certs
> > >
> > > <Proxy balancer://roos-ext-api-gw>
> > > BalanceMember https://roos-ext-api-gw-1/
> > > BalanceMember https://roos-ext-api-gw-2/
> > > BalanceMember https://roos-ext-api-gw-3/
> > > </Proxy>
> > >
> > > <Proxy balancer://roos-magnolia-publisher>
> > > BalanceMember https://roos-magnolia-publisher-1/
> > > BalanceMember https://roos-magnolia-publisher-2/
> > > </Proxy>
> > >
> > > <Proxy balancer://roos-search-provider>
> > > BalanceMember https://roos-search-provider-1/
> > > BalanceMember https://roos-search-provider-2/
> > > BalanceMember https://roos-search-provider-3/
> > > </Proxy>
> > >
> > > <Virtual Host : <<public domain name for shopping & turnkey>>:443>
> > > 	SSLEngine On
> > > 	ProxyPass: /api/shopping/allProducts balancer://roos-ext-api-gw/shopping/allProducts
> > >     ProxyPass: /api/shopping/products balancer://roos-ext-api-gw/shopping/products
> > >     ProxyPass: /api/shopping/turnKey balancer://roos-ext-api-gw/shopping/turnKey
> > > 	ProxyPass: / balancer://roos-magnolia-publisher
> > > 	ProxyPass: /search/ balancer://roos-search-provider
> > > </Virtual Host>
> > >
> > > Function: Load balances request across API Gateways over HTTPS. 
> > > Request arrives for the virtual host aka shopping domain or turnkey domain and request is load balanced to one of the gateways
> > > ```
> > >
> > > > ### Host: roos-ext-api-gw-1
> > > >
> > > > ```
> > > > On: External API GW
> > > > Protocol: HTTPS
> > > > Port: 443
> > > > VLAN: Application/API_<ENV>
> > > > SSL Termination: Yes, using Rendom certs
> > > > SSL Origination: Yes, using Rendom certs
> > > > Invokes VIP: 
> > > > roos-api-transform
> > > > roos-consul
> > > > roos-vault    
> > > >
> > > > ```
> > > >
> > > > > ### VIP: roos-vault
> > > > >
> > > > > ```
> > > > > On: Internal A10
> > > > > Protocol: SSL
> > > > > Members:
> > > > > roos-vault-1
> > > > > roos-vault-2
> > > > > roos-vault-3
> > > > > roos-vault-4
> > > > > roos-vault-5
> > > > > ```
> > > > >
> > > > > 
> > > > >
> > > > > ### VIP: roos-consul
> > > > >
> > > > > ```
> > > > > On: Internal A10
> > > > > Protocol: SSL
> > > > > Members:
> > > > > roos-consul-1
> > > > > roos-consul-2
> > > > > roos-consul-3
> > > > > ```
> > > > >
> > > > > ### VIP: roos-api-transform
> > > > >
> > > > > ```
> > > > > On: Internal A10
> > > > > Protocol: SSL
> > > > > Members:
> > > > > roos-api-transform-1
> > > > > roos-api-transform-2
> > > > > roos-api-transform-3
> > > > > ```
> > > > >
> > >
> > > 
> > >
> > > 
>
> 