---
title: foodPrep Demo
---
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
table{
    table-layout: fixed;
    width: 750px; 
}
.dropbtn {
    background-color: #4CAF50;
    color: white;
    padding: 2px;
    font-size: 16px;
    border: none;
	width: 80px;
	height: 20px;
	white-space: nowrap;
}

.dropdown {
    position: relative;
    display: inline-block;
}

.dropdown-content {
    display: none;
    position: absolute;
	white-space: nowrap;
	right: 0;
    background-color: #f1f1f1;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    z-index: 1;
}

.dropdown-content b {
    color: black;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
}

//.dropdown-content a:hover {background-color: #ddd}

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
</table>
<script>
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
			tableText += '<th>None</th>';
		}
		tableText += '</tr>';
	}
	var myTable = document.getElementById('stuff');
	myTable.innerHTML = tableText;

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
			/*var day;
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
					tableText += '<th>None</th>';
				}
				tableText += '</tr>';
			}*/
			var myTable = document.getElementById('stuff');
			//myTable.innerHTML = tableText;
			for(day = 1; day < timeTable[0].length; day++){
				for(hour = 0; hour < obj[day-1].length; hour++){
					tableText = '<th><div class="dropdown"><button class="dropbtn"><center>';
					tableText += obj[day-1][hour].name;
					tableText += '</center></button><div class="dropdown-content">';
					tableText += '<center><b>' + obj[day-1][hour].name + '</b></center>';
					tableText += '<b>Preparation Time: ';
					tableText += obj[day-1][hour].preptime.toString();
					tableText += ' minutes</b><b>Ingredients:</b>';
					var numIng;
					for(numIng = 0; numIng < obj[day-1][hour].ingredients.length; numIng++){
						tableText += '<b>&emsp;&#8226;';
						tableText += obj[day-1][hour].ingredients[numIng].name;
						tableText += '</b>';
					}
					tableText += '<b>Steps:</b>';
					var numStep;
					for(numStep = 0; numStep < obj[day-1][hour].steps.length; numStep++){
						tableText += '<b>&emsp;' + (numStep + 1).toString() + ': ';
						tableText += obj[day-1][hour].steps[numStep];
						tableText += '</b>';
					}
					
					tableText += '</div></div></th>';
					myTable.rows[obj[day-1][hour].time].cells[day].innerHTML = tableText;
				}
			}
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