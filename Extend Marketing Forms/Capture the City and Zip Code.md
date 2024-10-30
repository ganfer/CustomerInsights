# Capture the City and Zip Code.md

Forms on websites are often designed to allow users to input information relevant to website interaction or further communication. In addition to basic details such as name, email address, and phone number, it's also possible to capture a user's location, specifically their city and postal code. This can be useful for providing personalized services or location-based content.
A JavaScript script can be used to automatically capture a user's city and postal code without requiring manual input. This is achieved using the Geolocation API and an external geolocation source, such as OpenStreetMap.


```
<script>
document.addEventListener("d365mkt-afterformload", function() {
  var cityField = document.querySelector("input[name='address1_city']");
  var zipField = document.querySelector("input[name='address1_postalcode']");

  if ("geolocation" in navigator) {
    navigator.geolocation.getCurrentPosition(function(position) {
      var latitude = position.coords.latitude;
      var longitude = position.coords.longitude;

      // Use Geolocation API from openstreetmap
      var geolocationApiUrl = `https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}`;

      fetch(geolocationApiUrl)
        .then(response => response.json())
        .then(data => {
          var city = data.address.city;
          var zip = data.address.postcode;
          console.log("City: " + city);
          console.log("ZIP: " + zip);
          cityField.value = city;
          zipField.value = zip;
        })
        .catch(error => {
          console.error("Geolocation Error: " + error);
        });
    });
  } else {
    console.log("Geolocation not supported.");
  }
});
</script>

```

The script enables automated and user-friendly collection of location information within forms, enhancing accuracy and user convenience in website interactions.
