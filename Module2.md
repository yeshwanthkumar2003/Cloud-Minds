<h1 id="exercise-testing-the-application">Module-2: Testing the Application</h1>
<p>In this exercise, you will return to the AWS Cloud9 instance and run the local build for the project. The local build contains linting (or static analysis) of the code and unit tests. Next, you will run an integration test. The integration test finds the API Gateway Websocket endpoint, and it simulates a player completing a game. You will then add a simple feature to the code. Finally, you will fix the unit tests and commit the changes to your repository.</p>
<h2 id="task-1-running-unit-tests">Task 1: Running unit tests</h2>
<p>In this task, you run unit tests against the application code. You then view the reports that are outputted from these tests.</p>
<ol type="1">
<li><p>In your <strong>AWS Cloud9</strong> terminal, make sure you are still in the <code>trivia-app</code> folder. The prompt should also show the top-level Git branch.</p>
<pre><code>cd ~/environment/trivia-app</code></pre></li>
<li><p>Install the backend code requirements and the unit test requirements by using <code>pip3</code> to run the following commands.</p>
<div class="sourceCode" id="cb2"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true" tabindex="-1"></a><span class="ex">pip3</span> install <span class="at">-U</span> <span class="at">-r</span> back-end-python/gameactions/requirements.txt</span>
<span id="cb2-2"><a href="#cb2-2" aria-hidden="true" tabindex="-1"></a><span class="ex">pip3</span> install <span class="at">-U</span> <span class="at">-r</span> back-end-python/tests/requirements.txt</span>
<span id="cb2-3"><a href="#cb2-3" aria-hidden="true" tabindex="-1"></a><span class="ex">./local_build.sh</span></span></code></pre></div>
<p>You should see the code rating after <code>pylint</code> does its static analysis:</p>
<pre><code>------------------------------------
Your code has been rated at 10.00/10</code></pre>
<p>Then, <code>pytest</code> runs its unit tests, which are shown like the following example:</p>
<pre><code>back-end-python/tests/unit/test_handler.py .........</code></pre>
<p>It will also output a coverage report in HTML, which is in <code>/trivia-app/htmlcov</code>.</p></li>
<li><p>In the <code>htmlcov</code> folder, right-click the <code>index.html</code> file and view the coverage report by choosing <strong>Preview</strong>.</p></li>
</ol>
<h2 id="task-2-running-the-integration-test">Task 2: Running the integration test</h2>
<p>For this task, you run an integration test against the application code.</p>
<ol type="1">
<li><p>Start the integration test (which will simulate a game) by running the following command.</p>
<pre><code>AWS_SAM_STACK_NAME=trivia-app pytest -s back-end-python/tests/integration/test_api_gateway.py</code></pre>
<p>The integration test uses the AWS SAM stack to find the Websocket endpoint, so the stack is passed in the <code>AWS_SAM_STACK_NAME</code> environment variable.</p>
<p>At the end of the test, you should see the following message: <strong><em>1 passed</em></strong></p></li>
</ol>
<h2 id="task-3-updating-the-code-and-unit-tests">Task 3: Updating the code and unit tests</h2>
<p>In this task, you update the application code to change the score value from <em>10</em> to <em>20</em>. You also update the unit test code.</p>
<ol type="1">
<li><p>Open the <code>trivia-app/back-end-python/gameactions/app.py</code> file.</p></li>
<li><p>Scroll down to the <code>trivia_calculate_scores</code> function.</p></li>
<li><p>Update the code so the game increments the score by <code>20</code> instead of by <code>10</code>. Locate <code>score += 10</code> and change it to <code>score += 20</code>. Make sure to save the file.</p>
<pre><code>score += 20</code></pre></li>
<li><p>Verify that you left the code in a stable state by running the <code>./local_build.sh</code> command.</p>
<p>At the top of the failure report, you should see that the <code>test_trivia_calculate_scores_correct</code> test failed:</p>
<pre><code>=============================================== FAILURES ============================================================================================
_________________________________ test_trivia_calculate_scores_correct ______________________________________________________________________________</code></pre>
<p>You also need to update the unit test to match the new score.</p></li>
<li><p>Open the <code>trivia-app/back-end-python/tests/unit/test_handler.py</code> file.</p></li>
<li><p>Find the <code>test_trivia_calculate_scores_correct</code> function.</p></li>
<li><p>In the function’s <code>TABLE.update_item.assert_called_with</code> call, find the <code>AttributeUpdates</code> field and update the <code>Value</code> from <code>10</code> to <code>20</code>.</p>
<pre><code>AttributeUpdates={&#39;score&#39;: {&#39;Value&#39;: 20, &#39;Action&#39;: &#39;PUT&#39;}}</code></pre></li>
<li><p>You must also update <code>app.MANAGEMENT.post_to_connection.assert_has_calls</code> (which tests API Gateway Websocket calls). Change the value of <code>score</code> from <code>10</code> to <code>20</code>.</p>
<pre><code>mock.call(Data=&#39;{&quot;action&quot;: &quot;playerlist&quot;, &quot;players&quot;: [{&quot;connectionId&quot;: &quot;connection-1&quot;, &quot;playerName&quot;: &quot;AliceBlue&quot;, &quot;score&quot;: 20, &quot;currentPlayer&quot;: true}]}&#39;, ConnectionId=&#39;connection-1&#39;),</code></pre></li>
<li><p>Save the file and re-run <code>./local_build.sh</code>. The test summary should again show that everything passed.</p></li>
</ol>
<h2 id="task-4-committing-to-source-control-through-the-aws-cloud9-gui-or-the-aws-cli">Task 4: Committing to source control through the AWS Cloud9 GUI or the AWS CLI</h2>
<p>In this task, you commit the changes from the previous tasks to your code repository. <em>You can commit the code by using either the <strong>AWS Cloud9 GUI</strong>, or AWS CLI commands in the <strong>AWS Cloud9 terminal</strong></em>.</p>
<ol type="1">
<li><p>In the menu, at the left of <strong>AWS Cloud9</strong>, choose the source control icon:</p>
<li><p>Under <strong>Changes</strong>, select either the <code>app.py</code> or <code>test_handler.py</code> file to view the diffs between the original files and your changes.</p></li>
<li><p>Under <strong>Changes</strong>, stage the changes in <code>app.py</code> and <code>test_handler.py</code> for a commit by placing the pointer over the file name and choosing the stage icon (<strong>+</strong>) for the file. The files should now appear under <strong>Staged Changes</strong>.</p></li>
<li><p>In the <strong>Source Control</strong> pane, at the right side of the <strong>trivia-app</strong> heading, left-click paper scroll icon and choose <strong>Commit</strong>. Choose <strong>Yes</strong>.</p>
<li><p>Under the <strong>trivia-app</strong> heading, enter the following commit message and press Enter: <code>Scores now increment by 20</code></p>
<li><p>In the <strong>trivia-app</strong> heading, left-click the paper scroll icon and choose <strong>Push to…</strong>. When prompted to select a remote, choose the remote named <strong>origin</strong>.</p></li>
</ol>
<details>
<summary>
Commit changes through the AWS CLI
</summary>
<h2 id="task-4-committing-to-source-control-through-the-aws-cli">Task 4: Committing to source control through the AWS CLI</h2>
<ol type="1">
<li><p>In <strong>AWS Cloud9</strong>, you can also use the AWS CLI to commit the changes to AWS CodeCommit by running the following command:</p>
<div class="sourceCode" id="cb10"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> add <span class="pp">*</span></span>
<span id="cb10-2"><a href="#cb10-2" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> commit <span class="at">-m</span> <span class="st">&quot;Scores now increment by 20&quot;</span></span>
<span id="cb10-3"><a href="#cb10-3" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> push origin main</span></code></pre></div></li>
</ol>
