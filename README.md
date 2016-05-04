# PHPSecPage-Mart
Home Made Security Page in PHP

This PHP Script is a home made protection for single page applications. 

The code that I propose is open to change . I hope that many will be helping to improve the code and make it safer .

<h2>First Control</h2>
<strong>SSL Control</strong>

```
if((!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') || $_SERVER['SERVER_PORT'] == 443) {  
       /* Run next code */ 
} 
else { 
       header("location: my_ssl_address_site"); 
      } 
```
<p>Or, if server has a different configuration you can use:</p>

```
if($_SERVER['HTTP_X_FORWARDED_HOST'] == "my_ssl_address_site") {  /* Run next code */ } 
else { header("location: my_ssl_address_site"); } 
```

<h2>Second Control</h2>
<strong>IP Control</strong>
<p>If you know the ip administrator accounts you can create a control to restrict access to the page.</p>

```
$ipClient = getenv('HTTP_CLIENT_IP')?: getenv('HTTP_X_FORWARDED_FOR')?: getenv('HTTP_X_FORWARDED')?: getenv('HTTP_FORWARDED_FOR')?: getenv('HTTP_FORWARDED')?: getenv('REMOTE_ADDR'); 
if($ipClient == "admin_ip_address") { } 
```

<p>If you have more administrators for the page you can create an Array with ip</p>

```
$IParray = array(0 => "ip_1", 1 => "ip_2");
$numIParray = count($IParray);
$auth = 0;
for($i=0; $i<$numIParray; $i++) { 
 if($IParray[$i] == $ipClient) { $auth = 1; }
}
if($auth == 1) { Ip found 
 } else { 
  /* You can save the ip in a black list and send a mail to administrator with IP information. */
}
```
<p> This code retrieve information about IP using Ip-API.com</p>

```
$query = @unserialize(file_get_contents('http://ip-api.com/php/'.$ipClient));
if($query && $query['status'] == 'success') {
	  
	  if(isset($query['country']))  {
	  $country =  $query['country']; } else { $country = "unknown"; }
	  
	  if(isset($query['city']))  {
	  $city =  $query['city']; } else { $city = "unknown"; }
	  
	} else {
	  $city = "Unknown";
	  $country = "Unknown";
	}
```
<p> You can use this service if you want save information about intruder's IP. </p>

<h3>Third Control</h3>
<strong>Token Control</strong>

