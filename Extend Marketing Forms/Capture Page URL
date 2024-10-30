Frequently, harnessing the capability to seamlessly embed the web URL directly within the marketing form not only brings forth advantages but also facilitates the meticulous tracing of the origin of form submissions. 
Incorporating this feature becomes a logical step towards capturing essential web URLs. Here's a concise guide on how to seamlessly integrate them into your marketing form.

## Add a column on Lead Entity
Visit the website Power Apps  in order to initiate the creation of a fresh solution. Following that, advance by incorporating a novel column into the "Lead" entity. Be sure to make a record of its logical name for future reference.

Once you have established this field, you can directly integrate it into the form.

## Create a marketing form
Set up a form with the newly created field. You can hide it after your testing phase.

## Implement Code
'''<script>  
 var weburl = document.querySelector('input[name="cre25_weburl"]');
        // Check if the Input-Feld was found
        if (weburl) {
            // Set the Value of the Input-Felds as the actual Web-URL
            weburl.value = window.location.href;
        }
</script>'''

> [!IMPORTANT]
> **Attention**: Add your logical name of the field instead of the one here → Change cre25_weburl to your own Field

## Variant 1: Add Code in Form
Open the HTML Editor in the top right side and paste the code bevor the closing head tag </head> like it is shown in the picture here:

## Variant 2: Insert Code on the Website
If your website hosts multiple forms, it's advisable to publish the JavaScript code once and apply it site-wide. You can achieve this by utilizing Code Plugins like WordPress's 'Header and Footer Code Snippets,' or by directly inserting the code into the header of your website's template. This approach ensures that the JavaScript code is implemented consistently across all your forms, enhancing efficiency and maintaining a unified user experience.
Because the Script needs an Event to start, the code should looks like this:
 ``` <script>
document.addEventListener("d365mkt-afterformload", function() { 
	var weburl = document.querySelector('input[name="cre25_weburl"]');

        // Überprüfe, ob das Input-Feld gefunden wurde
        if (weburl) {
            // Setze den Wert des Input-Felds auf die aktuelle Web-URL
            weburl.value = window.location.href;;
        }
});
</script> ```
 ```bash
python run.py
```
