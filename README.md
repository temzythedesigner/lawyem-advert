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
1. Validation and Error Handling

checker.php

// Validate input
if (empty($_POST['student_id']) || empty($_POST['password'])) {
  echo "Please fill in both fields";
  exit;
}

// Hash password
$password = hash('sha256', $_POST['password']);

// Check if student ID exists
$query = "SELECT * FROM students WHERE student_id = '$student_id'";
$result = $conn->query($query);
if ($result->num_rows == 0) {
  echo "Student ID not found";
  exit;
}


2. Password Recovery

recover.php

<?php
// Send recovery email
if (isset($_POST['recover'])) {
  $student_id = $_POST['student_id'];
  $query = "SELECT email FROM students WHERE student_id = '$student_id'";
  $result = $conn->query($query);
  if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    $email = $row['email'];
    $password = generateRandomPassword();
    $query = "UPDATE students SET password = '$password' WHERE student_id = '$student_id'";
    $conn->query($query);
    sendEmail($email, "Password Recovery", "Your new password is: $password");
    echo "Password sent to your email";
  } else {
    echo "Student ID not found";
  }
}

function generateRandomPassword() {
  // Generate random password
}

function sendEmail($to, $subject, $message) {
  // Send email using mail() or SMTP
}


3. Result Sorting and Filtering

checker.php

// Sort results by subject or score
if (isset($_GET['sort'])) {
  $sort = $_GET['sort'];
  if ($sort == 'subject') {
    $query = "SELECT * FROM results WHERE student_id = '$student_id' ORDER BY subject";
  } elseif ($sort == 'score') {
    $query = "SELECT * FROM results WHERE student_id = '$student_id' ORDER BY score DESC";
  }
} else {
  $query = "SELECT * FROM results WHERE student_id = '$student_id'";
}

// Filter results by subject or score range
if (isset($_POST['filter'])) {
  $subject = $_POST['subject'];
  $min_score = $_POST['min_score'];
  $max_score = $_POST['max_score'];
  $query = "SELECT * FROM results WHERE student_id = '$student_id' AND subject = '$subject' AND score BETWEEN '$min_score' AND '$max_score'";
}


4. Student Profile

profile.php

// Display student profile information
$query = "SELECT * FROM students WHERE student_id = '$student_id'";
$result = $conn->query($query);
if ($result->num_rows > 0) {
  $row = $result->fetch_assoc();
  echo "<h2>Profile</h2>";
  echo "<p>Name: $row[name]</p>";
  echo "<p>Email: $row[email]</p>";
  echo "<p>Phone: $row[phone]</p>";
}


5. Admin Dashboard

admin.php

// Display admin dashboard
$query = "SELECT * FROM students";
$result = $conn->query($query);
if ($result->num_rows > 0) {
  echo "<h2>Students</h2>";
  echo "<table border='1'>";
  echo "<tr><th>Student ID</th><th>Name</th><th>Email</th></tr>";
  while ($row = $result->fetch_assoc()) {
    echo "<tr><td>$row[student_id]</td><td>$row[name]</td><td>$row[email]</td></tr>";
  }
  echo "</table>";
}
