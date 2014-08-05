# thumbsup

[![NPM](http://img.shields.io/npm/v/thumbsup.svg?style=flat)](https://npmjs.org/package/thumbsup)
[![License](http://img.shields.io/npm/l/thumbsup.svg?style=flat)](https://github.com/rprieto/thumbsup)

[![Build Status](http://img.shields.io/travis/rprieto/thumbsup.svg?style=flat)](http://travis-ci.org/rprieto/thumbsup)
[![Dependencies](http://img.shields.io/david/rprieto/thumbsup.svg?style=flat)](https://david-dm.org/rprieto/thumbsup)
[![Dev dependencies](http://img.shields.io/david/dev/rprieto/thumbsup.svg?style=flat)](https://david-dm.org/rprieto/thumbsup)


Build static HTML galleries from local photos & videos.

- thumbnails & multiple resolutions for fast previews
- only rebuilds changed files: it's fast!
- supports custom styles
- uses relative paths so you can deploy the pages anywhere
- works great with Amazon S3 for static hosting

[View sample website](http://rprieto.github.io/thumbsup)

[![screenshot](https://raw.github.com/rprieto/thumbsup/master/screenshot.jpg)](http://rprieto.github.io/thumbsup)

## Requirements

- [Node.js](http://nodejs.org/): `brew install Node`
- [GraphicsMagick](http://www.graphicsmagick.org/): `brew install graphicsmagick`
- [FFmpeg](http://www.ffmpeg.org/): `brew install ffmpeg`

## Input

Any folder with photos and videos. `thumbsup` currently supports 1 level of subfolders, where they each become a gallery.

```
input
  |
  |__ paris
  |    |__ img001.jpg
  |    |__ img002.jpg
  |
  |__ sydney
       |__ vid001.mp4
       |__ img003.png
```

## Generating the galleries

Install the module globally, which puts the binary in your path:

```
$ npm install -g thumbsup
```

```
$ thumbsup [args]

  Update metadata     [===================] 6/6 files
  Original media      [===================] 6/6 files
  Photos (large)      [===================] 5/5 files
  Photos (thumbs)     [===================] 5/5 files
  Videos (web)        [===================] 1/1 files
  Videos (poster)     [===================] 1/1 files
  Videos (thumbs)     [===================] 1/1 files
  Static website      [===================] done
  
  Gallery generated successfully
```

The following args are required:

- `--input <path>` path to the folder with photos / videos
- `--output <path>` target output folder

And you can optionally specify:

- `--title [text]` website title (default "Photo gallery")
- `--thumb-size [pixels]` thumbnail image size (default 120)
- `--large-size [pixels]` fullscreen image size (default 1000)
- `--sort-folders [name|date]` how to sort the folders/galleries
- `--css [file]` styles to be applied on top of the default theme (default none)

*Note:* all paths are relative to the current working directory.
For example:

```bash
thumbsup --input "/media/photos" --output "./website" --title "My holidays" --thumb-size 200 --large-size 1500 --sort-folders date --css "./custom.css"
```

You can also save all your arguments to a `JSON` file:

```bash
thumbsup --config config.json
```

**config.json**

```js
{
  "input": "/media/output",
  "output": "./website",
  "title": "My holiday",
  "thumb-size": 200,
  "large-size": 1500,
  "sort-folders": "date",
  "css": "./custom.css"
}
```

## Website structure

The generated static website has the following structure:

```
website
  |__ index.html
  |__ sydney.html
  |__ paris.html
  |__ public
  |__ media
  |    |__ original
  |    |__ large
  |    |__ thumbs
```

## Deployment

The simplest is to deploy the media and generated pages to S3 buckets on AWS using the [AWS CLI tools](http://aws.amazon.com/cli/).

- `aws s3 sync ./generated/website s3://my.website.bucket --delete`

## Password protection

Amazon S3 buckets do not offer any type of authentication. However you can choose to deploy to another web server that offers password protection, such as HTTP Basic Auth.

An alternative is to deploy the galleries to UUID-based locations, like Dropbox shared galleries.

## Dev notes

To create the sample gallery locally:

```
npm run clean      # clean the output
npm run example    # build the gallery
npm run open       # open it in the browser
```
