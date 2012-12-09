---
layout: post
title: "Be Careful with NSString's Hash"
date: 2012-12-08 11:24
comments: true
external-url: 
categories: iOS programming
---


My recent iOS project, Aimeiwei, is an image-oriented app focused on food. We use lots of images, and one picture user updated can have two different aspect ratios, one is the original square food picture, and one is a cropped rectangle picutre that can possiblly be used as the restaurant's header image. I utilized [EGOImageLoading](https://github.com/enormego/EGOImageLoading) to cache and display images from our remote server. But I came across a strange problem: ***Sometimes*** the rectangle image view displays the square one instead, dispite the fact that the urls of the two images are different and the url of the rectangle one points to the correct rectangle image.

After a long time digging, I found that the problem rooted in the `hash` method of `NSString`. These two url: 
	http://aizheke-img-dev.b0.upaiyun.com/imagefs/amw/food_story/50b85c545c168b2e6000003f/image/banner_46664ae45ffaff8ad1ab1cd55d38b6ac.jpg 
and 
	http://aizheke-img-dev.b0.upaiyun.com/imagefs/amw/food_story/50b85c545c168b2e6000003f/image/medium_46664ae45ffaff8ad1ab1cd55d38b6ac.jpg

Their hash is identical! (And [EGOImageLoading](https://github.com/enormego/EGOImageLoading) uses the hash result as a key to cache the image. So if the square image had been cached, the rectangle view used the cached square image to display.)

Then I found this [article](http://www.mulle-kybernetik.com/artikel/Optimization/opti-7.html) back in 2004, it says: 

	NSString/CFString/NSURL

	The hash is a convolution of the first and last eight bytes plus the length of the string basically the byte values are shifted and added to the string length.

I do know that the hash result is not guaranteed to be unique, but I don't expect a "weak unique" like these. The implementation of `hash` must have been improved after these years because the data provided in the article no longer give the same result. But as of now 2012, on iOS 6 SDK, those two urls in my project still yield the same hash.

The fix is simple, just use MD5 as the key. Now I use this [fork](https://github.com/jonkean/EGOImageLoading) instead. If someday I had time, I may give [SDWebImage](https://github.com/rs/SDWebImage) a try.




