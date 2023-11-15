<h2>Hands-on exercise 8: Using Docker security tools to audit container security</h2>
<p>&nbsp;</p>
<div>&nbsp;</div>
<h3>Step 1: Install Docker Bench</h3>
<div>Clone the Docker Bench Security repository from GitHub by running the following command in the root(your username) folder:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>git clone https://github.com/docker/docker-bench-security.git</code></pre>
</div>
<div>&nbsp;</div>
<div>Change into the cloned directory by running the command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>cd docker-bench-security</code></pre>
</div>
<div>&nbsp;</div>
<div>Run the Docker Bench container by running the command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>docker build -t docker-bench-security .</code></pre>
</div>
<div>&nbsp;</div>
<div>This command will build a Docker image named <em>docker-bench-security</em>&nbsp;from the Dockerfile in the current directory.</div>
<p>&nbsp;</p>
<h3>Step 2: Run Docker Bench</h3>
<div>Once you have installed Docker Bench, you can run it by executing the following command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>sudo docker run -it --net host --pid host --userns host --cap-add audit_control  -e DOCKER_CONTENT_TRUST=$DOCKER_CONTENT_TRUST -v /var/lib:/var/lib -v /var/run/docker.sock:/var/run/docker.sock --label docker_bench_security docker-bench-security  </code></pre>
</div>
<div>&nbsp;</div>
<div>&nbsp;</div>
<div>This command will run Docker Bench and audit the security of your Docker containers.&nbsp; &nbsp;</div>
<p>&nbsp;</p>
<h3>Step 3: Review the results</h3>
<p>&nbsp;</p>
<div>After Docker Bench has completed the audit, it will generate a report detailing any security issues that were found. The report is divided into sections, with each section corresponding to a different aspect of Docker container security.</div>
<p>&nbsp;</p>
<div>Here are some of the sections you may see in the report:</div>
<div>&bull; Host Configuration: This section checks the host system's configuration for security issues that can affect Docker container security, such as kernel parameters and system services.</div>
<div>&bull; Docker Daemon Configuration: This section checks the configuration of the Docker daemon for security issues, such as using the latest version of Docker and disabling legacy registry protocols.</div>
<div>&bull; Docker Container Configuration: This section checks the configuration of individual Docker containers for security issues, such as running containers as a non-root user and disabling privileged mode.</div>
<div>&bull; Docker Image and Build Configuration: This section checks the configuration of Docker images and build processes for security issues, such as using trusted base images and enabling content trust.</div>
<div>Each section will have a status indicator (either "pass," "warn," or "fail") and a detailed description of any issues that were found.</div>
<div>&nbsp;</div>
<blockquote>
<div><strong>OPTIONAL TASK</strong>: you can look for some of the fail/warn statuses and try to fix them</div>
</blockquote>
<div>&nbsp;</div>