---
title: foodPrep Demo
---

<h2>Welcome to foodPrep, to begin, click the button below.</h2><p>
<button type="button" onclick="sendData({test:'ok'})" id = "butt">Click Me!</button>
<script>
	function sendData(data) {
		var XHR = new XMLHttpRequest();
		var urlEncodedData = "";
		var urlEncodedDataPairs = [];
		var name;

		// Turn the data object into an array of URL-encoded key/value pairs.
		for(name in data) {
			urlEncodedDataPairs.push(encodeURIComponent(name) + '=' + encodeURIComponent(data[name]));
		}

		// Combine the pairs into a single string and replace all %-encoded spaces to 
		// the '+' character; matches the behaviour of browser form submissions.
		urlEncodedData = urlEncodedDataPairs.join('&').replace(/%20/g, '+');

		// Define what happens on successful data submission
		XHR.addEventListener('load', function(event) {
			alert('Yeah! Data sent and response loaded.');
		});

		// Define what happens in case of error
		XHR.addEventListener('error', function(event) {
			alert('Oops! Something went wrong.');
		});

		// Set up our request
		XHR.open('GET', 'https://mealplan.mccarty.io/foodplan');

		// Add the required HTTP header for form data POST requests
		XHR.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

		// Finally, send our data.
		//XHR.send(urlEncodedData);
		document.getElementById("butt").textContent="."
		alert(JSON.parse(XHR.responseText));
	}
</script>
