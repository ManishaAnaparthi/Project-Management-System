<?php
		session_start();
		
		$username = "";
		$errors = array();
		
		// connect to the database
		$db = mysqli_connect('localhost:3307', 'root', '','project1');
 
		// signup button is clicked
		if (isset($_POST['submit'])) {
			$username = mysqli_real_escape_string($db, $_POST['username']);
			$email = mysqli_real_escape_string($db, $_POST['email']);
			$password = mysqli_real_escape_string($db, $_POST['password']);
		          //ensure that form fields filled properly
			if (empty($username)){
				array_push($errors, "UserName is required");
				
			}
			
			if (empty($email)){
				array_push($errors, "email is required");
				
			}
 
			if (empty($password)){
				array_push($errors, "Password is required");
				
			}
			
			
			//if there are no errors, save user to database
			if(count($errors) == 0){
				
				$sql = "INSERT INTO users (username, email, password) 
  			  VALUES('$username', '$email', '$password')";
  	
				mysqli_query($db, $sql);				
				$_SESSION['username'] = $username;
				$_SESSION['success'] = "You are now logged in";
				header('location: HOME.html');
				
				
			}
			
			
		}
		
		//logout
		if (isset($_GET['logout'])) {
			session_destroy();
			unset($_SESSION['username']);
			header('location: index.html');
		}
		
		// LOGIN USER
		if (isset($_POST['login_user'])) {
		  $username = mysqli_real_escape_string($db, $_POST['username']);
		  $password = mysqli_real_escape_string($db, $_POST['password']);
 
		  if (empty($username)) {
			array_push($errors, "Username is required");
		  }
		  if (empty($password)) {
			array_push($errors, "Password is required");
		  }
 
		  if (count($errors) == 0) {
			
			$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
			$results = mysqli_query($db, $query);
			if (mysqli_num_rows($results) == 1) {
			  $_SESSION['username'] = $username;
			  $_SESSION['success'] = "You are now logged in";
			  header('location: HOME.html');
			}else {
				array_push($errors, "Wrong username/password combination");
			}
		  }
		}	
