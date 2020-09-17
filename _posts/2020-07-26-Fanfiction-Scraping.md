---
layout: post
title: Fanfiction Scraping
---

What makes web fiction successful?

This is actually a very complex question, with many factors. Web fiction is different from normal print books because of how informal, saturated, and low-effort it is to get into. People can easily read many versions of the same story, the same premise, with psuedo-plagarist "homages" running rampant. 

Furthermore, popularity is not a sign of quality. Many will attest to that, and the idea that much of what's popular is indeed worse fiction in most ways.

In fact, the market is so saturated, that most readers literally do not have the time to shovel through the massive amount of web fiction on their own to find something they'd like. People rely on referrals, recommendations, and other "content filters"/"distinct characteristics" to narrow down the selection to a point where they can sift through whats left.

Most readers just want to read something enjoyable for them, not the best story in the whole bunch, so they're fine with eliminating great works that they'd love as long as in the end there is something interesting to read.

This implies that there could be some strategy that makes certain web fiction more popular than others. That there's some sort of attribute that's used to filter more, or something that catches the eyes of readers.

But with how varied and disparate web fiction can be, how can we measure what makes a work of web fiction popular?

This is where Big Data comes in.

We can try to scrape all the statistics, metrics, and characteristics of comparable web fiction, then create a logistic regression model that tries to best match the results.

But in order for this to work, we need a scrapeable database of comparable fiction, and a way to quantify the "engagement" or "popularity" of the fiction.

This is why I turned to fanfiction.net, specifically, the Harry Potter section of it.

Fanfiction.net is easily scrapeable with BeautifulSoup and some choice Regexs, making it a prime target. Its Harry Potter is one of the largest sub-categories of fiction on the site, and it has a very engaged fanbase that has existed before fanfiction.net became a thing. This means that these stories are indeed comparable to each other in terms of popularity metrics, and the data is somewhat continuous from the start of the site. 

However, Fanfiction.net obfuscates and hides the page, chapter, and story viewcounts to the public, only allowing authors to see them. And while I could contact thousands of individual authors and hope they provided their metrics, discussions with a few well known authors gave me indications that I could approximate user interaction with three pieces of information Fanfiction.net provides publically: Reviews, Follows, and Favorites.

This means that an interaction metric using data from these three fields could approximate popularity/user engagement.

Working closely with notable fanfiction authors, we hammered out what each metric meant to the authors:

Favorites were seen as low effort **public** declarations of admiration.
Follows show what people actively follow, but are private and therefore easier to obtain than favorites.
Reviews are the basic public feedback on the site, prone to noise, but because it’s an effort gate, it shows user investment and the likelihood of “spreading the word”. However, it is possible to review stories more than once.

Across the authors I interviewed, the most representative equation was:

```
2*Follows + 3*Favorites + 5*Reviews
```
It's not perfect. I personally think Reviews were a bit more heavily weighted, as one person could review a story multiple times. But the scores from this equation lined up with the relative viewcounts the best across all the authors' stories.

Now armed with this output metric, I set up the beautiful soup web scraping code to scrape for everything on the Harry Potter category index pages. Everything, from the Title, number of chapters, description, and dates published/updated all could be valuable metrics, so I wanted to load up my dataset with as much as I could fit in there.

But scraping the entire Harry Potter category would be over 780,000 pieces of fiction, too much to handle at this point. So I limited it to fiction 60 thousand words and above, so that it is more likely that the analysis is being done on serial fiction, ones that have multiple updates and chapters which can draw a dedicated following. Furthermore, in order to properly parse the descriptions/blurbs of these fics, they should all be in the same language, English.

These are all modifiers that can be put at the end of the URL of the index page. In fact, the actual code is

>"?&srt=2&lan=1&r=10&len=60&p="

This cut the total number of fics to be scraped down to just above 20,000 different titles.

This meant that the main scraping function looked like:
```
def main():
    # we're only going to look at harry potter fanfics
    base_url = "https://www.fanfiction.net/book/Harry-Potter"
    # this gets appended in order to
    page_suffix = "?&srt=2&lan=1&r=10&len=60&p="

    # 3 seconds seems reasonable for a human to quickly scroll through a page
    rate_limit = 2

    metadata_list = {}
    full_list = {}
    first_page = 1
    last_page = 795
    pages = range(last_page, first_page - 1, -1)
    for page in pages:
        # log
        print("Scraping page {0}".format(page))

        # build the url
        url = '{0}/{1}{2}'.format(base_url, page_suffix, str(page))

        # and let's go!
        metadata_list= scrape_all_stories_on_page(url)
        full_list.update(metadata_list)

        # sleep to be good about terms of service
        time.sleep(rate_limit)

    
    with open(filename, 'w') as outfile:
        json.dump(full_list, outfile)
```

To handle rate limiting the webscraping, the Scraper class had to be modified as well:

```
class Scraper:
    
    def __init__(self):
        self.base_url = 'http://fanfiction.net/'
        self.rate_limit = 5
        self.parser = "html.parser"
```
