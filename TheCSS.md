Many of us have cut our teeth using the CSS background-image proper­ty to do a wide vari­ety of things. To many, it’s like an old friend, but it’s one we should con­sid­er say­ing good­bye to.

我们中的许多人已经使用CSS background-image属性切割牙齿来做各种各样的事情。对许多人来说，这就像一位老朋友，但我们应该考虑告别。

The real prob­lem isn’t the CSS `background-image` prop­er­ty itself. Rather it is that it’s been used in places where it real­ly should­n’t be, such as for the main CTA hero image or for UI images.

真正的问题不是CSS background-image属性本身。相反，它被用于它不应该的地方，例如主CTA英雄图像或UI图像。

Used improp­er­ly, `background-image` can be an anti-pat­tern. Are there legit use cas­es for `background-image`? Of course.

使用不当，背景图像可以是反模式。是否存在背景图像的合法用例？当然。

How­ev­er, there are some seri­ous down­sides to using the CSS `background-image` prop­er­ty, and more impor­tant­ly, we have bet­ter ways to imple­ment images in our browsers today.

但是，使用CSS background-image属性有一些严重的缺点，更重要的是，我们现在有更好的方法在我们的浏览器中实现图像。

## 我们不要谈论跳舞仓鼠背景的日子
> Let us not speak of the days of dancing hamster backgrounds 

So let’s jump right in and talk about the down­sides of using the CSS `background-image` prop­er­ty, and then what we can use instead.

因此，让我们直接进入并讨论使用CSS background-image属性的缺点，然后我们可以使用它。

Here are a few rea­sons why using the CSS `background-image` prop­er­ty can be bad:

以下是使用CSS background-image属性可能很糟糕的几个原因：

LINK 1. BAD FOR SEO
Images that are in CSS background-image prop­er­ties are not crawled or indexed by Google. No big deal right? Well, check out this quote from the How to Rank in Google Image Search Moz​.com article:

…a third of all searches performed in Google are for images and 12.5% of SERPs show Image Pack results…
Let that sink in. One third or 33% of search­es per­formed in Google are for images. If the image is rel­e­vant at all to the sub­ject of the page, or your clien­t’s busi­ness (and if its not, it prob­a­bly should be), you absolute­ly want to be indexed.

If you use background-image you miss out on this, nor can you have an alt="" descrip­tion of the image to give Google a descrip­tion & con­text for the image.

LINK 2. BAD FOR ACCESSIBILITY
The CSS background-image prop­er­ty is not good for acces­si­bil­i­ty. Screen read­ers will ignore background-image entirely.

Have a look at the Acces­si­bil­i­ty Con­cerns:

Browsers do not provide any special information on background images to assistive technology. This is important primarily for screen readers, as a screen reader will not announce its presence and therefore convey nothing to its users.
Even if screen read­ers did­n’t com­plete­ly ignore images, there’s no alt="" text that can give a use­ful descrip­tion of the image or con­text for it.

Pro Tip: If you actu­al­ly want a screen read­er to skip an image (maybe it’s just a design ele­ment), just have an emp­ty alt tag, e.g.: alt="" (the use of the role="presentation" prop­er­ty is not opti­mal, as it vio­lates the first rule of ARIA).

LINK 3. BAD FOR PERFORMANCE
How could the background-image prop­er­ty pos­si­bly be bad for per­for­mance? Because typ­i­cal­ly just one image is used for the background-image prop­er­ty regard­less of the device screen width or resolution.

What we ide­al­ly want is to be able to use respon­sive images so that the brows­er can load a dif­fer­ent sized image depend­ing on the device’s screen width or resolution.

This makes a mas­sive dif­fer­ence on mobile device or low band­width con­nec­tions, as well as on the pro­cess­ing pow­er need­ed to scale images. Check out the Post-Mortem: Applied Image Opti­miza­tion arti­cle for details.

On a typical site, images will by far be the largest chunk of the data sent. We should optimize them.
While you can do this via @media queries with CSS background-image it ends up get­ting real­ly sticky if you need to change the image or rename it or ver­sion it.

The rea­son it gets sticky is that the image is embed­ded in your CSS, which is typ­i­cal­ly built by a build process that is not in the clien­t’s control.

So because it’s hard, peo­ple usu­al­ly just punt on it and have one ver­sion of the image for all device screen sizes and resolutions.

While there are JavaScript libraries such bgset to help out, why add yet more JavaScript to solve a prob­lem that has so many oth­er downsides?

Add in the desire to use next-gen­er­a­tion forms like webp as a pro­gres­sive enhance­ment, and things get real­ly icky with background-image.

Your browser can’t start downloading an image until it has downloaded & parsed the CSS
Anoth­er per­for­mance ben­e­fit is that if your images are embed­ded in your CSS via background-image, the brows­er has to down­load and parse your CSS before it can request the images.

This means your hero image is going to be low­er down the down­load queue, and slow­er to load. If you do your images via HTML ele­ments instead, the brows­er can request them much faster, some­times even before the stylesheets start downloading.

LINK 4. BAD FOR CMS’S & CDN’S
The CSS background-image prop­er­ty is also not great when it comes to Con­tent Man­age­ment Sys­tems and Con­tent Dis­tri­b­u­tion Net­works (CDNs).

Let’s say that I have a hero image on my web­site that I imple­ment­ed as a <div> with the image set via background-image and then my web­site takes off!

Peo­ple from all over the world are hit­ting my site, and I want to use a CDN to deliv­er these resources to them optimally.

If I have the image URL embed­ded in my CSS, I’m going to have to do a glob­al search and replace for all of my images to switch over their URLs from local URLs to my CDN URLs.

If you pref­aced all of your image URLs with CSS vari­ables, good for you! But most peo­ple don’t, and hav­ing to build and deploy a new CSS asset bun­dle when­ev­er you want to change your images seems… exces­sive. But it’s either that, or dynam­i­cal­ly gen­er­at­ing inline CSS… but let’s not go there.

It’s be much eas­i­er to just let your CMS han­dle the URL pre­fix, but that can get gross if you’re doing it via CSS background-image, neces­si­tat­ing the gen­er­a­tion of inline styles and oth­er shenanigans.

Putting image URLs in CSS always feels like ​“hard­cod­ing it” to me now.

LINK SO WHEN IS IT GOOD?
So we’ve been talk­ing all about the down­sides, but when is it actu­al­ly good to use the CSS background-image property?

 Good boy doggo
It may seem bla­tant­ly obvi­ous, but if you have the case where you actu­al­ly have an image as a pure­ly dec­o­ra­tive image back­ground… that’s when to use background-image!

It’s good to use background-image when you have an actual background image
If you are doing that, con­sid­er using image-set() along with background-image to mit­i­gate the per­for­mance issues men­tioned above.

It’s not that the prop­er­ty is inher­ent­ly bad, it’s that it has been shoe­horned into doing things it prob­a­bly isn’t the best choice for.

LINK USE <PICTURE> INSTEAD!
I think a large rea­son peo­ple use background-image is because of the CSS background-size: cover; prop­er­ty. It lets you make sure that an image ​“cov­ers” the thing its attached to in an aes­thet­i­cal­ly pleas­ing way. 

The object-fit property is here to save the day!
If you use the seman­tic HTML ele­ment <picture> with the object-fit prop­er­ty you get the same ben­e­fits of background-size: cover; but you also get:

SEO-friend­ly images
Acces­si­bil­i­ty friend­ly images (just use the alt="" property)
Works great with CMS-gen­er­at­ed image links & CDNs
Works great with src­set for opti­mized images
Can use <source> for next-gen­er­a­tion image for­mats like .webp
It’s real­ly not hard to do, and the ben­e­fits are numer­ous! The object-fit prop­er­ty tells the image or video how it should fit into the par­ent con­tain­er. You’d just change this:

```html
<div style="background-image: url('/some/man-with-a-dog.jpg');
            background-size: cover;">
</div>
```
 The old way
To this:
```html
<picture>
  <img src="/some/man-with-a-dog.jpg"
       alt="Man with a dog"
       style="object-fit: cover;"
  />
</picture>
```
 The new way
Obvi­ous­ly in both cas­es, you prob­a­bly would­n’t be using inline styles, but hope­ful­ly you can see how much more flex­i­ble of an approach we have if we ditch the background-image prop­er­ty and use actu­al seman­tic HTML ele­ments like <picture> and <img>.

We can then eas­i­ly add respon­sive­ly sized images via srcset. We can also eas­i­ly add a next gen­er­a­tion image for­mats like .webp for the brows­er to choose from via <source> as well.

Then we also get to take advan­tage of things like native image lazy load­ing in Chrome and oth­er browsers as it gets implemented.

Here’s what an final block of code that does all the things might look like:
```html
<picture>
    <source
        srcset="/some/_1170x658_crop_center-center/man-with-a-dog.webp 1170w,
                /some/_970x545_crop_center-center/man-with-a-dog.webp 970w,
                /some/_750x562_crop_center-center/man-with-a-dog.webp 750w,
                /some/_320x240_crop_center-center/man-with-a-dog.webp 320w"
        sizes="100vw"
        type="image/webp"
    />
    <source
        srcset="/some/_1170x658_crop_center-center/man-with-a-dog.jpg 1170w,
                /some/_970x545_crop_center-center/man-with-a-dog.jpg 970w,
                /some/_750x562_crop_center-center/man-with-a-dog.jpg 750w,
                /some/_320x240_crop_center-center/man-with-a-dog.jpg 320w"
        sizes="100vw"
    />
    <img
        src="/some/man-with-a-dog-placeholder.jpg"
        alt="Man with a dog"
        style="object-fit: cover;"
        loading="lazy"
    />
</picture>
```
 The final way
This gives us

lazy load­ing if the brows­er sup­ports it (oth­er­wise it does nothing)
webp if the brows­er sup­ports it (oth­er­wise it does nothing)
a srcset of opti­mized respon­sive images that the brows­er can choose from.
SEO friend­ly, pro­gres­sive enhance­ment, acces­si­bil­i­ty and per­for­mance all rolled up into one nice tight lit­tle bundle.

 Tight little bundle seo performance accesibility
Boom. 💥

And all of the images can be com­ing auto­mat­i­cal­ly sized from our CMS via tools like Ima­geOp­ti­mize, mak­ing it easy to point to a CDN or Ama­zon S3. Final­ly, you can do some very cre­ative crop­ping of images with object-fit.

LINK OUT WITH THE OLD, IN WITH THE NEW
Brows­er sup­port for <picture> and srcset is pret­ty fan­tas­tic (with a poly­fill if you need it), and so is brows­er sup­port for object-fit (also with a poly­fill if you need it). Anoth­er nice pat­tern to use is this via poly​fill​.io

So the time is now to ditch background-image from your toolset (except in the rare cas­es where you actu­al­ly want a back­ground image).

 Out with the old in with the new
Code using best prac­tices for the over­whelm­ing major­i­ty now, then use pro­gres­sive enhance­ment and a poly­fill when nec­es­sary. Don’t be shack­led to the past for what amounts to a very small per­cent­age of your visitors.

Don’t use a content image as decoration; use semantic HTML content image elements
I think you’ll find that you end up build­ing web­sites that are more SEO friend­ly, more acces­si­ble, and more per­for­mant… as well as just feel­ing more sat­is­fy­ing to write in a seman­tic HTML way.

The CSS background-image prop­er­ty was nev­er meant to be used for your hero images or UI ele­ments. So now that bet­ter ways to do it are here, let’s use ​‘em.

If this is got­ten you excit­ed about opti­miz­ing images, check out the excel­lent images.guide for a com­pre­hen­sive write up.

Enjoy your object-fit!