# manga-dl
Need a tool to fetch those manga you love quickly, you hate advertisements and don't like to read online. Don't worry, _manga-dl_ gets you covered. _manga-dl_ will get your favorite manga from [Kissmanga](https://kissmanga.org/) straight into your computer, and you can read them on whatever device you have as long as it's **EPUB** or **PDF** compatible.

Simply put, with a single command _manga-dl_ downloads your manga and automatically bundles it into a portable EPUB/PDF file that you can read whenever you want without the internet.

Here is how the downloaded manga look like when you import them into [_Lithium_](https://play.google.org/store/apps/details?id=com.faultexception.reader) app. This is just for the demo, any app that supports reading EPUB/PDF file works just fine

![Created EPUB files on mobile](screens-demo.png)

**Before using this script, read [TERMS OF USING](terms-of-using.md).**

## Features

- [x] _youtube-dl_-like command line interface.
- [x] Chapter-mark book bundling.
- [x] Multithreading image downloading.
- [x] EPUB and PDF support.
- [x] Dockerized execution. 
- [x] New chapters resuming.
- [x] Other sites support i.e. MangaRawr, Nettruyen.
- [ ] Compressed PDF bundling.

## Setup

_manga-dl_ is a command line tool written in Java, so of course you need Java to run it. Grab one [here](https://www.oracle.com/java/technologies/javase-downloads.html).
You also need [Docker](https://www.docker.com/products/docker-desktop) to download manga (technically it's for extracting manga frames urls, the actual download is handled by your host machine).

1. Clone the repo
```
git clone https://github.com/giahuy2201/manga-dl.git
```
2. Navigate to the project folder
```
cd manga-dl
```
3. Build the project using `./gradlew`
```
./gradlew clean jar
```
You now have successfully built your `manga-dl-2.1.jar`.

I'm not sure if this works for your OS, but try executing `make install` in the project directory to install _manga-dl_ in your PATH. If it succeeds you can then run _manga-dl_ as a cli tool like _youtube-dl_. You can remove _manga-dl_ using this command `rm  /usr/local/bin/manga-dl`

## Usage

Before continue, make sure you have started Docker daemon. If you are on mac or windows, just open up your _Docker Desktop_.
You can now start executing your command by prompting it with `java -jar build/libs/manga-dl-2.1.jar`. Here is the help message.
```
Usage: manga-dl [-hlv] [-f=<format>] [URL] [COMMAND]
      [URL]               Link to manga.
  -f, --format=<format>   Output format of manga (i.e. epub, pdf).
  -h, --help              Show this help message.
  -l, --log               Save log file.
  -v, --verbose           Enable console log.
Commands:
  download  Only download image files (.png).
  bundle    Pack image files (.png) into an EPUB file
Supported sites: Kissmanga, MangaRawr, Nettruyen
```
> Note: add option `-t=` followed by number of threads (no space) that your computer can handle optimally to adjust downloading performance. By default, this option is set to 10 which works perfectly on my setup.

### Examples

Download and pack manga
```
java -jar build/libs/manga-dl-2.1.jar https://kissmanga.org/manga/bv917658
```
![Download and pack manga command output](screens-usage.png)

Download and save image files
```
java -jar build/libs/manga-dl-2.1.jar download https://kissmanga.org/manga/bv917658
```
Bundle images files into an EPUB file
```
java -jar build/libs/manga-dl-2.1.jar bundle 'Tensura Nikki Tensei Shitara Slime Datta Ken'
```
Bundle images files into an PDF file
```
java -jar build/libs/manga-dl-2.1.jar bundle -f=pdf 'Tensura Nikki Tensei Shitara Slime Datta Ken'
```

_manga-dl_  first downloads and stores your manga frames int a folder under the manga's title in `.png` format. It then collects all the frames and bundle them into an EPUB file. In addition, it saves your manga's metadata as well as all the frames' urls in `manga.xml` located at the manga directory for later bundling options. The EPUB file of your manga is generated right at the project folder, the manga folder can be deleted safely afterward.

### Issues

- You may encounter OutOfMemory error when bundling large PDF file. This is because PDFBox which is the library _manga-dl_ uses to bundle PDF file saves loaded images as objects in memory instead of references. To address this issue, you can add `-Xmx` followed by the amount of RAM for the program (i.e. `-Xmx4g`, `-Xmx6g`,`-Xmx8g`, etc.) if you run the `.jar` file. In case you had used `make install` to install _manga-dl_, you can increase the memory for the program by replacing `-Xmx4g` with a higher one and run `make install` again.

## How it works

_manga-dl_ uses Selenium in a Docker container to control Chrome browser to access your manga url and collect all the manga's frames. The manga frames' urls as well as other metadata about the manga is saved. After that _manga-dl_ downloader goes through all the frames' urls and retrieve them. Finally, _manga-dl_ bundler kicks in and create an EPUB file.

## Disclaimer

Manga piracy is an act of stealing which is _illegal_, using _manga-dl_ for non-personal purposes is discouraged. _manga-dl_ is made for the sole purpose of education, its creator will NOT be responsible for any consequences.
