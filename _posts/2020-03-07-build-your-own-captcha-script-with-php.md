---
layout: post
title: "Build Your Own CAPTCHA Script with PHP"
date: 2020-03-07 00:01:00 +0800
description: "Build Your Own CAPTCHA Script with PHP" # (optional)
img: build-captcha-script-php-min.png # Add image post (optional)
fig-caption: "Build Your Own CAPTCHA Script with PHP" # Add figcaption (optional)
tags: ['PHP','Programming', 'Captcha']
categories: ['Programming', 'PHP']
---

CAPTCHA or Security code is a challenge code used in web applications to determine whether the action is performed by human being. It’s a combination of some texts, numbers on distorted image to read only by humans.

The CAPTCHA is used in every web applications where form are submitted bu users to add security to allow form submission by humans not bots.

So if you’re developing a PHP project looking for implementing your own CAPTCHA then you’re here at right place. In our previous tutorial you have learned how to create Contact Form with Google ReCaptcha with PHP, In this tutorial you will learn how to create your own CAPTCHA Script with PHP to use in your project.

In this tutorial you will learn step by step to create a HTML Form with Captcha code with live example. We will validate the Captcha code on form submission to check whether the user entered the correct Captcha code as it is appearing.

As we will cover this tutorial with live example to create own Captcha script with PHP, so the major files for this example is following.

- index.php
- get_captcha.php
- load_captcha.js

## Step1: Design HTML Form with Captcha Code

First we will create index.php file and design HTML Form with a input to enter Captcha code and display Captcha code. We will call get_captcha.php PHP script to display Captcha code on image. We will also create a button with id reloadCaptcha to load new Captcha code.

```html
<div class="container">
	<h2>Example: Build Captcha Script with PHP</h2>
	<div class="row">
		<div class="col-md-6">
			<div class="col-md-12">
				<form name="form" class="form" action="" method="post">
					<div class="form-group">
						<label for="captcha" class="text-info">
						<?php if($message) { ?>
							<span class="text-warning"><strong><?php echo $message; ?></strong></span>
						<?php } ?>
						</label><br>
						<input type="text" name="securityCode" id="securityCode" class="form-control" placeholder="Enter Security Code">
					</div>
					<div class="form-group">
						<img src="get_captcha.php?rand=<?php echo rand(); ?>" id='captcha'>
						<br>
						<p><br>Need another security code? <a href="javascript:void(0)" id="reloadCaptcha">click</a></p>
					</div>
					<div class="form-group">
						<input type="submit" name="submit" class="btn btn-info btn-md" value="Submit">
					</div>
				</form>
			</div>
		</div>
	</div>
</div>
```

## Step2: Create Captcha Code

We will create get_captcha.php file and write code to create Captcha code with image. We will have options to define Captcha image height, width, text color, font size, font style etc. to design it according to requirement. We will also store created Captcha code into SESSION variable $\_SESSION[‘captcha’] to use when validate Captcha after form submit.

```php
<?php
	session_start();
	$captcha = '';
	$captchaHeight = 60;
	$captchaWidth = 140;
	$totalCharacters = 6;
	$possibleLetters = '123456789mnbvcxzasdfghjklpoiuytrewwq';
	$captchaFont = 'monofont.ttf';
	$randomDots = 50;
	$randomLines = 25;
	$textColor = "6d87cf";
	$noiseColor = "6d87cf";
	$character = 0;
	while ($character < $totalCharacters) { 
		$captcha .= substr($possibleLetters, mt_rand(0, strlen($possibleLetters)-1), 1); 
		$character++;
	}
	$captchaFontSize = $captchaHeight * 0.65;
	$captchaImage = @imagecreate( $captchaWidth, $captchaHeight );
	$backgroundColor = imagecolorallocate( $captchaImage, 255, 255, 255 );
	$arrayTextColor = hextorgb($textColor);
	$textColor = imagecolorallocate( $captchaImage, 
						$arrayTextColor['red'], 
						$arrayTextColor['green'], 
						$arrayTextColor['blue'] );
	$arrayNoiseColor = hextorgb($noiseColor);
	$imageNoiseColor = imagecolorallocate( $captchaImage,
						$arrayNoiseColor['red'],
						$arrayNoiseColor['green'],
						$arrayNoiseColor['blue'] );
	for( $captchaDotsCount=0; $captchaDotsCount<$randomDots; $captchaDotsCount++ ) {
		imagefilledellipse( $captchaImage,
						mt_rand(0,$captchaWidth),
						mt_rand(0,$captchaHeight),
						2,
						3,
						$imageNoiseColor ); 
	}
	for( $captchaLinesCount=0; $captchaLinesCount<$randomLines; $captchaLinesCount++ ) {
		imageline( $captchaImage, 
			mt_rand(0,$captchaWidth), 
			mt_rand(0,$captchaHeight), 
			mt_rand(0,$captchaWidth), 
			mt_rand(0,$captchaHeight), 
			$imageNoiseColor ); 
	} 
	$text_box = imagettfbbox( $captchaFontSize, 0, $captchaFont, $captcha ); 
	$x = ($captchaWidth - $text_box[4])/2; 
	$y = ($captchaHeight - $text_box[5])/2; 
	imagettftext( $captchaImage, 
		$captchaFontSize, 
		0, 
		$x, 
		$y, 
		$textColor, 
		$captchaFont, 
		$captcha );

	header('Content-Type: image/jpeg');
	imagejpeg($captchaImage);
	imagedestroy($captchaImage);
	$_SESSION['captcha'] = $captcha;

	function hextorgb ($hexstring){
		$integar = hexdec($hexstring);
		return array( "red" => 0xFF & ($integar >> 0x10),
						"green" => 0xFF & ($integar >> 0x8),
						"blue" => 0xFF & $integar
				);
	}
?>
```

## Step3: Reload or Refresh Captcha Code

We will create JavaScript file load_captcha.js and implement functionality to reload another Captcha code to display when user click to refresh Captcha code.

```js
$(document).ready(function(){
	$("#reloadCaptcha").click(function(){
		var captchaImage = $('#captcha').attr('src');
		captchaImage = captchaImage.substring(0,captchaImage.lastIndexOf("?"));
		captchaImage = captchaImage+"?rand="+Math.random()*1000;
		$('#captcha').attr('src', captchaImage);
	});
});
```

## Step4: Validate Captcha Code

We will validate Captcha code on form submit to get form post value and match with Captcha code stored in SESSION.

```php
<?php
session_start();
$message = '';
if ( isset($_POST['securityCode']) && ($_POST['securityCode']!="")){
	if(strcasecmp($_SESSION['captcha'], $_POST['securityCode']) != 0){
		$message = "You have entered incorrect security code! Please try again.";
	}else{
		$message = "Your have entered correct security code.";
	}
} else {
	$message = "Enter security code.";
}
?>
```

## Step5 Conclusion

In this tutorial you have learned to build your own custom Captcha code by developing a PHP script. This is a reusable Captcha script with PHP to use further in your projects as per your requirement. You can also enhance this to make it more challenging and secure. If you have any query or face issue, feel free to submit your precious comments.

You can view the live demo from the Demo link and can download the script from the Download link below.