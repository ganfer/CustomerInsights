# Honeypot
The term "honeypot" is also used in the field of web development and form protection. In this context, it is an invisible form field (honeypot field) that is invisible to human users but is filled in by automated bots. By checking the honeypot field, the website can recognize whether the form has been filled out by a bot or a real user.

To be honest, I've never seen Dynamics for Marketing / Customer Insights - Journeys forms being abused by spambots - but better safe than sorry!
So I created a "Honeypot" field on my form. I also wrote a little JavaScript that blocks the send event as soon as the field is filled and voila - I have a honeypot field on my own form. Spambots, prepare yourselves!

```
document.addEventListener("d365mkt-formsubmit", function(event) {
    var honeypot = document.querySelector('input[name="fga_honeypot"]');

    if (honeypot && honeypot.value !== '') {
        event.preventDefault();
        console.log("Please stop spamming!");
        return;
    }
});
```

> [!NOTE]
>  In this case I created the Field `fga_honeypot` as the honeypot field. Please change this to your field
