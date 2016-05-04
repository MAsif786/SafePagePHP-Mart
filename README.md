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
if($auth == 1) { /* Ip found */ 
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

<p>We can use a custom random string generator to create a string that we will use then. This function is not mine but is very useful to the script.</p>

```
function generateRandomString($length = 32) {
    $characters = '-0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $charactersLength = strlen($characters);
    $randomString = '';
    for ($i = 0; $i < $length; $i++) {
        $randomString .= $characters[rand(0, $charactersLength - 1)];
    }
    return $randomString;
  }
```
<p>The idea is to store the random string in our db and send it to our mail address. So...</p>

```
$generatedToken = generateRandomString();

$message = wordwrap($generatedToken, 70, "\r\n");

mail('my_email_address', 'New Generated Token', $message);
```
<p>At this point, if all steps are okay, the page show a form to send the token. 
The next code compare the tokens (sent and stored). For security the token stored in db is not the token that must be sent. The final form of authorized token is: </p>

```
md5($stra . $storedToken . $strb);
```
<p>In this way the token stored in db not is enough to have access to our SPA. The administrator must add the right strings a and b before and after the token and encrypt the string in md5.</p>

```
$accessToken = /* Retrieve information from DB */;

$stra = "something";
$strb = "something";

$encAccessToken = md5($stra . $accessToken . $strb);

if($encAccessToken == $_GET['accessToken']) { /* Access Granted */  } else { /* Send ip to administrator email */ }
```

The MIT License (MIT)

Copyright (c) 2016 Mart

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
