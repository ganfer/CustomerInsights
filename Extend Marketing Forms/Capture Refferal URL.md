# Capture Refferal URL
Please have a look at this [Capture URL Guide](/Extend%20Marketing%20Forms/Capture%20Page%20URL.md) and make some different changes

1. Create a Column on Lead entity called “Refferal URL”
2. Set this column in the Form
3. Add the code below
4. Enjoy

```
 var refurl = document.querySelector('input[name="cre25_referrerurl"]');
        // Überprüfe, ob das Input-Feld gefunden wurde
        if (refurl) {
            // Setze den Wert des Input-Felds auf die aktuelle Web-URL
            refurl.value = document.referrer;
        }

```

> [!IMPORTANT]
> **Attention**: Don`t forget to adjust the logical name in the code
