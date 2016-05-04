# PHPSecPage-Mart
Home Made Security Page in PHP

This PHP Script is a home made protection for single page applications. 

The code that I propose is open to change . I hope that many will be helping to improve the code and make it safer .

<h2>First Control</h2>
<strong>SSL Control</strong>

<code>if((!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') || $_SERVER['SERVER_PORT'] == 443) {  /* Run next code */ } else { header("location: my_ssl_address_site"); } </code>
<p>Or, if server has a different configuration you can use:</p>
<code>
if($_SERVER['HTTP_X_FORWARDED_HOST'] == "my_ssl_address_site") {  /* Run next code */ } 
else { header("location: my_ssl_address_site"); } 
</code> 

<h2>Second Control</h2>
<strong>IP Control</strong>
<p>If you know the ip administrator accounts you can create a control to restrict access to the page.</p>

<code>$ipClient = getenv('HTTP_CLIENT_IP')?: getenv('HTTP_X_FORWARDED_FOR')?: getenv('HTTP_X_FORWARDED')?: getenv('HTTP_FORWARDED_FOR')?: getenv('HTTP_FORWARDED')?: getenv('REMOTE_ADDR');

if($ipClient == "admin_ip_address") { } </code>

<p>If you have more administrators for the page you can create an Array with ip</p>

<code>
$IParray = array(0 => "ip_1", 1 => "ip_2");
$numIParray = count($IParray);
$auth = 0;
for($i=0; $i<$numIParray; $i++) { 
 if($IParray[$i] == $ipClient) { $auth = 1; }
}
if($auth == 1) { Ip found 
 } else { 
  /* You can save the ip in a black list and send a mail to administrator. */
  
 }
</code>
  
