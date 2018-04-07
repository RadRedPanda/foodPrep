---
title: foodPrep Demo
---
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
table{
    table-layout: fixed;
    width: 800px; 
}
.dropbtn {
    background-color: #4CAF50;
    color: white;
    padding: 16px;
    font-size: 16px;
    border: none;
}

.dropdown {
    position: relative;
    display: inline-block;
}

.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f1f1f1;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    z-index: 1;
}

.dropdown-content a {
    color: black;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
}

.dropdown-content a:hover {background-color: #ddd}

.dropdown:hover .dropdown-content {
    display: block;
}

.dropdown:hover .dropbtn {
    background-color: #3e8e41;
}
</style>
</head>
<body>
<h2>Welcome to foodPrep, to begin, click the button below.</h2><p>
<button type="button" onclick="sendData({test:'ok'})" id = "butt">Click Me!</button>
<table id="stuff">
	<tr>
		<th>Time</th>
		<th>Sunday</th>
		<th>Monday</th>
		<th>Tuesday</th>
		<th>Wednesday</th>
		<th>Thursday</th>
		<th>Friday</th>
		<th>Saturday</th>
	</tr>
	<tr>
		<th>12:00 AM</th>
		<th>Sunday</th>
		<th>Monday</th>
		<th>Tuesday</th>
		<th>Wednesday</th>
		<th>Thursday</th>
		<th>Friday</th>
		<th>Saturday</th>
	</tr>
</table>
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
			var obj = JSON.parse(XHR.responseText);
			var day;
			var hour;
			var timeTable = new Array(24).fill(new Array(8));
			tableText = '<tr><th>Time</th><th>Sunday</th><th>Monday</th><th>Tuesday</th><th>Wednesday</th><th>Thursday</th><th>Friday</th><th>Saturday</th></tr>';
			for(hour=0; hour<timeTable.length; hour++){
				tableText += '<tr><th>';
				if(hour == 0 || hour == 12){
					tableText += '12:00 ';
				}else{
					tableText += (hour % 12).toString() + ':00 ';
				}
				if(hour > 11){
					tableText += "PM";
				}else{
					tableText += "AM";
				}
				for(day=1; day<timeTable[hour].length; day++){
					tableText += '<th><div class="dropdown"><button class="dropbtn">' + obj[day-1][0] + '</button><div class="dropdown-content"><a>Link 1</a><a>Link 2</a><a>Link 3</a></div></div></th>';
				}
				tableText += '</tr>';
			}
			var myTable = document.getElementById('stuff');
			myTable.innerHTML = tableText;
			//obj[0][0].name is first food of first day
		});

		// Define what happens in case of error
		XHR.addEventListener('error', function(event) {
			alert('Oops! Something went wrong.');
		});

		// Set up our request
		XHR.open('POST', 'https://mealplan.mccarty.io/foodplan');

		// Add the required HTTP header for form data POST requests
		XHR.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

		// Finally, send our data.
		XHR.send(urlEncodedData);

	}
</script>