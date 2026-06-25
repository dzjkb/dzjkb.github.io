https://dziennikustaw.gov.pl/DU
jezu ten język - LLMy? lol
serio mogło by mieć sens
- no infinite scrolling
- no tags/metadata of any sorts attached to the entries
https://isap.sejm.gov.pl ju lepiej
ale wciąż niezbyt dla laika, bez tagów ani nic

what's the sota of looking through this though, any lawyers?
https://www.sp-7.pl/dziennik-ustaw-edukacja/

https://www.sejm.gov.pl/ - there's a calendar, voting records, schedule, news
- website from fucking 2002 least accessible shit, starting from even just the font size
- the bills themselves are PDFs, no tags, summaries, easy ways to view
    - I'd imagine software to effectively search through bills like this could be a big business, something like this has to exist already
### info that should be more available
- sejm calendar
- current issues
    - like ones that are going to be voted on next?
- votings with exact summaries of who voted how
    - oh shit there's a list of all votings of the current term
# git versioned law
oh shit what a great idea
https://github.com/legalize-dev/legalize-es
how hard would parsing stuff from the sejm website be to recreate this?
https://github.com/legalize-dev/legalize
https://github.com/legalize-dev/legalize-pipeline
https://enriquelopez.eu/blog/en/legalize-gstack-hacker-news
https://github.com/legalize-dev/legalize-pipeline/blob/main/CONTRIBUTING.md

some valid insights from the [HN thread](https://news.ycombinator.com/item?id=47553798)
there are tools like this, just commercial, and they also include court rulings which are an important part
if this was supposed to be targeted towards average people - what would actually make them use this? what would they use it for?

anyway
there's the whole pipeline ready with a [guide on how to add a country](https://github.com/legalize-dev/legalize-pipeline/blob/main/CONTRIBUTING.md)
basically defining a couple of interfaces - how to download from source and parse into the generic model
and voila