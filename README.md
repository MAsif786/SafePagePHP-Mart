# PHPSecPage-Mart
Home Made Security Page in PHP

This PHP Script is a home made protection for single page applications. 

The code that I propose is open to change . I hope that many will be helping to improve the code and make it safer .

<h2>First Control</h2>
<strong>SSL Control</strong>

<code>
if((!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') || $_SERVER['SERVER_PORT'] == 443) { 
}
</code>
<p>Or, if server has a different configuration you can use:</p>
<code>
if($_SERVER['HTTP_X_FORWARDED_HOST'] == "my_ssl_address_site") {  /* Run next code */ } 
      else { 
              header("location: my_ssl_address_site"); 
            } 
</code> 
  
