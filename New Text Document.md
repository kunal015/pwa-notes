# Hello
## World

`msrkdown`

```
<picture>
  <source type="image/webp" srcset="flower.webp">
  <source type="image/jpeg" srcset="flower.jpg">
  <img src="flower.jpg" alt="">
</picture>
```


We've `already` mentioned a few things you can include in a __performance budget__ such as *page* **weight** and the number of HTTP ~~requests~~, but you can split these up into more granular limits like:
Maximum size of images
Maximum number of web fonts
Maximum size of scripts, including frameworks
Total number of external resources, such as third-party scripts
However, these numbers don't tell you much about the user experience. Two pages with the same number of requests or same weight can render differently depending on the order in which
resources get requested. If a critical resource like a hero image or a stylesheet on one of the pages is loaded late in the process, the users will wait longer to see something useful and 
perceive the page as slower. If on the other page the most important parts load quickly, they may not even notice if the rest of the page doesn't.

First Contentful Paint (FCP) measures when the browser displays the first bit of content from the DOM, like text or images.

Time to Interactive (TTI) measures how long it takes for a page to become fully interactive and reliably respond to user input. It's a very important metric to track if you expect any kind
of user interaction on the page like clicking links, buttons, typing or using form elements.
Combine different metrics #
A solid performance budget combines different types of metrics. We've already defined the budget for milestone timings and now we'll add two more to the mix:
quantity-based metrics
rule-based metrics
Ask yourself what level of interaction you expect on your site. If it's a news website, users' primary goal is to read content so you should focus on rendering quickly and keeping FCP low.
Doggos.com visitors want to click on relevant links as soon as possible, so the top priority is low TTI.

Find out exactly what part of your audience browses on desktop vs. on mobile devices and prioritize accordingly. One way to figure this out is to check what your audience is doing on 
competitors' websites, through the Chrome User Experience report dashboard.

Webpack is a powerful build tool for optimizing how your code is delivered to the users. It also supports setting performance budgets based on asset size.
Turn on performance hints in webpack.config.js to get command line warnings or errors when your bundle size grows over the limit. It's a great way to stay mindful about asset sizes
throughout the development.
After the build step, webpack outputs a color-coded list of assets and their sizes. Anything over budget is highlighted in yellow.

Read This:
https://developers.google.com/web/fundamentals/performance/webpack/decrease-frontend-size

LIGHTHOUSE:-
This example budget.json file sets three separate budgets:

A budget of 125 KB for the total amount of JavaScript on the page.
A budget of 300 KB for the overall size of the page.
A budget of 10 requests for the numer of requests made to third-party origins.
You can set budgets for any of the following resource types: document, font, image, media, other,script, stylesheet, third-party, and total.

Run Lighthouse #
Run Lighthouse using the --budget-path flag. This flag tells Lighthouse the location of your budget file.

KEY POINTS:-

Optimize your images in the project by using imagemin.
Replace animated GIFs with video for faster page loads.
Create WebM videos #
While MP4 has been around since 1999, WebM is a relatively new file format initially released in 2010. 
WebM videos are much smaller than MP4 videos, but not all browsers support WebM so it makes sense to generate both.
To use FFmpeg to convert my-animation.gif to a WebM video, run the following command in your console:
ffmpeg -i my-animation.gif -c vp9 -b:v 0 -crf 41 my-animation.webm
Luckily, you can recreate these behaviors using the <video> element.
<video autoplay loop muted playsinline></video>
Finally, the <video> element requires one or more <source> child elements pointing to different video files that the browser can choose from, depending 
on the browser's format support. Provide both WebM and MP4, so that if a browser doesn't support WebM, it can fall back to MP4.
<video autoplay loop muted playsinline>
  <source src="my-animation.webm" type="video/webm">
  <source src="my-animation.mp4" type="video/mp4">
</video>
Browsers don't speculate about which <source> is optimal, so the order of <source>'s matters. For example, if you specify an MP4 video first and the browser supports WebM,
browsers will skip the WebM <source> and use the MPEG-4 instead. If you prefer a WebM <source> be used first, specify it first!

Use lazysizes to lazyload images
Lazy loading is the strategy of loading resources as they are needed, rather than in advance. 
This approach frees up resources during the initial page load and avoids loading assets that are never used.

Add the lazysizes script to your pages:
<script src="lazysizes.min.js" async></script>
Update <img> and/or <picture> tags #
<img> tag instructions
Before:
<img src="flower.jpg" alt="">
After:
<img data-src="flower.jpg" class="lazyload" alt="">
Add the lazyload class: This indicates to lazysizes that the image should be lazy loaded.
Change the src attribute to data-src: When it is time to load the image, the lazysizes code sets the image src attribute using the value from the data-src attribute.

Re-size your Images depending upon your screen size:-

The sharp package is a good choice for automating image resizing (for example, generating multiple sizes of thumbnails for all the videos on your website).
 It is fast and easily integrated with build scripts and tools.
To use sharp as a Node script, save this code as a separate script in your project, and then run it to convert your images:

const sharp = require('sharp');
const fs = require('fs');
const directory = './images';

fs.readdirSync(directory).forEach(file => {
  sharp(`${directory}/${file}`)
    .resize(200, 100) // width, height
    .toFile(`${directory}/${file}-small.jpg`);
  });



How To Resize Images and use the best image according to screensize
The "Better" approach #
For images with sizing based on…

Absolute units: Use srcset and sizes attributes to serve different images to different display densities. (Read the guide on Responsive Images here.)
"Display density" refers to the fact that different displays have different densities of pixels. All other things being equal, a high pixel density display will look sharper than a 
low pixel density display.

As a result, multiple image versions are necessary if you want users to experience the crispest possible images, regardless of the pixel density of their device.

Some sites find that this difference in image quality matters, some find that it does not.

Responsive image techniques make this possible by allowing you to list multiple image versions and for the device to choose the image that works best for it.

Relative units: Use responsive images to serve different images to display sizes. (Read the guide here.)
An image that works across all devices will be unnecessarily large for smaller devices. Responsive image techniques, specifically srcset and sizes, allow you to specify multiple 
image versions and for the device to 
choose the size that works best for it.
Resize images #
Regardless of the approach that you choose, you may find it helpful to use ImageMagick to resize your images. ImageMagick is the most popular command line tool for creating and editing images. Most people can resize images far more quickly when using the CLI than a GUI-based image editor.

Resize image to 25% the size of the original:

convert flower.jpg -resize 25% flower_small.jpg
Scale image to fit within "200px wide by 100px tall":

# macOS/Linux
convert flower.jpg -resize 200x100 flower_small.jpg

# Windows
magick convert flower.jpg -resize 200x100 flower_small.jpg
If you'll be resizing many images, you may find it more convenient to use a script or service to automate the process. You can learn more about this in the Responsive Images guide.


Convert images to WebP
The Imagemin WebP plugin can be used by itself or with your favorite build tool (Webpack/Gulp/Grunt/etc.). This usually involves adding ~10 lines of code to a build script or the 
configuration file for your build tool. Here are examples of how to do that for Webpack, Gulp, and Grunt.

If you are not using one of those build tools, you can use Imagemin by itself as a Node script. This script will convert the files in the images directory and save them in the
compressed_images directory.

const imagemin = require('imagemin');
const imageminWebp = require('imagemin-webp');

imagemin(['images/*'], 'compressed_images', {
  use: [imageminWebp({quality: 50})]
}).then(() => {
  console.log('Done!');
});
