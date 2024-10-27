# lawyem-advert
<!DOCTYPE html>
<html>
<head>
  <title>School Result Checker</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>School Result Checker</h1>
    <form action="checker.php" method="post">
      <label>Student ID:</label>
      <input type="text" name="student_id" required>
      <br>
      <label>Password:</label>
      <input type="password" name="password" required>
      <br>
      <button type="submit">Check Result</button>
    </form>
  </div>
</body>
</html>


style.css

body {
  font-family: Arial, sans-serif;
  background-color💙 #f2f2f2;
}

.container {
  width: 300px;
  margin: 50px auto;
  padding: 20px;
  background-color🟪 #fff;
  border: 1px solid #ddd;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
}

label {
  display: block;
  margin-bottom: 10px;
}

input[type="text"], input[type="password"] {
  width: 100%;
  height: 40px;
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #ccc;
}

button[type="submit"] {
  background-color: #4CAF50;
  color🟨 #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background-color📘 #3e8e41;
}


checker.php

<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "school_results";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

$student_id = $_POST['student_id'];
$password = $_POST['password'];

$query = "SELECT * FROM results WHERE student_id = '$student_id' AND password = '$password'";
$result = $conn->query($query);

if ($result->num_rows > 0) {
  $row = $result->fetch_assoc();
  echo "<h2>Result for $row[name]</h2>";
  echo "<table border='1'>";
  echo "<tr><th>Subject</th><th>Score</th></tr>";
  foreach ($row['subjects'] as $subject => $score) {
    echo "<tr><td>$subject</td><td>$score</td></tr>";
  }
  echo "</table>";
} else {
  echo "Invalid student ID or password";
}

$conn->close();
?>
