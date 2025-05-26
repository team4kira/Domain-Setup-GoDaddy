# Domain-Setup-GoDaddy

Pre-requisites: 
* A working website, with web address and IP Address of Web Application.
  * Azure and AWS offers some free hosting options. They both provide SSL Certifcates. 

GoDaddy 
* Sign into GoDaddy Account
* Create your domain
* Select DNS
* Edit A Record
* * Input the IP Address of your site (information found on the site hosting your website) 
* Edit or Create CNAME
  * Input the web address of your site. Note: this can take 24-hours to propagate
* Go to fowarding
  * Enter your domain name with the www. Example www.mysite.ca What this does is ensure that your naked site mysite.ca, redirects to www.mysite.ca, reducting complication with browsers accessing your website.
 
Azure (Note settings may differ for other Hosts)
* Select Custome Domain
  * Select Add - input the GoDaddy Domain as an Alias 
