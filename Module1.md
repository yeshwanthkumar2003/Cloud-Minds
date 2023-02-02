<!DOCTYPE html>
<html>
<head>
	
</head>
<body>
	<h1>Creating an AWS Cloud9 Environment</h1>
	<p>This guide provides instructions for creating an AWS Cloud9 development environment in Amazon Web Services (AWS).</p>
	<h2>Prerequisites</h2>
	<ol>AWS Cloud9</ol>
		<ul>AWS EC2</ul>
		<ul>AWS s3</ul>
		<ul>AWS Codecommit</ul>
		<ul>AWS Dnamodb</ul>
		<ul>Pytest</ul>
	<ul>
	</ul>
	<h1>Part-1 Setting up AWS Cloud9 Enviroinment</h1>
	<h2>Step 1: Log in to the AWS Management Console</h2>
	<p>Log in to the <b>AWS Management Console</b> using your AWS account credentials. If you don't have an AWS account, you can sign up for one on the <a href="https://aws.amazon.com/">AWS website</a>.</p>
	<h2>Step 2: Create a new Cloud9 Environment</h2>
	<p>Once you have logged in, go to the <b>Cloud9 console</b> by clicking on the Services menu and selecting Cloud9 under the Developer Tools category. Then click on the <b>Create environment</b> button. </p>
	<h2>Step 3: Configure the Environment</h2>
	<p>On the <b>Create environment</b> page, configure your environment as desired. You can choose the <b>name</b> and <b>description</b> for your environment, as well as the <b>instance type</b>, and environment settings. Make sure to choose the EC2 instance type that meets your requirements. Once you have made your selections, click on the <b>Create environment</b> button to proceed.</p>
	<h2>Step 4: Wait for the Environment to be created</h2>
	<p>It may take a few minutes for the environment to be created. You will be redirected to the <b>Cloud9 IDE</b> once the environment is ready. </p>
	<h2>Step 5: Download the Application by paste the Commands on Cloud9 terminal</h2>
	<p>Your Cloud9 environment is now ready to use. You can start coding, running commands, and deploying applications right from the <b>Cloud9 IDE</b>. Enjoy!</p><h3>Open the <b>AWS Cloud9 terminal</b> by running the following command</h3>
	<br>
	
	  wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/DEV-AWS-MO-DevOps-C1/downloads/trivia-app.zip -O ~/trivia-app.zip
	  unzip -o ~/trivia-app.zip

	<p>now check the EC2 instance is created and running</h1>
</body>
</html>
