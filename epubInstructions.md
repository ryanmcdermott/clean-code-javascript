# Creating an ebook of this repo

You can generate an epub version of this repo's markdown file by using Pandoc. You can then transfer that file to your favorite e-reader (e.g. an iPad).

## Installing and generating


### Install pandoc

See [the pandoc site](http://pandoc.org/installing.html) for full instructions. On OS X, it's as simple as: 

```shell
brew update
brew install pandoc
```

### Generate the epub file

```shell
pandoc README.md -o cleancode.epub --epub-stylesheet epub.css
```

On OS X Yosemite & newer, you can view the ebook using iBooks on your Mac with:

```shell
open cleancode.epub
```

Steps for transferring to your device will depend on your e-reader and platform.


### Customizing / Options

Feel free to edit the epub.css file to fit your reading / e-reader preferences. It's just standard CSS. The current values were subjectively chosen for what looks okay on my iPad. 

See the pandoc docs if you want to use custom fonts, a custom cover image, or generate a PDF file. You can use [KindleGen](https://www.amazon.com/gp/feature.html?docId=1000765211) to generate the mobi file needed for a Kindle.

