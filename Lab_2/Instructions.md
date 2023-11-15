<h2>Hands-on exercise 2: Managing volumes and data in Docker containers</h2>
<p>&nbsp;</p>
<div>&nbsp;</div>
<h3>Step 1: Create a volume</h3>
<p>&nbsp;</p>
<div>Create a new volume using the following command in <strong>terminal</strong>:</div>
<p>&nbsp;</p>
<div>
<pre class="language-javascript"><code>docker volume create mydata</code></pre>
</div>
<p>&nbsp;</p>
<div>This command will create a new volume named <em>mydata</em>.</div>
<p>&nbsp;</p>
<h3>Step 2: Mount the volume to the container</h3>
<p>&nbsp;</p>
<div>Run the following command to start the container and mount the volume to the <em>/app/data</em> directory in the container:</div>
<p>&nbsp;</p>
<div>
<pre class="language-javascript"><code>docker stop myapp
docker rm myapp
docker run -d -p 8080:80 -v mydata:/app/data --name myapp myapp</code></pre>
</div>
<p>&nbsp;</p>
<div>This command will start the container and map port 8080 on the host to port 80 in the container. The "-v" option is used to mount the "mydata" volume to the "/app/data" directory in</div>
<div>the container.</div>
<p>&nbsp;</p>
<h3>Step 3: Add data to the volume</h3>
<p>&nbsp;</p>
<div>Add some data to the volume by running the following command:</div>
<p>&nbsp;</p>
<div>
<pre class="language-javascript"><code>docker exec -it myapp bash</code></pre>
</div>
<p>&nbsp;</p>
<div>This command will start a Bash shell in the running container.</div>
<div>Once you are in the container, create a new file in the <em>/app/data</em> directory:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>echo "Hello World" &gt; /app/data/test.txt
exit</code></pre>
</div>
<p>&nbsp;</p>
<div>This will create a new file named <em>test.txt</em> in the <em>/app/data</em>&nbsp;directory of the container.</div>
<p>&nbsp;</p>
<h3>Step 4: Verify the data is persisted</h3>
<p>&nbsp;</p>
<div>To verify that the data is persisted, stop and remove the myapp container:</div>
<p>&nbsp;</p>
<div>
<pre class="language-javascript"><code>docker stop myapp
docker rm myapp</code></pre>
</div>
<p>&nbsp;</p>
<div>Then, start a new container and mount the <em>mydata</em> volume:</div>
<p>&nbsp;</p>
<div>
<pre class="language-javascript"><code>docker run -d -p 8080:80 --name myapp -v mydata:/app/data myapp</code></pre>
</div>
<p>&nbsp;</p>
<div>Finally, verify that the data is still present in the volume:</div>
<p>&nbsp;</p>
<div>
<pre class="language-javascript"><code>docker exec -it myapp cat /app/data/test.txt</code></pre>
</div>
<p>&nbsp;</p>
<div>This should output "<code>Hello, world!</code>".</div>
<p>&nbsp;</p>
<h3>Step 5: Backup the volume</h3>
<p>&nbsp;</p>
<div>To backup the volume, run the following command:</div>
<p>&nbsp;</p>
<div>
<pre class="language-javascript"><code>docker run --rm -v mydata:/volume -v $(pwd):/backup alpine tar -czvf /backup/mydata.tar.gz /volume</code></pre>
</div>
<p>&nbsp;</p>
<div>This will create a backup of the <em>mydata</em> volume in a file named <em>mydata.tar.gz</em> in the current directory.</div>
<p>&nbsp;</p>
<h3>Step 6: Create a new volume</h3>
<p>&nbsp;</p>
<div>Create a new volume using the following command:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>docker volume create mydatabackup</code></pre>
</div>
<p>&nbsp;</p>
<div>This command will create a new volume named <em>mydatabackup</em>.</div>
<p>&nbsp;</p>
<h3>Step 7: Restore the volume</h3>
<p>&nbsp;</p>
<div>To restore the volume, run the following command:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>docker run --rm -v mydatabackup:/volume -v $(pwd):/backup alpine tar -xzvf /backup/mydata.tar.gz</code></pre>
</div>
<p>&nbsp;</p>
<div>This will extract the contents of the <em>mydata.tar.gz</em>&nbsp;backup file to the <em>mydatabackup</em>&nbsp;volume.</div>
<p>&nbsp;</p>
<h3>Step 8: Verify the data is restored</h3>
<p>&nbsp;</p>
<div>To verify that the data is restored, start a new container and mount the <em>mydatabackup</em>&nbsp;volume:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>docker run -d -p 8082:80 --name myapp2 -v mydatabackup:/app/data myapp</code></pre>
</div>
<p>&nbsp;</p>
<div>Then, verify that the data is present in the volume:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>docker exec -it myapp2 sh
cat /app/data/test.txt
exit</code></pre>
</div>
<p>&nbsp;</p>
<div>This should output "<code>Hello, world!</code>".</div>
<div>&nbsp;</div>
<h2>Congratulations!</h2>
<p>&nbsp;</p>
<p>You've completed Hands-on exercise 2 where you've learned how to create and manage volumes and data on them.</p>