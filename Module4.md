<h1 id="exercise-creating-a-pipeline">Exercise: Creating a Pipeline</h1>
<p>In this exercise, you will first start by using AWS CodePipeline to create a new pipeline. You will create a new feature branch in your repository and edit some application code. You will then commit the changes to your feature branch and merge the feature branch into your main branch.</p>
<h2 id="task-1-creating-a-pipeline">Task 1: Creating a pipeline</h2>
<p>In this task, you use AWS CodePipeline to create a pipeline.</p>
<ol type="1">
<li><p>In the console, choose <strong>Services</strong>, and search for and select <strong>CodePipeline</strong>.</p></li>
<li><p>Choose <strong>Create pipeline</strong>.</p></li>
<li><p>For <strong>Pipeline name</strong>, enter <code>trivia-pipeline</code> and choose <strong>Next</strong>.</p></li>
<li><p>In the <strong>Add source stage</strong> step, configure the following settings:</p>
<ul>
<li><strong>Source provider</strong>: <em>AWS CodeCommit</em></li>
<li><strong>Repository name</strong>: <em>trivia-app</em></li>
<li><strong>Branch name</strong>: <em>main</em></li>
</ul></li>
<li><p>Choose <strong>Next</strong>.</p></li>
<li><p>In the <strong>Add build stage</strong> step, configure the following settings.</p>
<ul>
<li><strong>Build provider</strong>: <em>AWS CodeBuild</em></li>
<li><strong>Project name</strong>: <em>trivia-unittests</em></li>
</ul></li>
<li><p>Choose <strong>Next</strong>.</p></li>
<li><p>In the <strong>Add deploy stage</strong> step, choose <strong>Skip deploy stage</strong> and then choose <strong>Skip</strong>.</p></li>
<li><p>In the <strong>Review</strong> step, choose <strong>Create pipeline</strong>. You should see a <em>Success</em> message.</p></li>
</ol>
<h2 id="task-2-creating-a-feature-branch">Task 2: Creating a feature branch</h2>
<p>In this task, you create a new feature branch in your AWS CodeCommit repository.</p>
<ol type="1">
<li><p>In a separate browser tab, switch to the <strong>AWS Cloud9</strong> development environment. Make sure that you are in the <code>trivia-app</code> directory.</p>
<div class="sourceCode" id="cb1"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb1-1"><a href="#cb1-1" aria-hidden="true" tabindex="-1"></a><span class="bu">cd</span> ~/environment/trivia-app</span></code></pre></div>
<p>Create a feature branch by running the following command:</p>
<div class="sourceCode" id="cb2"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> checkout <span class="at">-b</span> feature-bonus-scores</span></code></pre></div>
<p>You should see that you created a new branch and switched to that branch:</p>
<pre><code>Switched to a new branch &#39;feature-bonus-scores&#39;</code></pre></li>
</ol>
<h2 id="task-3-editing-the-code">Task 3: Editing the code</h2>
<p>In this task, you edit the code to implement a bonus scores feature.</p>
<ol type="1">
<li><p>Open the <code>trivia-app/back-end-python/gameactions/app.py</code> file.</p></li>
<li><p>In the <code>trivia_calculate_scores</code> function, locate the code where <code>last_answer</code> is set.</p></li>
<li><p>On the next line, add code to set the <code>bonus</code> variable:</p>
<div class="sourceCode" id="cb4"><pre class="sourceCode python"><code class="sourceCode python"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true" tabindex="-1"></a>last_answer <span class="op">=</span> connection[<span class="st">&quot;lastAnswer&quot;</span>] <span class="cf">if</span> <span class="st">&quot;lastAnswer&quot;</span> <span class="kw">in</span> connection <span class="cf">else</span> <span class="st">&quot;&quot;</span></span>
<span id="cb4-2"><a href="#cb4-2" aria-hidden="true" tabindex="-1"></a>bonus <span class="op">=</span> question[<span class="st">&quot;bonus&quot;</span>] <span class="cf">if</span> <span class="st">&quot;bonus&quot;</span> <span class="kw">in</span> question <span class="cf">else</span> <span class="dv">0</span></span></code></pre></div></li>
<li><p>You also need to add the <code>bonus</code> variable to the calculation logic. Update the code incrementing <code>score</code> so that it includes <code>bonus</code>.</p>
<p>Code before:</p>
<div class="sourceCode" id="cb5"><pre class="sourceCode python"><code class="sourceCode python"><span id="cb5-1"><a href="#cb5-1" aria-hidden="true" tabindex="-1"></a><span class="cf">if</span> last_question_id <span class="op">==</span> question[<span class="st">&quot;id&quot;</span>] <span class="kw">and</span> last_answer <span class="op">==</span> question[<span class="st">&quot;answer&quot;</span>]:</span>
<span id="cb5-2"><a href="#cb5-2" aria-hidden="true" tabindex="-1"></a>    score <span class="op">+=</span> <span class="dv">20</span></span></code></pre></div>
<p>Code after:</p>
<div class="sourceCode" id="cb6"><pre class="sourceCode python"><code class="sourceCode python"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true" tabindex="-1"></a><span class="cf">if</span> last_question_id <span class="op">==</span> question[<span class="st">&quot;id&quot;</span>] <span class="kw">and</span> last_answer <span class="op">==</span> question[<span class="st">&quot;answer&quot;</span>]:</span>
<span id="cb6-2"><a href="#cb6-2" aria-hidden="true" tabindex="-1"></a>        score <span class="op">+=</span> <span class="dv">20</span> <span class="op">+</span> bonus</span></code></pre></div></li>
<li><p>Save the file.</p></li>
<li><p>You also need to update the unit tests code so that it tests the new bonus score. Open the <code>trivia-app/back-end-python/tests/unit/test_handler.py</code> file.</p></li>
<li><p>Replace <code>SCORES_EVENT</code> with a new event that includes a bonus score:</p>
<div class="sourceCode" id="cb7"><pre class="sourceCode python"><code class="sourceCode python"><span id="cb7-1"><a href="#cb7-1" aria-hidden="true" tabindex="-1"></a>SCORES_EVENT <span class="op">=</span> {</span>
<span id="cb7-2"><a href="#cb7-2" aria-hidden="true" tabindex="-1"></a><span class="st">&quot;gameid&quot;</span> : <span class="st">&quot;01234567012301230123012345678901&quot;</span>,</span>
<span id="cb7-3"><a href="#cb7-3" aria-hidden="true" tabindex="-1"></a><span class="st">&quot;questions&quot;</span> : [</span>
<span id="cb7-4"><a href="#cb7-4" aria-hidden="true" tabindex="-1"></a>    { <span class="st">&quot;id&quot;</span> : <span class="st">&quot;q-1111&quot;</span>, <span class="st">&quot;question&quot;</span> : <span class="st">&quot;Good question?&quot;</span>, <span class="st">&quot;answer&quot;</span> : <span class="st">&quot;Yes&quot;</span>, <span class="st">&quot;bonus&quot;</span>: <span class="dv">20</span>},</span>
<span id="cb7-5"><a href="#cb7-5" aria-hidden="true" tabindex="-1"></a>],</span>
<span id="cb7-6"><a href="#cb7-6" aria-hidden="true" tabindex="-1"></a><span class="st">&quot;iterator&quot;</span> : { <span class="st">&quot;questionpos&quot;</span> : <span class="dv">0</span> }</span>
<span id="cb7-7"><a href="#cb7-7" aria-hidden="true" tabindex="-1"></a>}</span></code></pre></div>
<p>Save the file.</p></li>
<li><p>Find the <code>test_trivia_calculate_scores_correct</code> section and change the assert statements to expect a score of 40.</p>
<div class="sourceCode" id="cb8"><pre class="sourceCode python"><code class="sourceCode python"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true" tabindex="-1"></a>app.TABLE.update_item.assert_called_with(</span>
<span id="cb8-2"><a href="#cb8-2" aria-hidden="true" tabindex="-1"></a>    Key<span class="op">=</span>{<span class="st">&#39;gameId&#39;</span>: <span class="st">&#39;01234567012301230123012345678901&#39;</span>, <span class="st">&#39;connectionId&#39;</span>: <span class="st">&#39;connection-1&#39;</span>},</span>
<span id="cb8-3"><a href="#cb8-3" aria-hidden="true" tabindex="-1"></a>    AttributeUpdates<span class="op">=</span>{<span class="st">&#39;score&#39;</span>: {<span class="st">&#39;Value&#39;</span>: <span class="dv">40</span>, <span class="st">&#39;Action&#39;</span>: <span class="st">&#39;PUT&#39;</span>}}</span>
<span id="cb8-4"><a href="#cb8-4" aria-hidden="true" tabindex="-1"></a>)</span>
<span id="cb8-5"><a href="#cb8-5" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb8-6"><a href="#cb8-6" aria-hidden="true" tabindex="-1"></a>app.MANAGEMENT.post_to_connection.assert_has_calls([</span>
<span id="cb8-7"><a href="#cb8-7" aria-hidden="true" tabindex="-1"></a>    mock.call(Data<span class="op">=</span><span class="st">&#39;{&quot;action&quot;: &quot;playerlist&quot;, &quot;players&quot;: [{&quot;connectionId&quot;: &quot;connection-1&quot;, &quot;playerName&quot;: &quot;AliceBlue&quot;, &quot;score&quot;: 40, &quot;currentPlayer&quot;: true}]}&#39;</span>, ConnectionId<span class="op">=</span><span class="st">&#39;connection-1&#39;</span>),</span>
<span id="cb8-8"><a href="#cb8-8" aria-hidden="true" tabindex="-1"></a>    mock.call(Data<span class="op">=</span><span class="st">&#39;{&quot;action&quot;: &quot;gameover&quot;}&#39;</span>, ConnectionId<span class="op">=</span><span class="st">&#39;connection-1&#39;</span>)</span>
<span id="cb8-9"><a href="#cb8-9" aria-hidden="true" tabindex="-1"></a>    ])</span></code></pre></div></li>
<li><p>Save the file.</p></li>
<li><p>Verify that the code is stable by running the unit test.</p>
<div class="sourceCode" id="cb9"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb9-1"><a href="#cb9-1" aria-hidden="true" tabindex="-1"></a><span class="ex">./local_build.sh</span></span></code></pre></div>
<p>You should see that everything passed.</p></li>
<li><p>(<em>Optional</em>) You can verify that the coverage is still 100 percent by previewing the <code>htmlcov/index.html</code> file.</p></li>
</ol>
<h2 id="task-4-committing-and-merging-the-new-code">Task 4: Committing and merging the new code</h2>
<p>In this final task, you commit the new code to your feature branch in AWS CodeCommit. You then merge the changes into the <code>main</code> branch.</p>
<ol type="1">
<li><p>In the <strong>AWS Cloud9</strong> terminal, view the status of your git repository by running the following command:</p>
<div class="sourceCode" id="cb10"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> status</span></code></pre></div>
<p>You should see that status of the two files changed to <em>not staged for commit</em>.</p>
<pre><code>modified:   back-end-python/gameactions/app.py
modified:   back-end-python/tests/unit/test_handler.py</code></pre></li>
<li><p>Add the files, create a commit, and push the changes to the <code>origin</code> remote.</p>
<div class="sourceCode" id="cb12"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb12-1"><a href="#cb12-1" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> add <span class="pp">*</span></span>
<span id="cb12-2"><a href="#cb12-2" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> commit <span class="at">-m</span> <span class="st">&quot;new bonus score feature&quot;</span></span>
<span id="cb12-3"><a href="#cb12-3" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> push origin feature-bonus-scores</span></code></pre></div>
<p>As of now, you haven’t made any changes to the <code>main</code> branch yet.</p></li>
<li><p>Switch back to the <strong>CodePipeline</strong> tab.</p></li>
<li><p>Choose <strong>Services</strong> and then select <strong>CodeCommit</strong>.</p></li>
<li><p>In CodeCommit, open the <code>trivia-app</code> repository and in the navigation pane, choose <strong>Commits</strong>.</p></li>
<li><p>On the right, view the bonus score commit by opening the dropdown menu with <code>main</code> and selecting <code>feature-bonus-scores</code>.</p></li>
<li><p>At the top of the window, choose the <code>trivia-app</code> breadcrumb.</p></li>
<li><p>Choose <strong>Create pull request</strong>.</p></li>
<li><p>For <strong>Destination</strong>, keep <code>main</code> selected and for <strong>Source</strong>, choose <code>feature-bonus-scores</code>.</p></li>
<li><p>Choose <strong>Compare</strong>. You can scroll down to see the code changes that you made.</p></li>
<li><p>In <strong>Details &gt; Title</strong>, enter <code>New feature: Bonus scoring</code>. Choose <strong>Create pull request</strong>.</p></li>
<li><p>Choose <strong>Merge</strong>.</p></li>
<li><p>Keep both <strong>Fast forward merge</strong> and <strong>Delete source branch feature-bonus-scores after merging?</strong> selected. Choose <strong>Merge pull request</strong>.</p></li>
<li><p>In the navigation pane, see the new merge commit by choosing <strong>Commits</strong>.</p></li>
<li><p>In the navigation pane, open the <strong>CodePipeline</strong> console by choosing <strong>Pipeline &gt; Pipelines</strong>.</p></li>
<li><p>View the pipeline details by opening <strong>trivia-pipeline</strong>. In the <strong>Source</strong> section, you should see the new commit: <em>Source: new bonus score feature</em></p></li>
<li><p>Review the <strong>Build</strong> section. The recently merged commit on the <code>main</code> branch triggered a pipeline build.</p>
<figure>
<img src="images/codepipeline-build.png" alt="pipeline_build" /><figcaption aria-hidden="true">pipeline_build</figcaption>
</figure></li>
<li><p>Switch back to the <strong>AWS Cloud9</strong> tab.</p></li>
<li><p>The new features you committed have been merged to <code>main</code>. Update the main branch locally by running the following commands:</p>
<div class="sourceCode" id="cb13"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb13-1"><a href="#cb13-1" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> checkout main</span>
<span id="cb13-2"><a href="#cb13-2" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> pull origin main</span>
<span id="cb13-3"><a href="#cb13-3" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> log</span></code></pre></div>
<p>In the Git log, you should see the <em>new bonus score feature</em> commit.</p></li>
</ol>
<h2 id="task-5-deleting-all-exercise-resources">Task 5: Deleting all exercise resources</h2>
<p>Congratulations! You have successfully completed the course project. In this task, you delete the AWS resources that you created for this project.</p>
<ol type="1">
<li>Open the <strong>AWS CodePipeline</strong> console.
<ul>
<li>Delete the <strong>trivia-pipeline</strong> pipeline.</li>
<li>Delete the <strong>trivia-unittests</strong> build project.</li>
</ul></li>
<li>Open the <strong>AWS CodeCommit</strong> console.
<ul>
<li>Delete the <strong>trivia-app</strong> repository.</li>
</ul></li>
<li>Open the <strong>Amazon Simple Storage Service (Amazon S3)</strong> console.
<ul>
<li>Empty and delete the <strong>-trivia-app-bucket</strong>.</li>
<li>Empty and delete the <strong>aws-sam-cli-managed-default-samclisourcebucket-</strong> bucket.</li>
<li>Empty and delete the <strong>codepipeline-</strong> bucket.</li>
</ul></li>
<li>Open the <strong>AWS CloudFormation</strong> console.
<ul>
<li>Delete the <strong>trivia-app</strong> stack.</li>
<li>Delete the <strong>aws-sam-cli-managed-default</strong> stack.</li>
</ul></li>
<li>Open the <strong>AWS Identity and Access Management (IAM)</strong> console.
<ul>
<li>Delete the following IAM roles.
<ul>
<li><strong>AWSCodePipelineServiceRole-&lt;region&gt;-trivia-pipeline</strong></li>
<li><strong>codebuild-trivia-unittests-service-role</strong></li>
<li><strong>cwe-role-region-trivia-pipeline</strong></li>
</ul></li>
</ul></li>
<li>Open the <strong>AWS Cloud9</strong> console.
<ul>
<li>Delete the <strong>trivia-game</strong> environment.</li>
</ul></li>
</ol>
