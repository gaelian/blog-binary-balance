---
title: Cargo cult web development
enki_id: 6
tags: [lamp, php, fail]
---
Not too long ago, I was brought in on a failing website development project. The client had commissioned a website from one of their local web design/development companies, that company had sub-contracted the development of the site out and the web developer in question had skipped the country without finishing the project, never to be heard from again. The client was understandably perturbed by this and had lost faith in the company that they originally hired to do the work.

There is a saying that will be familiar to many developers: 'Hell is other people's code' and I have found this saying to be quite accurate much of the time. So I'm often reluctant to come in at the end of a project that's ostensibly at the FUBAR stage. But this time, circumstances were such I decided to see if I could pitch in and sort things out.

I got a hold of the source files, an SQL dump of the database and set the site up on my local machine. What I found was quite possibly the most egregious example of what I can only refer to as [cargo cult](http://en.wikipedia.org/wiki/Cargo_cult#Other_uses_of_the_term) web development that I have ever seen. This version of the website in question never went live so I have no reservations talking about it here. But I will not be naming the website or those involved in order to protect the innocent.

### Investigation

The site initially appeared to be typical [LAMP](http://en.wikipedia.org/wiki/LAMP_%28software_bundle%29) based fare. The MIA developer had apparently decided that he would create a very basic content management system with a browser based admin area from scratch. Looking deeper I first noticed that the source files were a mess. There were three or four different versions of the website stuffed into various sub-directories. Within each of these sub-directories were copies of the same file with names like 'sys_delete_file_a.php', 'sys_delete_file_b.php' and 'sys_delete_file_c.php'. There were three different files in different locations, each containing the same database connection details. A version control system was not this developer's friend. Looking at the tables in the database its self, I noted that the table containing login details was storing passwords in plain text. The alarm bells had begun.

This is the kind of thing that gives PHP developers a bad name, I thought to myself. But the best was yet to come.

### The admin interface

I mentioned that the original developer had created a browser based UI for administering aspects of the website. I punched in the URL for the admin area and was presented with a login page. A pretty standard login form appeared on the page:

```html
<table width="526" border="0" align="center" cellpadding="0" cellspacing="0">
  <tr>
    <td height="30" valign="top">&nbsp;</td>

  </tr>
  <tr>

<td width="526" height="323" valign="top" background="images/splashWelcome.gif">
	<table width="482" border="0" cellspacing="0" cellpadding="0">
      <tr>
        <td width="50" height="164" valign="top">&nbsp;</td>
        <td width="432" valign="top">&nbsp;</td>
      </tr>
      <tr>

        <td width="50" valign="top">&nbsp;</td>
        <td width="432" valign="top"><form id="form1" name="form1" method="post" action="sys_menu.php">
		  <table width="100%" border="0" cellspacing="0" cellpadding="0">
            <tr>
              <td valign="top" class="formlable">Username:</td>
              <td valign="top" class="formlable">Password:</td>
            </tr>
            <tr>

              <td valign="top"><input name="MyUsername" type="text" class="formInput" id="username" size="25" /></td>
              <td valign="top"><input name="MyPassword" type="password" class="formInput" id="password" size="25" /></td>
            </tr>
          </table>
          <p>&nbsp;            </p>
          <p>
            <input name="cmdLogin" type="submit" class="ncopyright" id="cmdLogin" value="Login" />
          </p>

          <p>&nbsp;</p>
          </form>
        </td>
      </tr>
      <tr>
        <td valign="top">&nbsp;</td>
        <td valign="top">&nbsp;</td>
      </tr>
    </table>

	</td>
  </tr>
  <tr>
    <td valign="top"><div align="center">
      <p>&nbsp;</p>
      </div></td>
  </tr>
</table>
```

Using tables for layout, non-breaking spaces all over the place... the developer was obviously not of the school that keeps such considerations as semantically correct use of (X)HTML in mind and this markup looked needlessly verbose to my eyes. But hey, there's plenty of people who still use tables for layout so I decided not to hold this against him.

I punched in the username and password that was stored in the database, the form POSTed to sys_menu.php and I was in. After poking around the admin UI for a while, I decided I would take a look at the code in sys_menu.php to see what this developer had done to secure his admin area. Here is the piece of code that greeted me at the top of sys_menu.php:

```php
<script language="php">
	session_start();

	if (isset($_SESSION["myLoginStatus"]) == false) {
		// find out if login was successful
		include("../includes/modConnectToDB.php");
		$query = "select * from sys_settings";
		$result = mysql_query($query);
		$row = mysql_fetch_array($result);

		$MyPassword1 = $row["password"];
		$MyUsername1 = $row["username"];

		$MyLogedIn = false;

		if (($_POST["MyUsername"] == $MyUsername1) and ($_POST["MyPassword"] == $MyPassword1)) {
			session_register("myLoginStatus");
			$_SESSION["myLoginStatus"] = true;
			$MyLogedIn = true;

			//send confirmation email
			include("includes/send_stats.txt");
		}

		if ($MyLogedIn == false) {

			$url = "index.php?pid=2";

			echo "<script>";
			echo " self.location='".$url."';";
			echo "</script>";

			//exit();
		}
	}
</script>
```

I'm not going to step through the code, explaining line by line, it's pretty standard (if ham-fistedly implemented) stuff and most of it is not really relevant. I did find the use of '&lt;script language="php"&gt; ... &lt;/script&gt;' to be bizarrely amusing yet enlightening, as I must admit that I wasn't even aware that you could use this (again) overly verbose syntax with PHP when just '<?php ... ?>' or even the less universally accepted '<? ... ?>' is the norm, The More You Know, hey? I also liked the way this guy prefixes some of his variable names with 'My' or 'my', it's important to know which variables are yours after all. I assume he's taking the 'my' keyword from Perl and applying it as a - IMO rather pointless - convention in PHP. But what really stole the show was this excerpt from the above:

```php
if ($MyLogedIn == false) {

	$url = "index.php?pid=2";

	echo "<script>";
	echo " self.location='".$url."';";
	echo "</script>";

	//exit();
}
```

It couldn't be, I thought to myself. But it was. This guy had secured his admin UI *using JavaScript*. When the supplied username and password are not found in the database the PHP code outputs inline JavaScript that redirects the user back to the login page with the query string containing 'pid=2' which incidentally just signals to the login page that it should show an 'Invalid username and password' error message.

in order to just make sure what I thought was happening actually was, I logged out of the admin area, disabled JavaScript in my browser and clicked the 'Login' button without entering any username or password in to the form. I was straight in to the admin area and looking at the sys_menu.php page! I was close to speechless. I'd seen some pretty dodgy code in my time but nothing like this. The only saving grace of this 'authentication system' is possibly that no one would ever think that any web developer would be brain damaged enough to use JavaScript to secure a website admin area and would never think to try exploiting such a flaw. But I don't think relying on a malicious user not to realise just how retarded you really are is a viable security strategy.

### The contact form

Now I was enthralled with this website. I needed to know what other amazing jewels of incompetence it may be hiding. There were many, but none really worth mentioning beyond this one more.

The site sported a contact form. The form contained an implementation of a [CAPTCHA](http://en.wikipedia.org/wiki/CAPTCHA). This was an image CAPTCHA and looked pretty standard upon visual inspection. It appeared that the image showed four characters and one had to type these four characters into the provided text box in order to submit your message. But looking at the source code of the page I found this:

```html
<p>
<p>
<img src="images/captcha/6.gif" width="50" height="50" />
<img src="images/captcha/3.gif" width="50" height="50" />
<img src="images/captcha/F.gif" width="50" height="50" />
<img src="images/captcha/2.gif" width="50" height="50" />
</p>
<input name="hfdCaptcha" type="hidden" id="hfdCaptcha" value="63F2" />
</p>
```

Ignoring the invalid nested paragraph tag, I realised that the CAPTCHA image was in fact not a single image, it was four images set next to each other, and each image displayed a single character of the CAPTCHA! What's more, each image was named after the character that it displayed. So from this snippet of source alone and without being able to see the actual images on the page, can you tell what characters need to be input in order to solve this CAPTCHA? That's right it's '63F2'. Just in case you can't figure that out from the four img tags, there's a convenient hidden form field that contains the required string as well. When the form is submitted the string stored in this hidden form field is compared to the string entered by the user to solve the CAPTCHA and if they're the same, then the CAPTCHA is considered solved.

On the server side, this was the code that generates the CAPTCHA images:

```php
<script language="php">
	//CAPTCHA Script
	$array_captcha[0] = "1";
	$array_captcha[1] = "2";
	$array_captcha[2] = "3";
	$array_captcha[3] = "4";
	$array_captcha[4] = "5";
	$array_captcha[5] = "6";
	$array_captcha[6] = "7";
	$array_captcha[7] = "8";
	$array_captcha[8] = "9";
	$array_captcha[9] = "A";
	$array_captcha[10] = "B";
	$array_captcha[11] = "C";
	$array_captcha[12] = "D";
	$array_captcha[13] = "E";
	$array_captcha[14] = "F";
	$array_captcha[15] = "G";
	$array_captcha[16] = "H";


	shuffle($array_captcha);

	//echo $array_captcha[0].$array_captcha[1].$array_captcha[2].$array_captcha[3];
	//$_SESSION["captcha_source"] = $array_captcha[0].$array_captcha[1].$array_captcha[2].$array_captcha[3];
	$MyCaptchaSource = $array_captcha[0].$array_captcha[1].$array_captcha[2].$array_captcha[3];;

	echo "<p>";
	echo "<img src=\"images/captcha/".$array_captcha[0].".gif\" width=\"50\" height=\"50\" />";
	echo "<img src=\"images/captcha/".$array_captcha[1].".gif\" width=\"50\" height=\"50\" />";
	echo "<img src=\"images/captcha/".$array_captcha[2].".gif\" width=\"50\" height=\"50\" />";
	echo "<img src=\"images/captcha/".$array_captcha[3].".gif\" width=\"50\" height=\"50\" />";
	echo "</p>";
</script>
```

So the code constructs four img tags out of the possible 0-9 and A-H gif images stored in a directory on the server and they are output to the page. Totally. Awesome. A better example of cargo cult web development I have never seen. This implementation of a CAPTCHA deftly and completely negates the reason for its own existence. As with the previous admin area 'authentication' system, the only possible saving grace of this CAPTCHA might be that no one writing a spam bot would think that anyone would be this stupid and would likely not have written their spam bot to take advantage of such obvious buffoonery.

### Happily ever after

There was a happy ending to this story. The client's website has been live for some time now and I'm relieved to say that none of this hilariously bad code made it onto the production website. In fact, most of the website was trashed aside from the visual design.

Just because a website looks presentable on the surface doesn't necessarily mean that it is any good underneath the facade. And it's too bad this isn't more obvious to the people who typically commission such websites.
