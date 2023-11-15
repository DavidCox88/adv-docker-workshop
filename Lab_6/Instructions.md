<h2>Hands-on exercise 6: Building and scanning Docker images</h2>
<p>&nbsp;</p>
<h3>Step 1: Build Alpine Image</h3>
<div>In this step, you will pull the Alpine Docker image from Docker Hub. Run the following command in root folder(not the actual root, rather your username folder):</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>docker pull alpine</code></pre>
</div>
<div>&nbsp;</div>
<div>This will download the Alpine Docker image from Docker Hub.</div>
<p>&nbsp;</p>
<h3>Step 2: Run Clair Server</h3>
<div>Before scanning Docker images with Clair, you need to start the Clair server. You can use Docker to run the Clair server in a container. Run the following commands to start the Clair server:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>docker run -p 5433:5432 -d --name db arminc/clair-db:latest
docker run -p 6060:6060 --link db:postgres -d --name clair arminc/clair-local-scan:latest</code></pre>
</div>
<div>&nbsp;</div>
<div>This will start the Clair server on port 6060.</div>
<p>&nbsp;</p>
<h3>Step 3: Install Clair-Scanner</h3>
<div>Next, you need to install Clair-scanner on your machine. You can do this by running the following command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>sudo curl -L https://github.com/arminc/clair-scanner/releases/download/v12/clair-scanner_linux_amd64 -o /usr/local/bin/clair-scanner
sudo chmod +x /usr/local/bin/clair-scanner</code></pre>
</div>
<div>&nbsp;</div>
<div>This will download and install Clair-scanner on your machine.</div>
<p>&nbsp;</p>
<h3>Step 4: Find Your IP</h3>
<div>You need to find the IP address of your machine to scan Docker images using Clair-scanner. Run the following command to find the docker0 IP address:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>ifconfig docker0 | grep inet</code></pre>
</div>
<div>Use inet network IP address.</div>
<div>&nbsp;</div>
<div>If the ifconfig command is not found, you can install it with the following command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>sudo apt install net-tools</code></pre>
</div>
<div>&nbsp;</div>
<p><br><br></p>
<h3>Step 5: Scan Alpine:latest Image</h3>
<div>Now that you have started the Clair server and installed Clair-scanner, you can scan the Alpine Docker image using Clair-scanner. Run the following command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>clair-scanner --ip &lt;your_ip_address&gt; alpine:latest</code></pre>
</div>
<div>&nbsp;</div>
<div>Replace <em>&lt;your_ip_address&gt;</em> with the IP address of your machine that you found in the previous step.</div>
<div>&nbsp;</div>
<div>This command will scan the Docker image with the name alpine and tag <em>latest</em>. The <em>--ip </em>option specifies the IP address of the machine running the scanner.</div>
<p>&nbsp;</p>
<h3>Step 6: Scan Myapp Image</h3>
<div>In the previous hands-on lab, you built a Docker image named myapp. You can scan this Docker image using Clair-scanner as well. Run the following command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>clair-scanner --ip &lt;your_ip_address&gt; myapp:latest</code></pre>
</div>
<div>&nbsp;</div>
<div>Replace <em>&lt;your_ip_address&gt;</em> with the IP address of your machine that you found in Step 4.</div>
<div>This command will scan the myapp Docker image for vulnerabilities.</div>
<p>&nbsp;</p>
<h2>Congratulations!</h2>
<p>You have successfully built and scanned Docker images using Clair scanner!</p>