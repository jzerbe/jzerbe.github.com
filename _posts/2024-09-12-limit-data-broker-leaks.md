---
author: Jason Zerbe
custom_order: 2
layout: post
location: 32.894952, -96.968502
outcome: WORKED
published: true
tags:
- browser settings
- Privacy
- right to delete
- data brokers
title: Protect yourself from data broker leaks
---
__Result:__ It worked, and it stuck.
Two weeks after signing up for a data removal service, my information was gone from people search websites.
Two years later (October 2026), it is still gone.
The browser and email settings below keep new data from leaking out in the first place.

## The Problem
Every organization will leak your data eventually.
The profit motive can get in the way of security, and they may not pay enough to hire quality people.
So only give your data to services you absolutely need.
Remember: if the service is free, you and your data are the product being sold.

Then there are all the organizations you give data to without knowing it.
A [data broker](https://www.gartner.com/en/information-technology/glossary/data-broker) makes money
by collecting your data, and lots of other people's, then selling it.

Search for yourself on [TruePeopleSearch](https://www.truepeoplesearch.com/) and tell me you are okay with that information
being public. That is just the top. There is much more.

__Hypothesis:__ Paying a service to send deletion requests in bulk, plus locking down my browser and email,
gets me out of these databases and keeps me out.

## The Solution I Tried

### 1. Pay Someone to Send the Deletion Requests
Read [PCMag's review of data removal services](https://www.pcmag.com/picks/the-best-personal-data-removal-services)
and pick one that automates hundreds of deletion requests.
I went with [Incogni](https://incogni.com/). It has kept working for two years.

My Incogni referral link gets you a discount: <https://incogni.cello.so/iCje17eYe7z>

### 2. Lock Down the Browser

__Chrome:__

1. Open Settings.
   <img src="/assets/posts/chrome_settings.png" alt="Chrome menu with Settings highlighted" class="image fit" />
2. Open the "Privacy and security" tab.
   <img src="/assets/posts/chrome_privacy_and_security.png" alt="Chrome Privacy and security settings" class="image fit" />
3. Click "Delete browsing data", then "Delete data" to clear everything your browser stores
for websites, data brokers included. This logs you out of every website.
   <img src="/assets/posts/chrome_delete_data.png" alt="Chrome Delete browsing data dialog" class="image fit" />
4. Click "Third-party cookies", scroll down, and select "Block third-party cookies".
Google [dropped its plan to kill third-party cookies](https://en.wikipedia.org/wiki/Privacy_Sandbox) in 2025, so this stays your job.
   <img src="/assets/posts/chrome_third_party_cookies.png" alt="Chrome third-party cookie settings (2024 screenshot)" class="image fit" />
5. Open "Site settings", scroll down to "Embedded Content", and select "Sites are blocked ...".
   <img src="/assets/posts/chrome_embedded_content.png" alt="Chrome embedded content setting" class="image fit" />
6. Install [Privacy Badger](https://privacybadger.org/) from the Chrome Web Store.
Chrome does not send the Global Privacy Control signal on its own. Privacy Badger
[does, by default](https://privacybadger.org/).

The old "Ad privacy" page is going away as Google
[removes the Topics API](https://groups.google.com/a/chromium.org/g/blink-dev/c/_R85yctz4Rs/m/zp2f_i_qAgAJ).
If you still see it, turn everything off.

__Firefox:__

1. Open Firefox settings from the menu in the top right.
   <img src="/assets/posts/firefox_settings.png" alt="Firefox menu with Settings highlighted" class="image fit" />
2. Select the "Privacy & Security" tab on the left.
Firefox's standard protections cover most of what we did in Chrome.
   <img src="/assets/posts/firefox_privacy_security.png" alt="Firefox Privacy and Security settings" class="image fit" />
3. Under "Website Privacy Preferences", turn on "Tell websites not to sell or share my data".
That is [Global Privacy Control](https://globalprivacycontrol.org/).
Firefox [removed the old "Do Not Track" toggle in version 135](https://www.pcworld.com/article/2556498/firefoxs-do-not-track-feature-is-going-away-because-websites-ignore-it.html)
because websites ignored it.
   <img src="/assets/posts/firefox_website_privacy.png" alt="Firefox Website Privacy Preferences (2024 screenshot)" class="image fit" />

### 3. Lock Down Gmail

1. Click the gear icon in the top right corner, then "See all settings".
   <img src="/assets/posts/gmail_settings.png" alt="Gmail quick settings with See all settings" class="image fit" />
2. In the "General" tab, scroll down and select "Ask before displaying external images".
This blocks [tracking pixels](https://en.wikipedia.org/wiki/Web_beacon), which report when and how often you open an email.
   <img src="/assets/posts/gmail_images.png" alt="Gmail external images setting" class="image fit" />
3. Scroll further and turn off the
[smart features and personalization settings](https://techrepublic.com/article/news-google-gmail-ai-personalization-privacy-settings),
including smart features in other Google products.
This stops Google from using your email to suggest features and products elsewhere.
   <img src="/assets/posts/gmail_smart_features.png" alt="Gmail smart features settings" class="image fit" />
4. Scroll to the bottom and click "Save Changes".

### 4. If They Will Not Delete It
Use the law.

- __Colorado__: the Colorado Privacy Act gives you the
[right to have businesses delete your personal data](https://pirg.org/colorado/resources/what-the-colorado-privacy-act-means-for-you/),
and since July 1, 2024 covered businesses
[have to honor Global Privacy Control](https://www.duanemorris.com/alerts/colorado_privacy_act_requires_universal_opt_out_starting_july_1_2024_0424.html).
That is why step 2 matters.
- __California__: one request through the state's
[DROP portal](https://www.malwarebytes.com/blog/news/2026/08/californians-can-tell-data-brokers-to-drop-their-information)
goes to every registered data broker, over 600 of them. Brokers had to start processing them on August 1, 2026.
- __Everywhere else__: check whether your state has a right to deletion with the
[Bloomberg Law state privacy tracker](https://pro.bloomberglaw.com/insights/privacy/state-privacy-legislation-tracker/).
That is the sharp teeth you bring out if you have to sue.
