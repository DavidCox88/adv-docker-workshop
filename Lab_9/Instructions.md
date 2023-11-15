<h2>Hands-on exercise 5: Implementing Docker monitoring and performance optimization</h2>
<p>&nbsp;</p>
<h3>Step 1: Setting up a Monitoring Stack</h3>
<p>&nbsp;</p>
<div>To monitor Docker containers and hosts, we need to set up a monitoring stack. The monitoring stack consists of several components, including:</div>
<div>- Prometheus: A monitoring and alerting toolkit that collects metrics from targets by scraping metrics HTTP endpoints.</div>
<div>- Grafana: A dashboard and visualization platform that allows you to visualize and analyze metrics collected by Prometheus.</div>
<div>- cAdvisor: A container monitoring tool that collects metrics about containers running on a Docker host.</div>
<div>&nbsp;</div>
<div>To set up the monitoring stack, follow these steps:</div>
<p>&nbsp;</p>
<div>1. Create a new directory for the monitoring stack:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>mkdir docker-monitoring
cd docker-monitoring</code></pre>
</div>
<p>&nbsp;</p>
<div>2. Once inside newly created folder, create a new file named <em>docker-compose.yml </em>with the following contents:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>version: '3.7'   
services:   
  prometheus:   
    image: prom/prometheus:v2.28.1   
    container_name: prometheus   
    volumes:   
      - ./prometheus.yml:/etc/prometheus/prometheus.yml   
    ports:   
      - 9090:9090   
  grafana:   
    image: grafana/grafana:8.2.2   
    container_name: grafana   
    ports:   
      - 3030:3000  
  cadvisor:   
    image: gcr.io/cadvisor/cadvisor  
    container_name: cadvisor   
    ports:   
      - 8080:8080   
    volumes:   
      - /:/rootfs:ro   
      - /var/run:/var/run:rw   
      - /sys:/sys:ro   
      - /var/lib/docker/:/var/lib/docker:ro  </code></pre>
</div>
<div>&nbsp;</div>
<p>&nbsp;</p>
<div>This file specifies the services we need for our monitoring stack: Prometheus, Grafana, and cAdvisor.</div>
<p>&nbsp;</p>
<div>3. Create a new file named <em>prometheus.yml</em> with the following contents:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>global:   
  scrape_interval: 5s   
scrape_configs:   
  - job_name: 'cadvisor'   
    static_configs:   
      - targets: ['cadvisor:8080']   </code></pre>
</div>
<p>&nbsp;</p>
<div>This file specifies the configuration for Prometheus. It tells Prometheus to scrape metrics from cAdvisor every 5 seconds.</div>
<p><br><br></p>
<div>4. Start the monitoring stack:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>docker-compose up -d</code></pre>
</div>
<p>&nbsp;</p>
<div>This command starts all the services in the monitoring stack in detached mode.</div>
<div>After the services have started, you should be able to access Prometheus at <code>http://localhost:9090</code> , Grafana at <code>http://localhost:3030</code> and cAdvisor at<code> http://localhost:8080</code></div>
<p>&nbsp;</p>
<div>You can use port forwarding on GCP</div>
<p>![](https://raw.githubusercontent.com/DavidCox88/adv-docker-workshop/day2/Lab_9/images/gcp-port-forwarding.png)</p>
<div>&nbsp;</div>
<h3>Step 2: Access cAdvisor</h3>
<p>&nbsp;</p>
<div>Click on Change port and enter cAdvisor port number: 8080</div>
<p>&nbsp;</p>
<div>Click on Docker Containers</div>
<p>![](https://raw.githubusercontent.com/DavidCox88/adv-docker-workshop/day2/Lab_9/Images/cAdvisor1.png)</p>
<div>&nbsp;</div>
<p>&nbsp;</p>
<div>Choose some container just to see their status in cAdvisor:</div>
<div>![](https://raw.githubusercontent.com/DavidCox88/adv-docker-workshop/day2/Lab_9/Images/cAdvisor2.png)</div>
<p>&nbsp;</p>
<div>
<h3>Step 3: Access Prometheus</h3>
<p>&nbsp;</p>
<div>Click on Change port and enter Prometheus port number: 9090</div>
<p>Once in, choose and explore some of the metrics.</p>
<div>![](https://raw.githubusercontent.com/DavidCox88/adv-docker-workshop/day2/Lab_9/Images/prometheus.png)</div>
<p>&nbsp;</p>
<div>&nbsp;</div>
<p>&nbsp;</p>
</div>
<p>&nbsp;</p>
<h3>Step 4: Access Grafana</h3>
<p>&nbsp;</p>
<div>Change port in GCP and enter Grafana port number: 3030</div>
<p>&nbsp;</p>
<div>Log in using credentials<br>username: admin</div>
<div>password: admin</div>
<p>![](https://raw.githubusercontent.com/DavidCox88/adv-docker-workshop/day2/Lab_9/Images/grafana.png)</p>
<p><br><br></p>