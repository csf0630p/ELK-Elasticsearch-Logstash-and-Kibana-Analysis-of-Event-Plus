**ELK (Elasticsearch, Logstash, and Kibana): Analysis of User Behavior for Event+**


 **ELK (ElasticSearch, Logstash and Kibana)**

- Logstash is an open source, server-side data processing pipeline that ingests data from a **multitude** of sources **simultaneously** , **transforms** it, and then sends it to your favorite **stash**.
- Elasticsearch is a distributed, **RESTful**** search and analytics** engine capable of solving a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data so you can discover the expected and uncover the unexpected.
- Kibana lets you **visualize** your Elasticsearch data.

![1](https://user-images.githubusercontent.com/6482545/37438346-5a61b4e2-27c8-11e8-9a33-2ff8ab9b2325.jpg)

**Why use ELK?**

Now that we have the [**Event+ project**](https://www.patreon.com/posts/event-java-web-17280221) deployed to tomcat server on AWS EC2, we would like to know more about our users&#39; behaviours.

Since tomcat will generate logs for all of the http request sent to the web server,  and we can use Logstash to fetch and parse the tomcat logs and store the fields such as request status, response latency to the ElasticSearch for later use, for example we can utilize Kibana to visualize the requests either simply counting or creating more complex graph for data analytics.

Basically, I used **ElasticSearch to store logs that are fetched from a cloud server. Logstash to convert logs into a format that we need. Kibana to visualize the results and do some further analysis.**

**Steps to prepare for the environment:**

Install mysql/mongodb, tomcat, elasticsearch , kibana, logstash on VM on EC2

Set the ports for all web services:

![2](https://user-images.githubusercontent.com/6482545/37438367-7374406c-27c8-11e8-87b8-8e00485fe001.jpg)

**Config and start Logstash**

- Edit config file

sudo vi titan.conf

![3](https://user-images.githubusercontent.com/6482545/37438368-738431fc-27c8-11e8-90fa-667f2f78e2c6.jpg)

 This is the **Logstash** logs:

 ![4](https://user-images.githubusercontent.com/6482545/37438369-7394032a-27c8-11e8-9072-db7fb6fccc94.jpg)

![5](https://user-images.githubusercontent.com/6482545/37438370-73a60c0a-27c8-11e8-90c8-57952f19dd81.jpg)

**Kibana dashboard:**

 
![6](https://user-images.githubusercontent.com/6482545/37438371-73b6f5a6-27c8-11e8-9c67-9e68cac40e1d.jpg)
![7](https://user-images.githubusercontent.com/6482545/37438372-73c862d2-27c8-11e8-8bff-bc68794b490a.jpg)
![8](https://user-images.githubusercontent.com/6482545/37438373-73ddbec0-27c8-11e8-9f80-4dd76208b53c.jpg)

Keep sending some traffic to our webserver, then we could see a line of request count in the graph.

![9](https://user-images.githubusercontent.com/6482545/37438374-73fc8df0-27c8-11e8-9e3e-e8caf6c9e938.jpg)

Then, if we care about the **count individual type of request** , like **search, history or recommendation** , we can create graphs from each of them by specify the query argument in the search bar, for exmaple: api: search\*

![10](https://user-images.githubusercontent.com/6482545/37438375-740ce1dc-27c8-11e8-840b-7b6029c0ec88.jpg)

Then let&#39;s see if we could add count of error our server returned. In the same search bar, type api: search AND NOT status: 200 so that it&#39;ll only show you the request which caused error on server. We may need to send some bad request to webserver to have some sample data.

We can also add sub buckets to show different kinds of errors in the same graph, which is pretty useful. To Add a sub axis, click the add sub bucket button and fill in the following field:

![11](https://user-images.githubusercontent.com/6482545/37438376-741fbd98-27c8-11e8-8b40-cdd8798acea1.jpg)

Then we will see the graph like this

![12](https://user-images.githubusercontent.com/6482545/37438379-7433ab8c-27c8-11e8-81bf-b9749fc964b2.jpg)

At last, if add all the graphs to our dashboard. Click dashboard and create a new dashboard. Then select all the saved graphs from previous steps.

![13](https://user-images.githubusercontent.com/6482545/37438380-7444f266-27c8-11e8-8acd-ca64a1a2a40d.jpg)
![14](https://user-images.githubusercontent.com/6482545/37438381-74547380-27c8-11e8-81c9-b45192adec97.jpg)
![15](https://user-images.githubusercontent.com/6482545/37438382-7479cfea-27c8-11e8-9cd3-18a1f5f8ddf6.jpg)
