# tarkov-items-app
Tarkov items app, showing flea market prices and any barter trades available

Escape of Tarkov is a video game that has a heavy items-focused economy. This web app shows the current flea market price and any barter trades available for the particular item. 

The flea market price is web-scraped off of tarkov-market.com using Selenium for the infinite scroll and BS4 for actual parsing. The code is in newscraper.py. The app will check to see if a current date flea market price file is included in the directory; if it is not, the scraper will run before the app is launched. Ideally, the app would be set up to restart every midnight (as tarkov-market.com uses daily averages), so that the flea market prices are updated daily. The older scraper used Selenium for both infinite scroll and parsing, and worked much slower. It's included in oldscraper.py as a proof of concept.

The barter trades info was scraped from the wiki using a Scrapy tool (not included in this repository); the output is scrapyoutput.csv. This info was somewhat formatted using Cleaner.ipynb, and then formatted even more in Excel (the data was extremely dirty, with a lack of standardization. The final barter trades info is in bartertrades.xlsx. It is expected that this info will not change regularly; thus, there is no need to re-run the barter scraper every initialization.

The web app itself is a simple interface that uses a single text input and a submit button to show info from the two tables. The formulas used are in Parser.py; they are lazy, and use a simple "X in Y" formula to find a close match. Auto-complete was considered, but honestly this works just as well, and auto-complete is kind of a heavy solution for this.

The web app can be hosted on Heroku! The current link I am using is http://tarkov-items.herokuapp.com/. There is also a procfile so anyone can host it themselves.

Room to grow:
* The flea market scraper is kind of wonky. The infinite scroll uses a while loop that relies on scrolling down, waiting 4 seconds while the next page loads, and then scrolling down more. If the next section of the page is not loaded within 4 seconds, it breaks, and may prematurely stop the scraping. It's possible to increase the time limit (which will make the scraper run much longer), which will make it "safer" (in that it is much more likely to load and scrape all the info). Thankfully, this runs once a day (ideally) so it's not much of an issue. In the future, thinking up a better way to solve the infinite scroll would be ideal.
* Adding a price calculation for the barter items (based off the flea market) would be awesome to see where money could be made (ex. item X costs 5k roubles, and can be traded for item Y, which costs 10k roubles. 5k rouble profit!), but there is a LOT of name mismatches between the market item and barter item sheets. Thinking up a way to quickly make the names equivalent would be nice. It's possible to do it by hand, but it would take a really, really long time. Might need to see if there's some NLP thing I can do.
