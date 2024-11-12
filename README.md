# so: you want to host a podcast feed...

hosting a podcast feed is really simple but has a lot of hidden complexities: especially for someone that doesn't have experience hosting things on the internet. hopefully, this repository and README will guide someone with minimal technical experience to host their own feed.

to see the fully hosted rss feed, [click here](https://anna-murphy.github.io/diy-podcast-feed/example-feed.xml)!

## technical basics: what actually *is* a podcast feed?

at its core, a podcast feed is a text file that contains links to a series of audio files (with a sprinkling of metadata). but, to make sure that all podcatchers can read the same feeds, they have to be formatted in a certain way: [XML](https://en.wikipedia.org/wiki/XML).

all you really need to know about xml for the purposes of this project is that it's a way of containing information in text files by using "tags". a tag is a bit of text with triangle brackets on either side that will look really familiar if you've ever opened the inspector menu on a web page before (html and xml use a similar style of markup). for example, `<div>`, `<p>`, and `<a>` in html land, and `<rss>`, `<channel>`, and `<item>` in podcast xml land. for each of these tags, there needs to be a corresponding closing tag (`</rss>`, `</chanel>`, and `</item>` respectively). 

but tags by themselves don't contain a lot of data. so, you have two methods you can use to store actual data in an xml file: you can nest tags, and you can give tags "attributes". 

- "nesting" tags means just putting tags inside of each other. for example, in html land, a `<ul>` (a list) contains many `<li>`'s (list items). and, in podcast land, a `<channel>` (a podcast feed) contains many `<item>`'s (podcast episodes). but, contrary to both of these examples, there can be many kinds of sub-tags nested inside each tag. tags that don't have any nested tags can be "self closing", where there is no end tag and and the start tag ends with a `/>` (eg: `<div />`).
- "attributes" are values applied to the start tag itself. for example `<img src="example.jpg" />` in html land and `<itunes:image href="..." />` in podcast land. 

### okay, but actually tho

a podcast feed is a `<rss>` tag containing one or more `<channel>` tags, which contains multiple `<item>` tags (which define each episode in the feed).

the `<channel>` tag has several other sub-tags that define required information for the feed itself:

- `<title>`: the title of the feed.
- `<description>`: a description of the kind of content that you have in your feed.
- `<itunes:summary>`: a description of your feed as per `<description>`, but both are required.
- `<link>`: a link to a website for your podcast.
- `<docs>`: a link to your webside, as per `<link>`. these can be different, but don't need to be.
- `<language>`: a two letter language code to communiate what language your podcast is in. there's a list of these somewhere but i'll have to find them somewhere. this is written in english, the code would be `en` for this article.
- `<itunes:author>`: the author of the podcast / the feed.
- `<itunes:image />`: the display image for your feed (linked with an `href` attribute).
- `<itunes:category />`: a list of itunes categories that help categorize the genre of your show. the category is give via attribute (`text="..."`), but can be nested with sub categories. this also has a set list.
- `<itunes:owner>`: the owner name and email for podcast. put your name and email inside the `<itunes:name>` and `<itunes:email>` subtags.
- `<itunes:explicit>`: if you use (or don't care about) bad words :tm:,  put "true" here. otherwise, "false".
- `<itunes:type>`: either "episodic" or "serial". if the episodes are best listened to in a specific order, use "serial".
- `<itunes:new-feed-url>`: link to your new feed url if you change. 
- `<atom:link href="${channel.feedUrl}" rel="self" type="application/rss+xml"/>`: the link to the current feed. it get's pretty meta.
- `<podcast:locked>`: "yes" if the feed is no longer being added to. 

then, each `<channel>` can have any number of `<item>` tags. these describe each episode of your podcast, and should have the following fields. 

- `<title>`: title of the episode.
- `<link>`: a link to the audio file for that episode (more on hosting audio files later).
- `<guid isPermaLink="true">`: permalink to episode audio. yes, there's a lot of repeated information.
- `<description>`: description for the episode. these are your shownotes, and can take full html.
- `<pubDate>`: the date this episode was uploaded.
- `<itunes:duration>`: the duration of the episode in seconds. 
- `<itunes:explicit>`: "true" if you use Bad Words (as above).
- `<itunes:season>`: the number of the season this episode belongs to. this is just a numerical grouping, and doesn't actually change the episode at all.
- `<itunes:episode>`: the episode number of this episode in the current season. 
- `<itunes:episodeType>`: "full", "trailer", or "bonus" depending on the nature of the episode.
- `<enclosure />`: some technical details about the audio file, given in attributes.
  - `url`: same as the `<link>` above.
  - `length`: the number of bytes the file is. you should be able to see this in your file explorer, but *i think* it can be estimated.
  - type: the kind of file it is. for `.mp3` files, this will be `audio/mpeg`.

this is a lot of fields, and there's more information about what each of these mean in both of these links:
 - https://help.apple.com/itc/podcasts_connect/#/itcb54353390
 - https://github.com/Podcast-Standards-Project/PSP-1-Podcast-RSS-Specification?tab=readme-ov-file#channel-itunes-type

## well, know that i know what it is, how do i listen to it?

well, the actual xml file is just a text file. to actually have these show up in your podcatcher, you're going to need to get it online. there's a lot of different ways to get files online, but i'm going to outline how to do it using github pages. it's free, and if you're reading this, you're already on github.

### hosting an xml file on github

the first thing you're going to need to do is create your xml file. given the information above and the example file (link), update the file to contain information about your podcast.

then, make a github account and create a repository for this file. a repository is just a place to store code in an organized manner. for the purposes of this project, you shouldn't need to worry the complexities of github too much. 

> note: add more information aout this step eventually

once you have the repository create, you'll upload your xml file. navigate to the repository's page, click "Add File" and "Upload Files".

![a screenshot of the github UI showing the "add file" and "upload files" buttons](/images/00_upload_file.png)

if you have cover art for your podcast, you can upload it here as well.

then, set up github pages to host these files. from the repository page, navigate to the settings page.

![a screenshot of the github ui with the settings link highlighted](/images/01_github-main.png)

then, click "pages" on the left side of the window.

![a screenshot of the github repository settings page with the pages link highlighted](/images/02_github-settings-highlight-pages.png)

then, where the dropdown says "none" under the "branch" heading, change the branch to "main".

![the github pages settings page with the branch select dropdown highlighted](/images/03_github-settings-select-branch.png)
![the same page with the correct option highlighted](/images/04_github-pages-select-main.png)

this is cause github to create a website based on the contents of your repository. in other circumstances, you might want to have your code in one "branch" of the repository and the website in another. but for our purposes, we're going to only worry about one branch to make everything a little simpler.

give github a few minutes to build the page, and then you can navigate back to the "pages" settings in your repository. you should see a banner like this that will link you to a website:

![the github repo settings page with the deployed pages banner](/images/05_github_pages_deployed.png)

this banner will contain a link to the website that was created. click the link to go to that page. if you uploaded a `README.md` file, you should see it on this page. if not, it will (probably?) be blank. manually change the url in the search bar of your browser, and add the file name of the xml file you uploaded. this will be the url for your podcast feed. 

for this repository, github gave me this url: `https://anna-murphy.github.io/diy-podcast-feed/`. so, because the xml file is called `example-feed.xml`, my podcast feed is `https://anna-murphy.github.io/diy-podcast-feed/example-feed.xml`.

if you uploaded an image file to your repository, you should be able to access it in the same way (my `cover-image.jpg` is accessible at https://anna-murphy.github.io/diy-podcast-feed/cover-image.jpg).

with these two links in hand, go back to your xml file and set the links to the feed and your cover image appropriately. 