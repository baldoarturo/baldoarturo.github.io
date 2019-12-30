---
id: 282
title: Using Zabbix API for Custom Reports
date: 2019-01-04T14:31:11-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=282
permalink: /using-zabbix-api-for-custom-reports/
categories:
  - Projects
tags:
  - api
  - html
  - javascript
  - php
  - zabbix
---
<span class="wsc-spelling-problem" data-spelling-word="Zabbix" data-wsc-lang="en_US">Zabbix</span> is an open source monitoring tool for diverse IT components, including networks, servers, virtual machines (<span class="wsc-spelling-problem" data-spelling-word="VMs" data-wsc-lang="en_US">VMs</span>) and cloud services. It provides monitoring metrics, among others network utilization, CPU load and disk space consumption. Data can be collected in a agent-less fashion using SNMP, ICMP, or with an multi-platform agent, available for most operating systems.

Even when it is considered one of the best NMS on the market, its reporting capabilities are very limited. For example, this is an availability report created with PRTG.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/hlassets.paessler.com/common/files/screenshots/prtg-reportfiber.png?w=640&#038;ssl=1" alt="Image result for prtg reports" data-recalc-dims="1" /></figure>
</div>

And this is a <span class="wsc-spelling-problem" data-spelling-word="Zabbix" data-wsc-lang="en_US">Zabbix</span> Report. There is no graphs, no data tables, and it is difficult to establish a defined time span for the data collection.  
  
<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-17.png?w=640&#038;ssl=1" alt="" class="wp-image-284" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-17.png?w=896&ssl=1 896w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-17.png?resize=300%2C245&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-17.png?resize=768%2C628&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure> 

My client required an executive report with the following information.

  * Host / Service Name
  * Minimum SLA for ICMP echo request monitoring
  * Achieved SLA for ICMP echo request monitoring 
  * Memory usage graph, if host is being SNMP-monitored
  * Main network interface graph, if host is being SNMP-monitored 
  * And storage usage graph, also if the host is being SNMP-monitored  
    

<p class="has-medium-font-size">
  <strong>Using the Zabbix API</strong>
</p>

To do call the API, we need to send HTTP POST requests to the <span class="wsc-spelling-problem" data-spelling-word="api" data-wsc-lang="en_US"><em>api</em></span>__jsonrpc.php_ file located in the frontend directory. For example, if the Zabbix frontend is installed under _http://company.com/_<span class="wsc-spelling-problem" data-spelling-word="zabbix" data-wsc-lang="en_US"><em>zabbix</em></span>, the HTTP request to call the apiinfo.version method may look like this:

<pre class="wp-block-code"><code>POST http://company.com/zabbix/api_jsonrpc.php HTTP/1.1
Content-Type: application/json-rpc
{
    "jsonrpc":"2.0",
    "method":"apiinfo.version",
    "id":1,
    "auth":null,
    "params":
        {
        }
}</code></pre>

The request must have the Content-Type header set to one of these values: _application/_<span class="wsc-spelling-problem" data-spelling-word="json-rpc" data-wsc-lang="en_US"><em>json-rpc</em></span>_, application/_<span class="wsc-spelling-problem" data-spelling-word="json" data-wsc-lang="en_US"><em>json</em></span> _or application/_<span class="wsc-spelling-problem" data-spelling-word="jsonrequest" data-wsc-lang="en_US"><em>jsonrequest</em></span>.

Before access any data, it&#8217;s necessary to log in and obtain an authentication token. The _user.login_ method is used for this. 

<pre class="wp-block-code"><code>{
    "jsonrpc": "2.0",
    "method": "user.login",
    "params": {
        "user": "Admin",
        "password": "zabbix"
    },
    "id": 1,
    "auth": null
}</code></pre>

If the authentication request succeeds, the API response will look like this.

<pre class="wp-block-code"><code>{
    "jsonrpc": "2.0",
    "result": "0424bd59b807674191e7d77572075f33",
    "id": 1
}</code></pre>

The _result_ field is the authentication token, which will be sent on subsequent requests.

Instead of reinvent the wheel, let&#8217;s use a existing library to call the API. 

<p class="has-medium-font-size">
  <strong>Using <span class="wsc-spelling-problem" data-spelling-word="jqzabbix" data-wsc-lang="en_US">jqzabbix</span> <span class="wsc-spelling-problem" data-spelling-word="jQuery" data-wsc-lang="en_US">jQuery</span> plugin for the Zabbix API</strong>
</p>

<span class="wsc-spelling-problem" data-spelling-word="GitHub" data-wsc-lang="en_US">GitHub</span> user _<span class="wsc-spelling-problem" data-spelling-word="kodai" data-wsc-lang="en_US">kodai</span>_ provides a nice JavaScript client, in a form of a <span class="wsc-spelling-problem" data-spelling-word="jQuery" data-wsc-lang="en_US">jQuery</span> plugin. You can get it on [https://github.com/kodai/jqzabbix.](https://github.com/kodai/jqzabbix)

The usage is quite forward, first, include both <span class="wsc-spelling-problem" data-spelling-word="jQuery" data-wsc-lang="en_US">jQuery</span> and _jqzabbix.js_ on your HTML file. I using Cloudflare to link <span class="wsc-spelling-problem" data-spelling-word="jQuery" data-wsc-lang="en_US">jQuery</span>.

<pre class="wp-block-code"><code>&lt;script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js">/script>
&lt;script type="text/javascript" charset="utf-8" src="jqzabbix.js">&lt;/script></code></pre>

An object has to be created to initialize the client. I prefer to set _<span class="wsc-spelling-problem" data-spelling-word="url" data-wsc-lang="en_US">url</span>_, _username_<u>,</u> and _password_ dynamically, with data provided by the end user, so no credentials are stored here.

<pre class="wp-block-code"><code>server = new $.jqzabbix({
	url: url,  			// URL of Zabbix API
	username: user,   	// Zabbix login user name
	password: pass,  	// Zabbix login password
	basicauth: false,   // If you use basic authentication, set true for this option
	busername: '',      // User name for basic authentication
	bpassword: '',      // Password for basic authentication
	timeout: 5000,      // Request timeout (milli second)
	limit: 1000,        // Max data number for one request
});</code></pre>

As told before, the first step is to authenticate with the API, and save the authorization token. This is handled by the _<span class="wsc-spelling-problem" data-spelling-word="jqzabbix" data-wsc-lang="en_US">jqzabbix</span>_ library by first making a request to get the API version, and then authenticating.

<pre class="wp-block-code"><code>server.getApiVersion();
server.userLogin();</code></pre>

If the authentication procedure is completed properly, the API version and authentication ID are stored as properties of the _server_ object. The _<span class="wsc-spelling-problem" data-spelling-word="userlogin" data-wsc-lang="en_US">userlogin</span>()_ method allows to set callbacks for both success and error.

<pre class="wp-block-code"><code>var success = function() { console.log('Success!'); }
var error = function() { console.error('Error!'); }

server.userLogin(null, success, error)</code></pre>

Once authenticated, the <span class="wsc-spelling-problem" data-spelling-word="Zabbix" data-wsc-lang="en_US">Zabbix</span> API methods are called in the following fashion with the _<span class="wsc-spelling-problem" data-spelling-word="sendAjaxRequest" data-wsc-lang="en_US">sendAjaxRequest</span>_ method.

<pre class="wp-block-code"><code>server.sendAjaxRequest(method, params, success, error)</code></pre>

**Retrieving Hosts**

I set a global array _hosts_ to store the hosts information.  
Another global array called _SEARCH_GROUPS_ is used to define which hosts groups should considered on the API request. By setting the _<span class="wsc-spelling-problem" data-spelling-word="selectHosts" data-wsc-lang="en_US">selectHosts</span>_ parameter to _true_, the hosts on the host groups are retrieved too on the response. 

On success, the result is stored on the _hosts_ array, and the _get_graphs_ function is called. If there is an error, the default _error_ callback is fired.

<pre class="wp-block-code"><code>hosts = [];
function get_hosts() {
	// Get hosts
	server.sendAjaxRequest(
		"hostgroup.get",
		{
			"selectHosts": true,
			"filter": {
				"name": SEARCH_GROUPS
			}
		},
		function (e) {
			e.result.forEach(group => {
				group.hosts.forEach(host => {
					hosts.push(host);
				});
			});
			get_graphs();
		},
		error,
	);
}</code></pre>

**Retrieving Graphs**

Previously, user defined graphs were configured on <span class="wsc-spelling-problem" data-spelling-word="Zabbix" data-wsc-lang="en_US">Zabbix</span>, to match the client <span class="wsc-spelling-problem" data-spelling-word="requeriments" data-wsc-lang="en_US">requeriments</span> of specific information. All names for the graphs that should be included on the report were terminated the _&#8221; &#8211; Report&#8221;_ suffix.

This function retrieves all those graphs, and by setting the _<span class="wsc-spelling-problem" data-spelling-word="selectHosts" data-wsc-lang="en_US">selectHosts</span>_ to true, the hosts linked to each graph are retrieved too.

On success, the result is stored on the _graphs_ array, and the _render_ <font color="#191e23"><span style="background-color: rgb(232, 234, 235);"></font>function is called. If there is an error, the default <em>error</em> callback is fired. </p> 

<pre class="wp-block-code"><code>graphs = [];
function get_graphs() {
	server.sendAjaxRequest(
		"graph.get",
		{
			"selectHosts": "*",
			"search": {
				name: "- Report"
			}
		},
		function (e) {
			graphs = e.result;
			render();
		},
		error
	)
}</code></pre>

<p>
  <strong>Retrieving Graphs Images Instead of Graph Data</strong>
</p>

<p>
  By this time you should have noticed that the <span class="wsc-spelling-problem" data-spelling-word="Zabbix" data-wsc-lang="en_US">Zabbix</span> API allows to retrieve values for the graphs, but no images. An additional PHP file will be stored with the HTML and JS files, as a helper to call the web interface by using <em>php_curl</em>.
</p>

<p>
  You can get it on <em>https://zabbix.org/wiki/Get_Graph_Image_PHP</em>. I made a couple modifications to it in order to pass username and password on the URL query, with parameters for the graph ID, the <span class="wsc-spelling-problem" data-spelling-word="timespan" data-wsc-lang="en_US">timespan</span>, and the image dimensions.
</p>

<pre class="wp-block-code"><code>&lt;?php
//////////
// GraphImgByID v1.1 
// (c) Travis Mathis - travisdmathis@gmail.com
// It's free use it however you want.
// ChangeLog:
// 1/23/12 - Added width and height to GetGraph Function
// 23/7/13 - Zabbix 2.0 compatibility
// ERROR REPORTING
error_reporting(E_ALL);
set_time_limit(1800);


$graph_id = filter_input(INPUT_GET,'id');
$period= filter_input(INPUT_GET,'period');
$width= filter_input(INPUT_GET,'width');
$height = filter_input(INPUT_GET,'height');
$user = filter_input(INPUT_GET,'user');
$pass = filter_input(INPUT_GET,'pass');

//CONFIGURATION
$z_server = 'zabbix_url'; //set your URL here
$z_user = $user;
$z_pass = $pass;
$z_img_path = "/usr/local/share/zabbix/custom_pages/tmp_images/";

//NON CONFIGURABLE
$z_tmp_cookies = "";
$z_url_index = $z_server . "index.php";
$z_url_graph = $z_server . "chart2.php";
$z_url_api = $z_server . "api_jsonrpc.php";

// Zabbix 1.8
// $z_login_data  = "name=" .$z_user ."&password=" .$z_pass ."&enter=Enter";
// Zabbix 2.0
$z_login_data = array('name' => $z_user, 'password' => $z_pass, 'enter' => "Sign in");

// FUNCTION
function GraphImageById($graphid, $period = 3600, $width, $height) {
    global $z_server, $z_user, $z_pass, $z_tmp_cookies, $z_url_index, $z_url_graph, $z_url_api, $z_img_path, $z_login_data;
    // file names
    $filename_cookie = $z_tmp_cookies . "zabbix_cookie_" . $graphid . ".txt";
    $image_name = $z_img_path . "zabbix_graph_" . $graphid . ".png";

    //setup curl
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $z_url_index);
    curl_setopt($ch, CURLOPT_HEADER, false);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_BINARYTRANSFER, true);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $z_login_data);
    curl_setopt($ch, CURLOPT_COOKIEJAR, $filename_cookie);
    curl_setopt($ch, CURLOPT_COOKIEFILE, $filename_cookie);
    // login
    curl_exec($ch);
    // get graph
    curl_setopt($ch, CURLOPT_URL, $z_url_graph . "?graphid=" . $graphid . "&width=" . $width . "&height=" . $height . "&period=" . $period);
    $output = curl_exec($ch);
    curl_close($ch);
    // delete cookie
    header("Content-type: image/png");
    unlink($filename_cookie);
    /*
      $fp = fopen($image_name, 'w');
      fwrite($fp, $output);
      fclose($fp);
      header("Content-type: text/html");
     */
    return $output;
}

echo GraphImageById($graph_id, $period, $width, $height);</code></pre>

<p>
  <strong>Quick and Dirty Frontend</strong>
</p>

<p>
  You should be able to customize this small frontend to your needs.
</p>

<pre class="wp-block-code"><code>&lt;html>

&lt;head>
	&lt;link rel="stylesheet" href="https://unpkg.com/chota@latest">
	&lt;script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js">&lt;/script>
	&lt;script src="https://cdn.jsdelivr.net/npm/js-cookie@2/src/js.cookie.min.js">&lt;/script>
	&lt;script src="jqzabbix.js">&lt;/script>
	&lt;style>
		.host-container {
			margin-bottom: 3em;
		}
		@media print {
			.host-container {
				page-break-before: auto;
				page-break-after: auto;
				page-break-inside: avoid;
			}
			img {
				display: block;
				page-break-before: auto;
				page-break-after: auto;
				page-break-inside: avoid;
			}
		}
	&lt;/style>
&lt;/head>

&lt;body>
	&lt;div id="container" class="container">

		&lt;div class="row" style="margin-bottom: 3em">
			&lt;div class="col">
				&lt;h2>Services and Availability Report&lt;/h2>
				&lt;table id="table" class="bg-dark">
					&lt;thead>
						&lt;th>Host Name&lt;/th>
						&lt;th>Target&lt;/th>
						&lt;th class="is-text-center">Availibilty&lt;/th>
						&lt;th class="is-text-center">Availabilty Status&lt;/th>
						&lt;th class="is-text-center">Total Availability&lt;/th>
					&lt;/thead>
				&lt;/table>
			&lt;/div>
		&lt;/div>


		&lt;div id="template" style="display: none">
			&lt;div class="host-container">
				&lt;div class="row bg-dark">
					&lt;div class="col-12">
						&lt;span id="host-HOST_ID-name">Service Name&lt;/span>
					&lt;/div>
				&lt;/div>
				&lt;div class="row bg-light">
					&lt;div class="col-3">
						Status
					&lt;/div>
					&lt;div class="col-3">
						SLA Minimum
					&lt;/div>
					&lt;div class="col-3">
						SLA
					&lt;/div>
				&lt;/div>
				&lt;div class="row bg-primary">
					&lt;div class="col-3">
						&lt;span id="host-HOST_ID-status">&lt;/span>OK&lt;/span>
					&lt;/div>
					&lt;div class="col-3">
						&lt;span id="host-HOST_ID-sla">&lt;/span>99.9%
					&lt;/div>
					&lt;div class="col-3">
						&lt;span id="host-HOST_ID-sla-value">&lt;/span>100%
					&lt;/div>
				&lt;/div>
				&lt;div class="row is-text-center" id="host-HOST_ID-graphs">
				&lt;/div>
			&lt;/div>
		&lt;/div>

	&lt;/div>

	&lt;script src="ui.js">&lt;/script>

&lt;/body>

&lt;/html></code></pre>

<p>
  <strong>Result</strong>
</p>

<p>
  The final page is a complete report, including a briefing table which resumes the services status and SLA compliance.
</p><figure class="wp-block-image">

<img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-18.png?w=640&#038;ssl=1" alt="" class="wp-image-296" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-18.png?w=800&ssl=1 800w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-18.png?resize=281%2C300&ssl=1 281w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-18.png?resize=768%2C819&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /></figure>