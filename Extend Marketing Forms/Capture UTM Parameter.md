# Capture UTM Parameter

UTM parameters (Urchin Tracking Module) are special codes appended to URLs to track the origin and effectiveness of online marketing campaigns. They enable precise analysis of website traffic, helping to understand which sources, media, or campaigns are directing visitors to your site. Here are key points about UTM parameters and their significance:

1. Source Tracking: UTM parameters help identify the source of traffic. This means you can determine whether visitors are coming from social media, email marketing, advertisements, or other sources.
2. Measuring Campaign Success: By using UTM parameters, you can quantify the success of your marketing campaigns. You can see which campaigns generate the most clicks, conversions, or engagement.
3. Data-Informed Decisions: UTM parameters provide data-driven insights. You can identify which channels or platforms deliver the best results and adjust your resources accordingly.
4. Customization: UTM parameters allow you to customize URLs to track different campaigns or target audiences. You can use different parameters for various ads or channels.
5. Google Analytics Integration: Data captured with UTM parameters can be processed in tools like Google Analytics for more in-depth analysis. You can track conversions, bounce rates, time on site, and more.

### Common UTM parameters include:
- `utm_source`: The source of traffic (e.g., facebook, newsletter).
- `utm_medium`: The type of medium (e.g., cpc for cost-per-click, email for emails).
- `utm_campaign`: The name of the campaign or advertisement.
- `utm_term`: Keywords or terms that triggered the ad.
- `utm_content`: Different versions of an ad or detailed information.

In summary, UTM parameters offer an effective method to track, measure, and optimize the success of your online marketing efforts.

In this case we want to track the UTM Parameter at the form submission, so we have them saved on the leads and we can track them also in dynamics 365.

First of all we will create new colums on lead entity

![PowerApps](/img/Extend%20Marketing%20Forms/powerapps_utm.png)

Then we add those fields on a real time marketing form. While it's possible to designate them as hidden elements, ensuring that guests do not see them, for the purposes of this test, I won't implement this approach. Instead, I'll keep the fields visible to provide a more transparent user experience during the testing phase.

![Form](/img/Extend%20Marketing%20Forms/form_utm.png)

Publish the form and host it on your website. Afterwards publish following code on the website. This code will store the UTM Parameters in the local session Storage:

```
<script>
  var queryForm = function(settings){
    var reset = settings && settings.reset ? settings.reset : false;
    var self = window.location.toString();
    var querystring = self.split("?");
    if (querystring.length > 1) {
      var pairs = querystring[1].split("&");
      for (i in pairs) {
        var keyval = pairs[i].split("=");
        if (reset || sessionStorage.getItem(keyval[0]) === null) {
          sessionStorage.setItem(keyval[0], decodeURIComponent(keyval[1]));
        }
      }
    }
  }
</script>
```

> [!IMPORTANT]
> **Attention**: Keep in mind that you need the parameters stored in the session storage before the form loads. So implement an beforeformload or pipeline the javascript. At the End of the Guide there is a version you can use out of the box.

And this code will fill out the Form fields with the values of the utm parameters stored in local session storage.

```
<script>
document.addEventListener("d365mkt-afterformload", function() { 

  var utmCampaign = document.querySelector('input[name="cre25_utmcampaign"]');
        if (utmCampaign) {
            utmCampaign.value = sessionStorage.utm_campaign;
        }
  var utmSource = document.querySelector('input[name="cre25_utmsource"]');
        if (utmSource) {
            utmSource.value = sessionStorage.utm_source;
        }
  var utmMedium = document.querySelector('input[name="cre25_utmmedium"]');
        if (utmMedium) {
            utmMedium.value = sessionStorage.utm_medium;
        } 
});
</script>
```

Keep in mind, that you can easily update it with more variables like utm_term just by creating a new column on lead entity and adding more code in the forms code:

```
var utmTerm = document.querySelector('input[name="cre25_utmterm"]');
        if (utmMedium) {
            utmMedium.value = sessionStorage.utm_term;
        }
```

You dont have to add something in the first script → It will update it by it`s own.

![Browser](/img/Extend%20Marketing%20Forms/browser.png)


I found the script for parsing the UTM Parameters in the local session storage [here](https://www.gkogan.co/blog/save-url-parameters/))

## Script

```
<script>
document.addEventListener("d365mkt-beforeformload", function() { 
  var storeParameters = function(settings){
    var reset = settings && settings.reset ? settings.reset : false;
    var self = window.location.toString();
    var querystring = self.split("?");
    if (querystring.length > 1) {
      var pairs = querystring[1].split("&");
      for (var i in pairs) {
        var keyval = pairs[i].split("=");
        if (reset || sessionStorage.getItem(keyval[0]) === null) {
          sessionStorage.setItem(keyval[0], decodeURIComponent(keyval[1]));
        }
      }
    }
  }
  // Hier wird die storeParameters-Funktion aufgerufen
  storeParameters();
});
</script>
```

I added `d365mkt-beforeformload` as an Event so the parameters were stored in the session Storage before the form is loaded.

I have also developed a highly compressed code, ensuring swift loading performance:

```
<script>
document.addEventListener("d365mkt-beforeformload",function(){var e=function(e){var t=e&&e.reset?e.reset:!1,n=window.location.toString(),r=n.split("?");if(r.length>1){var i=r[1].split("&");for(var o in i){var a=i[o].split("=");(t||null===sessionStorage.getItem(a[0]))&&sessionStorage.setItem(a[0],decodeURIComponent(a[1]))}}};e()});
</script>
```

## Effects on Dynamics for Marketing
In Customer Insights Journeys you can set up UTM Parameters while sending out emails:

![Settings](/img/Extend%20Marketing%20Forms/utm_setting.png)

In this case, you don't have to include UTM parameters within the email itself. This is highly beneficial and can significantly streamline your workflow. When sending out an email with UTM parameters and a tracking parameter, the tracking parameter typically appears as `#msdynmkt_trackingcontext=53b4ed03-1ace-4842-a03f-554a90997676.` However, there could be a potential issue where the UTM script may also include the last UTM parameter. To ensure that the tracking parameter doesn't interfere with the UTM parameters, you can incorporate the following line of code, which will segment the URL after `#msdynmkt.`

```
    // Check if '#msdynmkt' is present in the URL
    if (self.indexOf('#msdynmkt') !== -1) {
      // Split the URL on '#msdynmkt'
      var urlParts = self.split("#msdynmkt");
      var querystring = urlParts[0]; // Get the part before '#msdynmkt'
```

## Final
So the whole Script will look like this:

```
<script>
document.addEventListener("d365mkt-beforeformload", function() { 
  var storeParameters = function(settings){
    var reset = settings && settings.reset ? settings.reset : false;
    var self = window.location.toString();
    
    // Check if '#msdynmkt' is present in the URL
    if (self.indexOf('#msdynmkt') !== -1) {
      // Split the URL on '#msdynmkt'
      var urlParts = self.split("#msdynmkt");
      var querystring = urlParts[0]; // Get the part before '#msdynmkt'
      
      // Check if there are query string parameters
      if (querystring.indexOf('?') !== -1) {
        querystring = querystring.split('?')[1]; // Get the part after '?'
        var pairs = querystring.split("&");
        for (var i in pairs) {
          var keyval = pairs[i].split("=");
          if (reset || sessionStorage.getItem(keyval[0]) === null) {
            sessionStorage.setItem(keyval[0], decodeURIComponent(keyval[1]));
          }
        }
      }
    }
  }
  // Here, the storeParameters function is called
  storeParameters();
});
</script>
```
There is also a minimized version of the script:

```
<script>
document.addEventListener("d365mkt-beforeformload", function() {
  var s = sessionStorage,
    u = window.location.toString(),
    i = u.indexOf("#msdynmkt");
  i !== -1 && (function(e) {
    for (var t, r = e.split("#msdynmkt"), n = r[0], a = n.indexOf("?"); a !== -1 && (n = n.split("?")[1], t = n.split("&"), e = 0, t[e]; e++) {
      var s = t[e].split("=");
      !0 || s[0]) === null && s[0])]);
      }
    }
  })(u);
});
</script>
```

# Alternative
I found an alternative way to do so. Please check out this nice blog:
[Capturing UTM Parameters Via Realtime Marketing Forms (meganvwalker.com)](https://meganvwalker.com/capturing-utm-parameters-via-realtime-marketing/))

This script can be exposed in the form HTML itself and will write N/A in empty form fields. So you have two ways to go, check your needs first. Also don`t forget to delete the log element.

 

I choosed the other way, because it will store the parameters in the session cookies and also if the contact is browsing on the website it will not be deleted. But on the other hand, it will be a bit slower for the forms loading. Thats because “before formload” will interrupt the form load, while “after formload” will not. So you have to check your needs before implementing the script.

```
<script>
document.addEventListener("d365mkt-afterformload", function() {
  function populateFormFields() {
    let params = new URLSearchParams(document.location.search);
    let referrerUrlField = document.querySelector('input[name="YOUR_FIELD_NAME"]');
    let utmSourceField = document.querySelector('input[name="YOUR_FIELD_NAME"]');
    let utmCampaignField = document.querySelector('input[name="YOUR_FIELD_NAME"]');
    let utmMediumField = document.querySelector('input[name="YOUR_FIELD_NAME"]');
    let utmContentField = document.querySelector('input[name="YOUR_FIELD_NAME"]');
    referrerUrlField.value = document.referrer;
    
    if (params.has("utm_source")) {
      utmSourceField.value = params.get("utm_source");
    } else {
      utmSourceField.value = 'N/A';
    }
    
    if (params.has("utm_campaign")) {
      utmCampaignField.value = params.get("utm_campaign");
    } else {
      utmCampaignField.value = 'N/A';
    }
    
    if (params.has("utm_medium")) {
      utmMediumField.value = params.get("utm_medium");
    } else {
      utmMediumField.value = 'N/A';
    }
    
    if (params.has("utm_content")) {
      utmContentField.value = params.get("utm_content");
    } else {
      utmContentField.value = 'N/A';
    }
  }

  populateFormFields();
  console.log("d365mkt-afterformload");
});

document.addEventListener("d365mkt-afterformsubmit", function(event) {
  console.log("success - " + event.detail.successful);
  console.log("payload - " + JSON.stringify(event.detail.payload));
});
</script>
```


