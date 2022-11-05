---
custom_order: 1
layout: post
published: true
tags:
- antitrust
- data privacy
- machine learning
- training data
title: How to prevent price fixing through data privacy
---
Every time you go to the airport, call your friends, and purchase groceries there are multiple
businesses collecting that information, many more selling that information, and even still more
trying to influence you with that information. I don't think that surprises most
digital natives or millennials, but it does disappoint us, especially when it makes life
difficult or expensive for a negligible amount of business profit.

### Engaging an audience
10 years ago, there were just a few technical folks who realized they could provide a more customized and engaging
experience to their audience by collecting information to make informed guesses about their audience.

Yes, I want to absolutely see more cute cat pictures, send me more. Yes, I want to see more trail running shoes
and thank you for assuming that based on my purchase of trail running shirts.

I focused in on database systems during the final year of my Computer Science coursework and was offered a position
by my professor, [Jaideep Srivastava](https://www.crunchbase.com/person/jaideep-srivastava), at his latest startup
in the space. In our case, we knew there were social influencers, but no one had quantified their total value yet;
well we did and could tell a business exactly how much @social_user contributed to the platform in ad revenue,
user retention (my friends are on so I stay on), and sales.
There was never really a discussion of whether selling data was right or wrong,
because the data market did not exist; we were making the market in social network monetization!

Yes, tell me about how my gaming partners are trying out a new game, I'd be interested to buy and play it too.
Yes, tell me about how @social_user is posting about Brand X, I'm interested in that and Brand Y.

[If the service is free, you're the product being sold!](https://www.forbes.com/sites/marketshare/2012/03/05/if-youre-not-paying-for-it-you-become-the-product/)

### Guarding health data
Fast forward through many years of building data pipelines to figure out what motivates people, and
now I sell data authorization and security software. When you go into the doctor's office
and hand them all of your personal information, the software I sell allows bureaucrats to make sure the other
20,000 bureaucrats of the health care provider do not see your name, weight, birthdate, address, social,
preexisting conditions, and email ... unless you (the data subject, to use a technical phrase) consent.
Or if the marketing bureaucrats want to sell you more diagnostic services
or sexual pills they can only see your first name and email.

The elderly legislatures are starting to catch up and mandate these kinds of data protections,
but in many parts of the world these don't exist yet. I chat with companies which produce oil, bread,
medical implants, heavy machinery, and clothing. They all know this is coming, but until there is a big
enough fine to damage their quarterly earnings, there is no reason to do anything.
_It just doesn't make business sense to pay for something you're getting for free._
If you thought [energy](https://www.imf.org/en/Topics/climate-change/energy-subsidies) and
[farming](https://www.nal.usda.gov/legacy/topics/agricultural-subsidies) were subsidized,
we are all subsidizing businesses with free access to our data to increase their profit margins.

### Liquid gold
Let's talk about another application of this. Oil wells. We've had them for 200 years, not very exciting, until
you think about charging more money for oil products. Most oil companies now automatically and meticulously track
information on the amount of oil being pumped out of the ground by their oil wells. To the oil company and to most
energy regulators this is classified as very sensitive information.

If capitalism is working correctly, and companies are competing to win business, and Oil Producer 2 finds
out that Oil Producer 1 is pumping slightly less than average ... #2 could pump slightly more, and charge slightly less
for oil products, making Oil Producer 1 unable to compete and potentially go out of business or be bought out.
With no more #1, #2 has no incentive to keep prices competitive.

If capitalism is not working correctly, Oil Producer 1 and 2 could share pump rate information and agree to
keep supply of oil lower that normal.

Either way the oil producers get to make more money on oil products.

### Profits better than housing people
Now let's take that price-fixing example even further.
The first place I moved into in the Denver metro was ~730 ft^2 with a base rent of $1050/month in March 2013.
I have moved around to Wheat Ridge, Boulder, and Golden, but I was still rather surprised when I looked today
and that same unit is $1920/month in November 2022.

Not surprising, but is disappointing in a way,
I have spent $186,348 just on base rent:
+ 2013 South Denver $1050/month * 12
+ 2014 South Denver $1114/month * 12
+ 2015 Wheat Ridge $1355/month * 12
+ 2016 Wheat Ridge $1415/month * 12
+ 2017 Boulder $1735/month * 12
+ 2018 Golden $1730/month * 12
+ 2019 Golden $1745/month * 12
+ 2020 Golden $1785/month * 12
+ 2021 Golden $1800/month * 12
+ 2022 Golden $2000/month * 12

Interestingly, you'll notice two time periods of price jumps in the data: 2015-2017 and 2021-2022.
These time periods coincided with periods of economic recovery and
[higher inflation](https://www.macrotrends.net/countries/USA/united-states/inflation-rate-cpi).
I'm of the mind that business profits lead to inflation in modern economies;
businesses believe they can charge more for goods, they do, and the price index goes up
... especially during these recovery periods.

Say you had enough rental price information, you could make a pretty good guess what your competitors are
charging for rent, and what people are willing to pay. Now say you wanted that guess instead to be certain.
You would need the exact price per month of every unit different apartment managers are leasing.

If capitalism is working, there is no way Manager 1 would make this information public, Managers 2 and 3 could
charge slightly less than Manager 1 and put Manager 1 out of business.

If capitalism is not working, Managers 1, 2, and 3 will form an agreement to share data to predict with
certainty what the optimal price to charge for rent is to make the most profit possible.
Apartment managers in this scenario are not interested in leasing out as many units as possible,
but making the most profit. To be fair, in capitalism, continuous growth and maximizing profit are supposed
to be the driving force, not housing people.

Fortunately, in some countries, there are laws which hold the well-being of people above profits.
In North America, we call these antitrust laws.
[Two things need to be proved](https://www.bonalaw.com/insights/legal-resources/the-elements-of-antitrust-injury-a-two-prong-test)
to win these kinds of court cases:
1. A higher price for a product than would have prevailed if there had been no price-fixing agreement
2. A link between alleged anticompetitive conduct and injuries must be proved

Which is precisely what the
[class action lawsuit against several apartment managers and their shared technology provider](https://news.bloomberglaw.com/esg/realpage-major-landlords-face-antitrust-lawsuit-over-rent-spike)
must prove, that RealPage enabled apartment managers to set higher prices, and that renters
were affected negatively by these higher rents.

### Prevention
From a societal standpoint, stricter data privacy and sharing laws would prevent businesses from wandering into these
legal grey areas. We could choose to limit businesses from using our data free of charge and level the
playing field for all businesses regardless of technical acumen.

From a technical angle, there are solutions like the one I sell which allow
ethical data stewards to set limits on how data is used; yes I opted to be allowed to be contacted for marketing
purposes, and so the marketing folks are allowed to just see my first name and email, but not my credit card number.