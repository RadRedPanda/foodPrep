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

.textInputs {
	width: 80px;
}

.truncated {
    display: block;
    white-space: nowrap; /* forces text to single line */
    overflow: hidden;
    text-overflow: ellipsis;
}

.empty {
	background-color: #BB1111;
	width: 84px;
	height: 23px;
}

.dropbtn {
    background-color: #4CAF50;
    color: white;
    padding: 0px;
    font-size: 16px;
    border: none;
	width: 84px;
	height: 23px;
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

.dropdown:hover .dropdown-content {
    display: block;
}

.dropdown:hover .dropbtn {
    background-color: #3e8e41;
}
</style>
</head>
<body>
<a href="/foodPrep/about">About</a>
<h2>Welcome to foodPrep, to begin, enter the minutes available during the respective time, and then click the button below.</h2><p>
<button type="button" onclick="sendData()" id = "butt">Generate schedule!</button>
<table id="stuff">
</table>
<script>
	var day;
	var hour;
	var timeTable = [];
	tableText = '<tr><th>Time</th><th>Sunday</th><th>Monday</th><th>Tuesday</th><th>Wednesday</th><th>Thursday</th><th>Friday</th><th>Saturday</th></tr>';
	for(hour=0; hour<24; hour++){
		tableText += '<tr><td>';
		if(hour == 0 || hour == 12){
			tableText += '12:00 ';
		}else{
			tableText += (hour % 12).toString() + ':00 ';
		}
		if(hour > 11){
			tableText += 'PM';
		}else{
			tableText += 'AM';
		}
		tableText += '</td>';
		var tempArr = [];
		for(day=1; day<8; day++){
			tableText += '<td><input type="number" id="';
			tableText += day.toString() + ',' + hour.toString();
			tableText += '" value="0" class="textInputs"/></td>';
			tempArr.push(0);
		}
		timeTable.push(tempArr);
		tableText += '</tr>';
	}
	var myTable = document.getElementById('stuff');
	myTable.innerHTML = tableText;

	var foodPrepArray = [];
	for(hour=0; hour<24; hour++){
		var foodTempArray = [];
		for(day=0; day<7; day++){
			var inception = [];
			foodTempArray.push(inception);
		}
		foodPrepArray.push(foodTempArray);
	}
	
	function hoverFood(day, hour, index){
		dropDownBox = document.getElementById(day.toString() + ',' + hour.toString() + 'drop');
		tableText = '</button><div class="dropdown-content" id="' + day.toString() + ',' + hour.toString() + 'drop">';
		tableText += '<center><b>' + foodPrepArray[hour][day-1][index].name + '</b></center>';
		tableText += '<b>Preparation Time: ';
		tableText += foodPrepArray[hour][day-1][index].preptime.toString() + ' minutes</b>';
		if(foodPrepArray[hour][day-1][index].ingredients.length > 0){
			tableText += '<b>Ingredients:</b>';
		}
		var numIng;
		for(numIng = 0; numIng < foodPrepArray[hour][day-1][index].ingredients.length; numIng++){
			tableText += '<b>&emsp;&#8226;';
			tableText += foodPrepArray[hour][day-1][index].ingredients[numIng].name;
			tableText += '</b>';
		}
		if(foodPrepArray[hour][day-1][index].steps.length > 0){
			tableText += '<b>Steps:</b>';
		}
		var numStep;
		for(numStep = 0; numStep < foodPrepArray[hour][day-1][index].steps.length; numStep++){
			tableText += '<b>&emsp;' + (numStep + 1).toString() + ': ';
			tableText += foodPrepArray[hour][day-1][index].steps[numStep];
			tableText += '</b>';
		}
		
		tableText += '</div></div></td>';
		dropDownBox.innerHTML = tableText;
	}
	
	function sendData() {
		var buttonId = document.getElementById('butt');
		if(buttonId.innerHTML == 'Edit schedule!'){
			buttonId.innerHTML = 'Generate schedule!';
			var day;
			var hour;
			tableText = '<tr><th>Time</th><th>Sunday</th><th>Monday</th><th>Tuesday</th><th>Wednesday</th><th>Thursday</th><th>Friday</th><th>Saturday</th></tr>';
			for(hour=0; hour<24; hour++){
				tableText += '<tr><td>';
				if(hour == 0 || hour == 12){
					tableText += '12:00 ';
				}else{
					tableText += (hour % 12).toString() + ':00 ';
				}
				if(hour > 11){
					tableText += 'PM';
				}else{
					tableText += 'AM';
				}
				tableText += '</td>';
				for(day=1; day<8; day++){
					tableText += '<td><input type="number" id="';
					tableText += day.toString() + ',' + hour.toString();
					tableText += '" value="' + timeTable[hour][day-1].toString() + '" class="textInputs"/></td>';
				}
				tableText += '</tr>';
			}
			var myTable = document.getElementById('stuff');
			myTable.innerHTML = tableText;
			foodPrepArray = [];
			for(hour=0; hour<24; hour++){
				var foodTempArray = [];
				for(day=0; day<7; day++){
					var inception = [];
					foodTempArray.push(inception);
				}
				foodPrepArray.push(foodTempArray);
			}
		}else{
			buttonId.innerHTML = 'Edit schedule!';
			var rawData = [];
			var day;
			var hour;
			var cell;
			var array;
			for(hour = 0; hour < 24; hour++){
				for(day = 1; day < 8; day++){
					cell = document.getElementById(day.toString() + ',' + hour.toString());
					if(parseInt(cell.value) > 0){
						array = {
							start: hour,
							time: parseInt(cell.value),
							day: day-1
						};
						rawData.push(array);
					}
					timeTable[hour][day-1] = parseInt(cell.value);
				}
			}
			var data = JSON.stringify(rawData);
			var XHR = new XMLHttpRequest();

			// Define what happens on successful data submission
			XHR.addEventListener('load', function(event) {
				var obj = JSON.parse(XHR.responseText);
				var day;
				var hour;
				tableText = '<tr><th>Time</th><th>Sunday</th><th>Monday</th><th>Tuesday</th><th>Wednesday</th><th>Thursday</th><th>Friday</th><th>Saturday</th></tr>';
				for(hour=0; hour<24; hour++){
					tableText += '<tr><td>';
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
					tableText += '</td>';
					for(day=1; day<8; day++){
						tableText += '<td><div class="empty"></div></td>';
					}
					tableText += '</tr>';
				}
				var myTable = document.getElementById('stuff');
				myTable.innerHTML = tableText;
			
				for(day = 1; day < 8; day++){
					for(hour = 0; hour < obj[day-1].length; hour++){
						foodPrepArray[obj[day-1][hour].time][day-1].push(obj[day-1][hour]);
						if(foodPrepArray[obj[day-1][hour].time][day-1].length > 1){
							var optionSelect = document.getElementById(day.toString() + ',' + obj[day-1][hour].time.toString() + 'select');
							var opt = document.createElement("option");
							opt.text = obj[day-1][hour].name;
							opt.onmouseover = "hoverFood(day, hour, 0)";
							optionSelect.add(opt);
						}else{
							tableText = '<td><div class="dropdown"><button class="dropbtn">';
							//tableText += obj[day-1][hour].name;
							tableText += '<select onchange="hoverFood(' + day.toString()+ ', ' + obj[day-1][hour].time.toString() + ', this.selectedIndex);" style="width: 84px" id="' + day.toString() + ',' + obj[day-1][hour].time.toString() + 'select"><center><div class="truncated"><option>' + obj[day-1][hour].name + '</option></select>';
							tableText += '</button><div class="dropdown-content" id="' + day.toString() + ',' + hour.toString() + 'drop">';
							tableText += '<center><b>' + obj[day-1][hour].name + '</b></center>';
							tableText += '<b>Preparation Time: ';
							tableText += obj[day-1][hour].preptime.toString() + ' minutes</b>';
							if(obj[day-1][hour].ingredients.length > 0){
								tableText += '<b>Ingredients:</b>';
							}
							var numIng;
							for(numIng = 0; numIng < obj[day-1][hour].ingredients.length; numIng++){
								tableText += '<b>&emsp;&#8226;';
								tableText += obj[day-1][hour].ingredients[numIng].name;
								tableText += '</b>';
							}
							if(obj[day-1][hour].steps.length > 0){
							tableText += '<b>Steps:</b>';
							}
							var numStep;
							for(numStep = 0; numStep < obj[day-1][hour].steps.length; numStep++){
								tableText += '<b>&emsp;' + (numStep + 1).toString() + ': ';
								tableText += obj[day-1][hour].steps[numStep];
								tableText += '</b>';
							}
							
							tableText += '</div></div></td>';
							myTable.rows[obj[day-1][hour].time + 1].cells[day].innerHTML = tableText;
						}
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
			XHR.send(data);
		}
	}
</script>