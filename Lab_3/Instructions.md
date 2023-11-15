<h2>Hands-on exercise 3: Configuring advanced Docker networking scenarios</h2>
<div>&nbsp;</div>
<br>
<h3>Step 1: Creating a custom bridge network</h3>
<br>
<div>In this task, you will learn how to create a custom network and attach containers to it.</div>
<div>To create a custom network, run the following command:</div>
<br>
<div>
<pre class="language-markup"><code>docker network create mynetwork</code></pre>
</div>
<br>
<div>This will create a new network named <em>mynetwork</em>.</div>
<br>
<h3>Step 2: Attach container to the network</h3>
<br>
<div>To attach containers to the network, run the following command:</div>
<br>
<div>
<pre class="language-markup"><code>docker network connect mynetwork myapp</code></pre>
</div>
<br>
<div>This will connect existing container to the existing network.</div>
<br>
<h3>Step 3: Create new container using existing image and network</h3>
<br>
<div>To create new container, run the following command:</div>
</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>docker run -d -p 8083:80 --name myapp3 --network mynetwork myapp</code></pre>
<br><br>
<div>This will start a container named <em>myapp3 </em>and attach it to the <em>mynetwork</em> network.</div>
<br>
<h3>Step 4: Verify the network configuration</h3>
<br>
<div>To verify the network configuration, run the following command:</div>
<br>
<div>
<pre class="language-markup"><code>docker inspect myapp
docker inspect network mynetwork</code></pre>
</div>
<br>
<div>This will display the container configuration, including the network configuration.</div>
<br>
<h3>Step 5: Test the connection</h3>
<br>
<div>There are various ways to test the connection between the two containers. Here are a few methods:</div>
<br>
<div>&emsp;</div>
<div><strong><em>a) Use the container IP addresses</em></strong></div>
<br>
<div>You can test the connection using the container IP addresses. Run the following commands to get the IP addresses of the two containers:</div>
<br>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>docker inspect -f '{{ .NetworkSettings.IPAddress }}' myapp
docker inspect -f '{{ .NetworkSettings.IPAddress }}' myapp3</code></pre>
</div>
<div>&nbsp;</div>
<div>If you don&rsquo;t see IP address for <em>myapp3</em> run the following command:</div>
<br>
<div>
<pre class="language-markup"><code>docker inspect myapp3</code></pre>
</div>
<br>
<div>Use the IP address of <em>myapp3</em>&nbsp;container in the following command to test the connection from <em>myapp</em>:</div>
<br>
<div>
<pre class="language-markup"><code>docker exec myapp ping &lt;myapp3_ip_address&gt;</code></pre>
</div>
<div>&nbsp;</div>
<div>If ping still does not work, exec into <em>myapp</em> container and install the ping utility by running the following commands:</div>
<br>
<div>
<pre class="language-markup"><code>docker exec -it myapp /bin/bash


apt-get update -y
apt-get install -y iputils-ping</code></pre>
</div>
<div>&nbsp;</div>
<div>This will update the package list and install the ping utility. Exit the container and try to&nbsp;ping again.</div>
<br>
<div><strong><em>b) Use the container names</em></strong></div>
<br>
<div>You can also test the connection between the two containers using their names. Run the following command to test the connection from <em>myapp</em> to <em>myapp3</em>:</div>
<br>
<div>
<pre class="language-markup"><code>docker exec -it myapp /bin/bash
/app# ping myapp3
/app# exit</code></pre>
</div>
</div>
<p>&nbsp;</p>
<div>
<h3>Step 6: Isolating Containers with Networks</h3>
<br>
<div>You are going to use Docker networks to isolate containers, and to do so, first you will c<span style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;">reate a new network.</span></div>
<div>&nbsp;</div>
<div>To create a new network, run the following command:</div>
<br>
<div>
<pre class="language-markup"><code>docker network create myisolatednetwork</code></pre>
</div>
<br>
<div>This will create a new network named <em>myisolatednetwork</em> with the <em>bridge</em> driver.</div>
<br>
<h3>Step 7: Start a container on the isolated network</h3>
<br>
<div>To start a container on the isolated network, run the following command:</div>
<br>
<div>
<pre class="language-markup"><code>docker run -d --name isolatedcontainer --network myisolatednetwork nginx</code></pre>
</div>
<br>
<div>This will start a new container named <em>isolatedcontainer</em> on the <em>myisolatednetwork</em> network.</div>
<br>
<div>
<pre class="language-markup"><code>docker run -d --name isolatedcontainer2 --network myisolatednetwork myapp</code></pre>
</div>
</div>
<div><br>
<h3>Step 8: Verify network isolation</h3>
<br>
<div>To verify network isolation, attempt to ping the <em>isolatedcontainer</em> from <em>myapp</em> container:</div>
<br>
<div>
<pre class="language-markup"><code>docker exec myapp ping isolatedcontainer</code></pre>
</div>
<br>
<div>This should fail to ping the <em>isolatedcontainer</em> since it is on a different network.</div>
<br>
<div>&emsp;</div>
</div>
<p>&nbsp;</p>
<h2>Congratulations!</h2>
<p>&nbsp;</p>
<p>You've completed Hands-on exercise 3 where you have learned how to create, manage network and use them to isolate the containerized workload in Docker.</p>