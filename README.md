# Voice-User-Interface
### Bulid a user interface to convert voice to text (speech to text)


![لقطة شاشة 2024-07-20 084156](https://github.com/user-attachments/assets/9f6c8900-18d0-48ed-be8f-9f90257a1419)
### The code 
ex Git :
```

<!DOCTYPE html>
<html>
<head>
  <title>Speech to Text</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
      background-color: #f8d7da;
    }
    #robot__control {
      width: 80%;
      height: 300px;
      font-size: 16px;
      padding: 10px;
      display: block;
      margin: 20px auto;
      background-color: white;
      border: 1px solid #ddd;
      border-radius: 10px;
    }
    button {
      font-size: 16px;
      padding: 10px 20px;
      background-color: #dc3545;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Speech to Text</h1>
  <textarea id="robot__control" placeholder="Converted text will appear here..."></textarea>
  <button onclick="convertSpeechToText()">Convert Speech to Text</button>

  <script>
    function convertSpeechToText() {
      // Check if the browser supports the Web Speech API
      if ('webkitSpeechRecognition' in window) {
        var recognition = new webkitSpeechRecognition();
        recognition.lang = 'ar-SA'; // Set the language to Arabic
        recognition.onresult = function(event) {
          var text = event.results[0][0].transcript;
          document.getElementById('robot__control').value = text;
          
          // Save the text to a database
          saveTextToDatabase(text);
        };
        recognition.start();
      } else {
        alert('Speech recognition is not supported in this browser.');
      }
    }

    function saveTextToDatabase(text) {
      var xhr = new XMLHttpRequest();
      xhr.open("POST", "save.php", true);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
          alert("Text saved to database.");
        }
      };
      xhr.send("text=" + encodeURIComponent(text));
    }
  </script>
</body>
</html>



```

### Save the output to database 

![لقطة شاشة 2024-07-20 084545](https://github.com/user-attachments/assets/8c104d1d-097b-4188-856f-388e7b2bd458)

#### The Code save with php :
ex Git :
```
<?php
$servername = "localhost";
$username = "root";
$password = ""; // Add your database password if you have one
$dbname = "robot__control";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $text = $_POST['text'];
    $sql = "INSERT INTO saund_movements (text) VALUES ('$text')";
    
    if ($conn->query($sql) === TRUE) {
        echo "Text saved successfully👏🏻.";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
    } else {
    echo "No 'direction' value submitted in the POST request.";
    }
  
  $conn->close();
  
?>











```
