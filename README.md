# alertme-component-javascript

Component to display user alert subscription preferences.

## Overview
The alertme component is intended to be dropped onto an existing customer portal, enabling customers to subscribe to relevant business alerts.  e.g.
  - Banking
    - Alert me when my balance is below $x
    - Alert me when a transaction exceeds $x
  - eCommerce
    - Alert me when my order ships
    - Alert me of new products
  - Service provider
    - Alert me of planned outages
    - Alert me when my usage limit is near.
  - etc.

The actual business alert topics are configured via an administration site.  This component displays those topics and collects the preferences for each customer.

## Installation
Grab the latest javascript file from the lib directory and include it with your site assets. 

## Usage
On the page where you want to display the component, simply include an html tag for <alertme-preference-center>, and also include the js file at the bottom of the page. 

```html
<div>Some html</div>
<alertme-preference-center publisher="test.com" token="token1"></alertme-preference-center>

<div>Some more html</div>

<script type="text/javascript" src="alertme-1.0.4.js"></script>
```

Included in the tag is a publisher parameter.  This is the that you chose when signing up for the alertme service. (see signing up below)

The token parameter is a user security token retrieved by a server-side API call using the server-side technology of your choice.  The API call is a HTTP GET request to https://api.alertmehub.com/api/v1/subscriber/token/[userid]. 

   - Set the Authorization header to your alertme API key (see below)
   - The userid can be any string that uniquely identifies the user who is currently logged into your  portal - such as a customer id, account id, or any unique value such as a hash.

Example server-side code to retrieve an Alertme token.

ASP.NET MVC
``` C#
   
        public async Task<IActionResult> Alerts()
        {
            string tokenUrl = "https://api.alertmehub.com/api/v1/subscriber/token/" + User.Identity.Name;
            using (var httpClient = new HttpClient())
            {
                httpClient.DefaultRequestHeaders.Add("Authorization", "4f0c8355d0107f3e4b4705b85085506a4567debfb0842d4b2ab1ee38dcd62ac5");

                try
                {
                    string response = await httpClient.GetStringAsync(tokenUrl);
                    ViewBag.CustomerToken = response.Replace("\"", "");
                }
                catch(HttpRequestException ex)
                {
                    ViewBag.Error = ex.Message;

                }

            }

            return View();
        }
```

**It is important that the Alertme API key be treated as a secret and not exposed in client-side code.**  

## Alertme Registration
In order to use Alertme, you must first register at https://admin.alertmehub.com. 

You'll pick a publisher id - typically identified by the domain that your customer portal runs on - e.g. mycompany.com, or customerportal.mycompany.com. After registering you'll be provided with an API key which you can use to obtain user tokens as described in the previous section.

## Configure Alert Topics
The alert topics that your users can subscribe to are managed via the Alertme administration site.  https://admin.alertmehub.com

