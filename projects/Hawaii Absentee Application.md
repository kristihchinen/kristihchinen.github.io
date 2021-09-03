---
layout: project
type: project
image: images/Ballot.jpeg
title: Hawaii Absentee Application
permalink: projects/ballot
date: 2020-04-16
labels:
  - html
  - PHP
summary: In ICS 215 we were tasked to create an absentee ballot application that would be able to run on Google Chrome.
---


In the spring of 2020 when I took ICS 215 we needed to create an absentee ballot application using PHP which we would then need to convert it to a html file to run on Google Chrome. This application would ask you the basic questions, like, name, birthday, gender, etc. Then, once the user submits enter the previous answers would be cleared and ready for the next user.

Source code:

<!-- Object: To make the form that a voter is filling out sticky and will produce and error message when something is not filled out -->
<!doctype html>
<html>
<head>
<title>Hawaii Absentee Application</title>
<!--                   PHP Assignment 01                    -->
<!--   Modified version of HI Absentee Ballot Application   -->
<!--                 Created by: Ed Meyer                   -->
<style>
  .error {
    color: #A00;
    font-weight: bold;
  }
</style>
<script type="text/javascript">

  /**
   * Determines how many days there should be in the month, when the user changes the month.
   * Calls populateDays() with the amount of days.
   *
   * @param monthValue An integer representing the month. Janurary is 1.
   */
  function adjustDays(monthValue) {
    // January, March, May, July, August, October and December have 31 days.
    if (monthValue == 1 || monthValue == 3 || monthValue == 5 || monthValue == 7 ||
        monthValue == 8 || monthValue == 10 || monthValue == 12) {
      populateDays(31);
    }
    // February will have 29 days.
    else if (monthValue == 2) {
      populateDays(29);
    }
    // The rest of the months have 30 days.
    else {
      populateDays(30);
    }
  }

  /**
   * Creates and adds <option> for each day in the month.
   * Clears the current list and re-populates at every change.
   *
   * @numDays An integer representing how many days.
   */
  function populateDays(numDays) {
    // Get the day <select> menu.
    let daysNode = document.getElementById("day");
    // Clears all the days.
    daysNode.innerHTML = "";
    // Populate the days <select> menu for how much there should be.
    for (let i = 1; i <= numDays; i++) {
      // Temporary day node to be added to the day <select> menu.
      let tempDayNode = document.createElement("OPTION");
      tempDayNode.innerHTML = i;
      daysNode.add(tempDayNode);
    }
  }

  function copyAddress(checkedParam) {
    if (checkedParam) {
      document.absentapp.forwardName.value = document.absentapp.name.value;
      document.absentapp.forwardAddress.value = document.absentapp.resAdd.value + "\n";
      document.absentapp.forwardAddress.value += document.absentapp.citytown1.value + " HI, " + document.absentapp.zip1.value;
    }
  }

</script>
</head>
<body>
<form name="absentapp" method="post">
<strong>Section I.</strong> I hereby request Absentee Ballots for the following Election(s):<br>
<!-- Used to make the buttons sticky when the user enters an input  -->
<!-- For all the the buttons, Primary, General, Primary and General, and Specail -->
<input type="radio" name="BallotType" value="PrimaryOnly"
<?php
  if (isset($_GET['BallotType'])) echo "checked";
?>
>Primary Only
<input type="radio" name="BallotType" value="GeneralOnly"
<?php
  if (isset($_GET['BallotType'])) echo "checked";
?>
>General Only
<input type="radio" name="BallotType" value="PandG"
<?php
  if (isset($_GET['BallotType'])) echo "checked";
?>
>Primary &amp; General
<input type="radio" name="BallotType" value="Special"
<?php
  if (isset($_GET['BallotType'])) echo "checked";
?>
>Special Elections


<br>
<br>
<strong>Section II. </strong>Failure to complete all items will prevent acceptance of this application.<br>

<label for="ssn">1. Social Security Number*:</label> <input name="ssn" id="ssn" type="text" value=" <?php
//making sure that the SSN section won't have the information disappear even after the user hits submit
if (isset($_GET['ssn'])) echo $_GET['ssn'];
?>>

<?php
//Ensuring that whenever the user enters a SSN that it will meet the requirements and there will be no additional information
if (!preg_match("/^\d{3}-\d{2}-\d{4}$/")) {
  echo '<span class="error"> Not a valid Social Security Number.</span>' ;
}
?>
<?php if (isset($_GET['submit']) && strlen($_GET['ssn']) == 0) echo '<span class="error">Please enter a
Social Security Number.</span>'; ?>" <br>


<br>
2. Date of Birth:
<select id="month" onchange="adjustDays(this.value)">
  <script type="text/javascript">
    let months = ["January", "February","March", "April", "May", "June", "July", "August",
                  "September", "October", "November", "December"];
    for (let i = 1; i <= 12; i++) {
      document.write("<option value=\"" + i + "\">" + months[i-1] + "</option>");
    }
  </script>
</select>
<select id="day">
  <script type="text/javascript">
    for (let i = 1; i <= 31; i++) {
      document.write("<option>" + i + "</option>");
    }
  </script>
</select>
<select id="year">
  <script type="text/javascript">
    let currentYear = new Date().getFullYear();
    for (let i = currentYear - 17; i >= (currentYear - 122); i--) {
      document.write("<option>" + i + "</option>");
    }
  </script>
</select>
<br>

3. Gender:
<input type="radio" name="gender" value="male"
<?php
  if (isset($_GET['gender'])) echo "checked";
?>> Male
<input type="radio" name="gender" value="female"
<?php
  if (isset($_GET['gender'])) echo "checked";
?>> Female
<br>

<label for="hometele">4. Telephone Number - Home:</label> <input name="hometele" id="hometele" type="text" value="">
<br>

<label for="name">5. Name: </label>
<input name="name" id="name" type="text" value=""><br>

6. Residence Address In Hawaii Street:
<input name="resAdd" id="resAdd" type="text" value=""> <br>
City/Town: <input name="citytown1" id="citytown1" type="text" value=""> Zip Code <input name="zip1" id="zip1" type="text" value=""><br>
<br>
<strong>Section III.</strong> Please mail my ballots to: <br>

Use same as Residence Address: <input name="copyResAdd" id="copyResAdd" type="checkbox" onclick="copyAddress(this.checked)"> <br>

7. Name: <input name="forwardName" id="forwardName" type="text"> <br>

8. Forwarding Address: <textarea rows="3" name="forwardAddress" id="forwardAddress"></textarea>
<br>
<br>

<label for="affirm"><strong>Section IV.</strong> I hereby affirm that: 1) I am the person named above; 2) I am requesting an absentee ballot for myself and no other; and 3) all information furnished on this application is true and correct. </label>

<input name="affirm" id="affirm" type="checkbox"> <br>

<br>
<input type="submit" name="submit" value="Submit">
</form>
*Notice: A Social Security Number is required by HRS &sect;11-15 and HRS &sect;15-4. It is used to prevent fraudulent registration and voting. Failure to furnish this information will prevent acceptance of this application. Pursuant to HRS &sect;11-20, the City/County Clerks may use this application to transfer a voter to the proper precinct to correspond with the address given above, under item 6.
</body>
</html>
