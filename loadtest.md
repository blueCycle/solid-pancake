Preparing your Load Test

Now that we have monitoring enabled we will simulate heavy load to our EKS Cluster hosting our Wordpress install. While generating the load, we can watch CloudWatch Container Insights for the performance metrics.
Install Siege for load testing on your Workspace:

Download Siege by running the below command in your Cloud9 terminal.

```
curl -C - -O http://download.joedog.org/siege/siege-latest.tar.gz
```

Once downloaded we’ll extract this file and change to the extracted directory. The version may change but you can see the directory name created via the output of the tar command.

```
tar -xvf siege-latest.tar.gz
cd siege-4.0.4

./configure
make all
sudo make install 

siege --version

```

Now that Siege is setup and running, we’re going to generate some load to our Wordpress site. With that load we can see the metrics change in CloudWatch Container Insights.

From your terminal window in the Siege directory, run the following command.

```
siege -c 200 -i {YOURLOADBALANCER URL}
```

This command tells Siege to run 200 concurrent connections to your Wordpress site at varying URLS. You should see an output like the below. At first it will show connections to the root of your site, and then you should start to see it jump around to various URLS of your site.



