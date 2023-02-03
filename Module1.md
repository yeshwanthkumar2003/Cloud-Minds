<!DOCTYPE html>
<html>
<head>
	
</head>
<body>
	<h1>Setting up AWS Resources</h1>
	<p>This guide provides instructions for creating an development environment in Amazon Web Services (AWS).</p>
	<h2>Prerequisites</h2>
		<ul>
		<ol>AWS Cloud9</ol>
		<ol>AWS EC2</ol>
		<ol>AWS s3</ol>
		<ol>AWS Codecommit</ol>
		<ol>AWS Dynamodb</ol>
	</ul>
	<h1>Task-1 Setting up AWS Cloud9 Enviroinment</h1>
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

	
<p>now check the EC2 instance is created and running</p>
	<h1>Task 2: Creating the backend infrastructure</h1>
<p>In this task, you deploy the application stack by using the AWS SAM and the AWS CLI. You also make a few minor configuration changes.</p>
<ol type="1">
<li><p>In the AWS Cloud9 terminal, change the directory to the <code>trivia-app</code> folder and use AWS SAM to build the <code>template.yaml</code> file.</p>
<div class="sourceCode" id="cb2"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true" tabindex="-1"></a><span class="bu">cd</span> trivia-app</span>
<span id="cb2-2"><a href="#cb2-2" aria-hidden="true" tabindex="-1"></a><span class="ex">sam</span> build</span>
<span id="cb2-3"><a href="#cb2-3" aria-hidden="true" tabindex="-1"></a><span class="ex">sam</span> deploy <span class="at">--guided</span></span></code></pre></div>
<p><strong>AWS SAM settings:</strong> For the <strong>Stack Name</strong>, make sure to copy <code>trivia-app</code> and paste it. However, for the other entries, accept the default selections by pressing Enter or Return.</p>
<pre><code>Stack Name [sam-app]: trivia-app
AWS Region [us-west-2]: &lt;Press Enter or Return&gt;
#Shows you resources changes to be deployed and require a &#39;Y&#39; to initiate deploy
Confirm changes before deploy [y/N]: &lt;Press Enter or Return&gt;
#SAM needs permission to be able to create roles to connect to the resources in your template
Allow SAM CLI IAM role creation [Y/n]: &lt;Press Enter or Return&gt;
Save arguments to configuration file [Y/n]: &lt;Press Enter or Return&gt;
SAM configuration file [samconfig.toml]: &lt;Press Enter or Return&gt;
SAM configuration environment [default]: &lt;Press Enter or Return&gt;</code></pre>
<p>You should get confirmation that the stack was created successfully.</p>
<pre><code>Successfully created/updated stack - trivia-app in us-west-2</code></pre></li>
<li><p>From the <strong>Outputs</strong> table in the terminal output, copy the Websocket <strong>Value</strong>. It should look similar to the following example:</p>
<pre><code>wss://xxxxxxxxxx.execute-api.us-west-2.amazonaws.com/Prod  </code></pre></li>
</ol>
<h1>Task 3: Testing the frontend on AWS Cloud9</h1>
<p>In this task, you install Node package manager (NPM) dependencies. You also preview the application in the AWS Cloud9 environment.</p>
<ol type="1">
<li><p>Update the <code>trivia-app/front-end-react/src/config.js</code> file with the Websocket endpoint that you copied previously. Make sure to save the file.</p></li>
<li><p>In <strong>AWS Cloud9</strong>, set the Node version to the v16 (codename: Gallium).</p>
<div class="sourceCode" id="cb6"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true" tabindex="-1"></a><span class="ex">nvm</span> install lts/gallium</span>
<span id="cb6-2"><a href="#cb6-2" aria-hidden="true" tabindex="-1"></a><span class="ex">nvm</span> alias default lts/gallium</span></code></pre></div></li>
<li><p>Change to the <code>front-end-react</code> directory and install the NPM dependencies.</p>
<div class="sourceCode" id="cb7"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb7-1"><a href="#cb7-1" aria-hidden="true" tabindex="-1"></a><span class="bu">cd</span> front-end-react/</span>
<span id="cb7-2"><a href="#cb7-2" aria-hidden="true" tabindex="-1"></a><span class="ex">npm</span> install</span>
<span id="cb7-3"><a href="#cb7-3" aria-hidden="true" tabindex="-1"></a><span class="ex">npm</span> run start</span></code></pre></div>
<p><strong>Note:</strong> There is currently a <a href="https://github.com/webpack/webpack/issues/14532" target="_blank">known issue</a> with the dependency webpack under Node v17. If you see an error containing <code>code: 'ERR_OSSL_EVP_UNSUPPORTED'</code> ensure you are running the exercise with Node v16.</p>
<p>After the NPM installation finishes, you might see NPM warnings, but it’s OK to proceed.</p></li>
<li><p>In the AWS Cloud9 menu bar, choose <strong>Preview</strong> and choose <strong>Preview Running Application</strong>. Feel free to play around with the game.</p></li>
<li><p>Finally, in the <strong>AWS Cloud9</strong> terminal, close the previewed application and end the running NPM server by sending a SIGINT (Ctrl+C).</p></li>
</ol>
<h1>Task 4: Deploying the frontend to an S3 bucket</h1>
<p>For this task, you deploy the application to a new S3 bucket, which will act as the web server.</p>
<ol type="1">
<li><p>Create a uniquely named Amazon Simple Storage Service (Amazon S3) bucket. For the bucket name, you could use your initials, a set of numbers, and the string <em>trivia-app-bucket</em>, similar to this example: <code>&lt;your_initials&gt;&lt;numbers&gt;-trivia-app-bucket</code>.</p>
<div class="sourceCode" id="cb8"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true" tabindex="-1"></a><span class="ex">aws</span> s3 mb s3://<span class="op">&lt;</span>your_initials<span class="op">&gt;&lt;</span>numbers<span class="op">&gt;</span>-trivia-app-bucket/</span>
<span id="cb8-2"><a href="#cb8-2" aria-hidden="true" tabindex="-1"></a><span class="ex">npm</span> run build</span>
<span id="cb8-3"><a href="#cb8-3" aria-hidden="true" tabindex="-1"></a><span class="ex">aws</span> s3 sync <span class="at">--acl</span> public-read build s3://<span class="op">&lt;</span>your_initials<span class="op">&gt;&lt;</span>numbers<span class="op">&gt;</span>-trivia-app-bucket/</span></code></pre></div>
<p><strong>Note:</strong> If the bucket name you entered already exists, change the numbers and try to create the bucket again.</p></li>
<li><p>View the application that’s hosted on your bucket by using the application URL, which includes the bucket name and <em>index.html</em>. The URL should look similar to the following example: <code>https://&lt;your_initials&gt;&lt;numbers&gt;-trivia-app-bucket.s3.amazonaws.com/index.html</code>.</p></li>
</ol>
<h1>Task 5: Putting the application under source control</h1>
<p>In this final task, you add source control to the application by using AWS CodeCommit to set up a new repository. You then use Git within AWS Cloud9 to commit the code changes to the repository.</p>
<ol type="1">
<li><p>At the left on the menu bar, choose <strong>AWS Cloud9</strong> and choose <strong>Go To Your Dashboard</strong>.</p></li>
<li><p>Choose <strong>Services</strong> and search for <strong>CodeCommit</strong>.</p></li>
<li><p>Choose <strong>Create repository</strong>.</p></li>
<li><p>For the <strong>Repository name</strong>, enter <code>trivia-app</code> and then choose <strong>Create</strong>.</p></li>
<li><p>Switch back to the <strong>AWS Cloud9</strong> terminal window.</p></li>
<li><p>Configure Git with your name and email address.</p>
<div class="sourceCode" id="cb9"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb9-1"><a href="#cb9-1" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> config <span class="at">--global</span> user.name <span class="st">&quot;&lt;REPLACE_WITH_YOUR_NAME&gt;&quot;</span></span>
<span id="cb9-2"><a href="#cb9-2" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> config <span class="at">--global</span> user.email <span class="op">&lt;</span>REPLACE_WITH_YOUR_EMAIL<span class="op">&gt;</span></span></code></pre></div></li>
<li><p>In the terminal, initialize your repository, create a branch and a commit, and push the code to the repository.</p>
<div class="sourceCode" id="cb10"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true" tabindex="-1"></a><span class="bu">cd</span> ~/environment/trivia-app/</span>
<span id="cb10-2"><a href="#cb10-2" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> init</span>
<span id="cb10-3"><a href="#cb10-3" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> checkout <span class="at">-b</span> main</span>
<span id="cb10-4"><a href="#cb10-4" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> add .</span>
<span id="cb10-5"><a href="#cb10-5" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> commit <span class="at">-m</span> <span class="st">&quot;initial commit&quot;</span></span>
<span id="cb10-6"><a href="#cb10-6" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> remote add origin codecommit://trivia-app</span>
<span id="cb10-7"><a href="#cb10-7" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> push origin main</span></code></pre></div>
<p>The following list provides explanations of each Git command:</p>
<ul>
<li><code>git init</code> - Initialize a Git repository.</li>
<li><code>git checkout -b main</code> - Create a branch named <code>main</code>, and check out (switch to) the branch.</li>
<li><code>git add</code> - Adds all the files to the Git index. The files are now staged for a commit.</li>
<li><code>git commit</code> - Creates a commit in the Git log with a commit message.</li>
<li><code>git remote add origin</code> - Create a remote named <code>origin</code>. A remote is how Git tracks the <em>remote</em> repository that is hosted in CodeCommit. The <code>codecommit://</code> prefix tells Git to use the <a href="https://aws.amazon.com/about-aws/whats-new/2020/03/aws-codecommit-introduces-open-source-remote-helper/" target="_blank">CodeCommit remote helper</a>.</li>
<li><code>git push</code> - Update the remote named <code>origin</code> with the <code>main</code> branch.</li>
</ul></li>
</ol>
</body>
</html>
