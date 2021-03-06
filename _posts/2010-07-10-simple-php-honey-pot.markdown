---
author: Monji Dolon
comments: true
date: 2010-07-10 22:20:11+00:00
layout: post
slug: simple-php-honey-pot
title: Quick and Simple PHP Honey Pot Spam Prevention
wordpress_id: 1369
categories:
- Tutorials
tags:
- contact form
- honey pot
- php
- programming
- Tutorials
---

This technique has been floating around the web for the past few weeks so it's definitely worth sharing here.  Using standard HTML, CSS and a little PHP, you can filter out a lot of robots and spammers that crawl the web for unsuspecting forms.  Credit to [Aaron James Young](http://www.aaronjamesyoung.com/) for reminding me about this technique (he posted a similar snippet on [Forrst](http://forrst.com/)).

A honey pot trap involves creating a form with an extra field that is hidden to human visitors but readable by robots.  The robot fills out the invisible field and submits the form, leaving you to simply ignore their spammy submission or blacklist their IP.  It's a very simple concept that can be implemented in a few minutes and it just works - add them to your contact and submission forms to help reduce spam.  I've used them extensively in my last few projects, I've found it to be well worth the small time investment.

## Contact Form Example

Here is an example of a simple contact form that uses the honey pot spam prevention method:
<div class="download">
  <a href="http://demos.devgrow.com/honeypot/honeypot.zip" class="primary">Download</a>
  <a href="http://demos.devgrow.com/honeypot/" class="secondary">Preview</a>
</div>

### The HTML:

{% highlight html linenos=table %}
<form method="post" action="">
  <fieldset>
    <legend>Contact Me</legend>
    <p>
      <label>Name:</label>
      <input name="name" type="text" id="name" />
    </p>
    <p>
      <label>E-mail:</label>
      <input name="email" type="text" id="email" />
    </p>
    <p>
      <label>Message:</label>
      <textarea name="message" id="message"></textarea>
    </p>
    <!-- The following field is for robots only, invisible to humans: -->
    <p class="robotic" id="pot">
      <label>If you're human leave this blank:</label>
      <input name="robotest" type="text" id="robotest" class="robotest" />
    </p>
    <p>
      <input type="submit" value="Send Message" class="submit" />
    </p>
  </fieldset>
</form>
{% endhighlight %}

### The PHP:

{% highlight php linenos=table %}
<?php
  if($_POST){
    $to = 'your-email-here@gmail.com';
    $subject = 'Contact Form Submission';
    $from_name = $_POST['name'];
    $from_email = $_POST['email'];
    $message = $_POST['message'];
    $robotest = $_POST['robotest'];
    if($robotest)
      $error = "You are a gutless robot.";
    else{
      if($from_name && $from_email && $message){
        $header = "From: $from_name <$from_email>";
        if(mail($to, $subject, $message, $header))
          $success = "You are human and your message was sent!";
        else
          $error = "You are human but there was a problem sending the e-mail.";
      }else
        $error = "All fields are required.";
    }
    if($error)
      echo '<div class="msg error">'.$error.'</div>';
    elseif($success)
      echo '<div class="msg success">'.$success.'</div>';
  }
?>
{% endhighlight %}

### The CSS:

{% highlight css linenos=table %}
.robotic { display: none; }
{% endhighlight %}

In the above example link, I added some optional CSS to make it look a little nicer.  Also, you'll notice the actual e-mail form doesn't work, as the to e-mail address is a fake one.  The contact form part is just to illustrate the honey pot, the important thing to notice is that the last text field is hidden using CSS (the entire paragraph) and that if text is entered in the field, the entire form fails.

## Conclusion

This concept is certainly not my own and in fact, there is even an entire project dedicated to catching spammers using dummy forms and blacklisting their IP addresses (rightfully called [Project Honey Pot](http://www.projecthoneypot.org/)).  Again thanks to [Aaron](http://www.aaronjamesyoung.com/) for reminding me about this technique, I've been meaning to do a write up on it for a while now.  This post is also thanks to [Forrst](http://forrst.com/), I'm quite pleasantly surprised at the amount of interesting information I'm finding there.