<h1 id="exercise-using-aws-codebuild">Module-3: Using AWS CodeBuild</h1>
<p>In this exercise, you will be updating the application with a buildspec file, and using CodeBuild to test the application. The buildspec file contains the same tests you performed with the local build: linting with pylint, and unit tests with pytest. You will also use CodeBuild to run unit tests against the application, and then view the log output.</p>
<h2 id="task-1-creating-a-buildspec-file">Task 1: Creating a buildspec file</h2>
<p>In this task, you create a <em>buildspec</em> file, which CodeBuild uses to run a build. You will then check these changes into your repository.</p>
<ol type="1">
<li><p>If needed, return to AWS Cloud9 and open your development environment.</p></li>
<li><p>In the <strong>Environment</strong> pane, create a file by right-clicking the <strong>trivia-app</strong> folder and choosing <strong>New File</strong>.</p></li>
<li><p>Rename the new file to <code>buildspecs/unittests.yaml</code>. In the <strong>Confirm move to a new folder</strong> window, choose <strong>Yes</strong>.</p></li>
<li><p>Open the new file and in the new file, copy and paste the following code:</p>
<div class="sourceCode" id="cb1"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb1-1"><a href="#cb1-1" aria-hidden="true" tabindex="-1"></a><span class="fu">version</span><span class="kw">:</span><span class="at"> </span><span class="fl">0.2</span></span>
<span id="cb1-2"><a href="#cb1-2" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb1-3"><a href="#cb1-3" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb1-4"><a href="#cb1-4" aria-hidden="true" tabindex="-1"></a><span class="fu">phases</span><span class="kw">:</span></span>
<span id="cb1-5"><a href="#cb1-5" aria-hidden="true" tabindex="-1"></a><span class="at">  </span><span class="fu">install</span><span class="kw">:</span></span>
<span id="cb1-6"><a href="#cb1-6" aria-hidden="true" tabindex="-1"></a><span class="at">    </span><span class="fu">commands</span><span class="kw">:</span></span>
<span id="cb1-7"><a href="#cb1-7" aria-hidden="true" tabindex="-1"></a><span class="at">      </span><span class="kw">-</span><span class="at"> pip3 install -U -r back-end-python/gameactions/requirements.txt</span></span>
<span id="cb1-8"><a href="#cb1-8" aria-hidden="true" tabindex="-1"></a><span class="at">      </span><span class="kw">-</span><span class="at"> pip3 install -U -r back-end-python/tests/requirements.txt</span></span>
<span id="cb1-9"><a href="#cb1-9" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb1-10"><a href="#cb1-10" aria-hidden="true" tabindex="-1"></a><span class="at">  </span><span class="fu">build</span><span class="kw">:</span></span>
<span id="cb1-11"><a href="#cb1-11" aria-hidden="true" tabindex="-1"></a><span class="at">    </span><span class="fu">commands</span><span class="kw">:</span></span>
<span id="cb1-12"><a href="#cb1-12" aria-hidden="true" tabindex="-1"></a><span class="at">      </span><span class="kw">-</span><span class="at"> pylint --fail-under=8 back-end-python/gameactions/app.py</span></span>
<span id="cb1-13"><a href="#cb1-13" aria-hidden="true" tabindex="-1"></a><span class="at">      </span><span class="kw">-</span><span class="at"> pytest back-end-python/tests/unit --junit-xml=unittests.xml --cov-report=xml --cov=gameactions --cov-branch</span></span>
<span id="cb1-14"><a href="#cb1-14" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb1-15"><a href="#cb1-15" aria-hidden="true" tabindex="-1"></a><span class="fu">reports</span><span class="kw">:</span></span>
<span id="cb1-16"><a href="#cb1-16" aria-hidden="true" tabindex="-1"></a><span class="at">  </span><span class="fu">UnitTests</span><span class="kw">:</span></span>
<span id="cb1-17"><a href="#cb1-17" aria-hidden="true" tabindex="-1"></a><span class="at">    </span><span class="fu">files</span><span class="kw">:</span></span>
<span id="cb1-18"><a href="#cb1-18" aria-hidden="true" tabindex="-1"></a><span class="at">      </span><span class="kw">-</span><span class="at"> </span><span class="st">&#39;unittests.xml&#39;</span></span>
<span id="cb1-19"><a href="#cb1-19" aria-hidden="true" tabindex="-1"></a><span class="at">  </span><span class="fu">NewCoverage</span><span class="kw">:</span><span class="co"> #</span></span>
<span id="cb1-20"><a href="#cb1-20" aria-hidden="true" tabindex="-1"></a><span class="at">    </span><span class="fu">files</span><span class="kw">:</span></span>
<span id="cb1-21"><a href="#cb1-21" aria-hidden="true" tabindex="-1"></a><span class="at">      </span><span class="kw">-</span><span class="at"> </span><span class="st">&#39;coverage.xml&#39;</span></span>
<span id="cb1-22"><a href="#cb1-22" aria-hidden="true" tabindex="-1"></a><span class="at">    </span><span class="fu">file-format</span><span class="kw">:</span><span class="at"> COBERTURAXML</span></span></code></pre></div></li>
<li><p>Save the file.</p></li>
<li><p>Add the file to your AWS CodeCommit repository.</p>
<div class="sourceCode" id="cb2"><pre class="sourceCode bash"><code class="sourceCode bash"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> add <span class="pp">*</span></span>
<span id="cb2-2"><a href="#cb2-2" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> commit <span class="at">-m</span> <span class="st">&quot;adding a unittests buildspec&quot;</span></span>
<span id="cb2-3"><a href="#cb2-3" aria-hidden="true" tabindex="-1"></a><span class="fu">git</span> push origin main</span></code></pre></div></li>
</ol>
<h2 id="task-2-running-the-unit-tests-in-codebuild">Task 2: Running the unit tests in CodeBuild</h2>
<p>In this task, you use CodeBuild to run tests and the console to view the log output.</p>
<ol type="1">
<li><p>At the top left, choose the <strong>AWS Cloud9</strong> icon and choose <strong>Go To Your Dashboard</strong>.</p></li>
<li><p>Search for and choose <strong>CodeCommit</strong>.</p></li>
<li><p>In the AWS CodeCommit navigation pane, choose <strong>Repositories</strong>.</p></li>
<li><p>In the <strong>Repositories</strong> pane, open the <strong>trivia-app</strong> repository.</p></li>
<li><p>View the commit that was pushed from <strong>AWS Cloud9</strong> by going to the navigation pane and choosing <strong>Commits</strong>.</p></li>
<li><p>In the navigation pane, expand <strong>Build</strong> and then choose <strong>Build projects</strong>.</p></li>
<li><p>Choose <strong>Create build project</strong> and configure the following settings.</p>
<ul>
<li><strong>Project name</strong>: <code>trivia-unittests</code></li>
<li><strong>Repository</strong>: <code>trivia-app</code></li>
<li><strong>Branch</strong>: <em>main</em></li>
<li><strong>Operating system</strong>: <em>Ubuntu</em></li>
<li><strong>Runtime(s)</strong>: <em>Standard</em></li>
<li><strong>Image</strong>: <em>aws/codebuild/standard:5.0</em></li>
<li><strong>Build specifications</strong>: Keep <em>Use a buildspec file</em> selected</li>
<li><strong>Buildspec name - optional</strong>: <code>buildspecs/unittests.yaml</code></li>
</ul></li>
<li><p>Choose <strong>Create build project</strong>.</p></li>
<li><p>Choose <strong>Start build</strong>.</p></li>
<li><p>To see the build output, choose <strong>Tail logs</strong>.</p></li>
<li><p>When the build shows as <em>Succeeded</em>, close the log window by choosing <strong>Close</strong> (at the bottom right of the window).</p></li>
<li><p>Choose the <strong>Reports</strong> tab.</p></li>
<li><p>View the <em>Pass rate</em> by selecting the <strong>Test</strong> report, or the <em>Line coverage</em> and <em>Branch coverage</em> by selecting the <strong>Code coverage</strong> report.</p></li>
</ol>
<h6>Share the repo to your Cloud Architects and Start building project🚀</h6>
<h6>Project done with 💓 by <em>Yeshwanth</em></h6>
