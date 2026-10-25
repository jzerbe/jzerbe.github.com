---
author: Jason Zerbe
layout: post
location: 49.440384, -124.761068
outcome: WORKED
published: true
tags:
- Canada tax policy
- Alberta
- British Columbia
- Limited Company
title: The Western Canada Limited Company
---
__Result:__ It worked. My company, Vraid Systems Limited, moved from British Columbia to Alberta on September 22, 2026.
The continuance took about 11 weeks and cost 1,003.82 CAD through a registration agent.
BC was the right call in 2021 because a foreigner could run a company there entirely self-service.
Then BC started requiring a BC Services Card for most filings, which only residents can get.
Once I needed an agent anyway, Alberta won on everything else.

_First written July 2021, updated October 2026._

## The Problem
My minimum bar for most strategies: residency and citizenship should not matter.
I want a Canadian company I can run from anywhere, with as little upkeep as possible:
easy tax filing, no required meetings, and full flexibility to act as director whenever I need to.

__Hypothesis (2021):__ British Columbia is the best place for a non-resident to run a Canadian company,
because the whole thing is self-service online.

__Hypothesis (2026):__ Once BC requires an agent for non-residents, Alberta is better on every count that is left.

## Why Western Canada

- __Non-resident friendly__: British Columbia lets incorporators and directors be non-residents.
Alberta [dropped its rule that 25% of directors be Canadian residents](https://www.bennettjones.com/Insights/Blogs/Important-Changes-to-the-Alberta-Business-Corporations-Act-Now-in-Effect)
on March 29, 2021.
Nova Scotia allows non-resident directors too, but has [high annual renewal fees](https://www.novascotia.ca/incorporate-limited-company).
- __Alberta's taxes__: no provincial sales tax, only the 5% federal GST.
Like other energy exporters, the province pays for much of its services with energy royalties.
The combined general corporate rate for 2026 is
[23% in Alberta vs 27% in BC](https://www.taxtips.ca/smallbusiness/corporatetax/corporate-tax-rates-2026.htm).
- __Market access__: once you incorporate in a member province, the rest of Western Canada is easy to sell into under the
[New West Partnership Trade Agreement](https://www.newwestpartnershiptrade.ca/the_agreement.htm).

## The Solution I Tried

### 1. Set up as a non-resident
- __Address__: for about 15 CAD a month, [Anytime Mailbox]({{ site.program.business.anytimemb }})
or [iPostal1]({{ site.program.business.ipostal1 }}) gives you a remotely managed address in Alberta or British Columbia.
That covers the requirement for an office and mailing address in your province.
- __Investment Canada Act__: a non-Canadian starting a brand-new Canadian business only has to notify
_Innovation, Science and Economic Development Canada_, before or within 30 days of starting it.
New businesses are [notifiable, not reviewable](https://www.cwilson.com/rules-governing-foreign-investment-in-canada/).
Fill out [ISED-ISDE3675E](https://ised-isde.canada.ca/site/investment-canada-act/en/investment-canada-act/forms) and email it back.
- __Personal tax number__: to file with the Canada Revenue Agency for your company, you need an ITN.
Send in the
[_Application for a Canada Revenue Agency Individual Tax Number (ITN) for Non-Residents_](https://www.canada.ca/en/revenue-agency/services/forms-publications/forms/t1261.html)
with a notarized copy of your passport, wet signature from the notary.
I did this at my local UPS store for $5.

### 2. Incorporate in British Columbia (2021)

1. For a real name instead of a number, reserve one with the
[Name Request tool](https://www.bcregistry.ca/namerequest) for
[$30](https://www.corporateonline.gov.bc.ca/WebHelp/fee_schedule.htm).
It takes a few days to process.
1. Go through the [BC Registry Services incorporation wizard](https://www.bcregistry.ca/corporateonline/colin/accesstransaction/menu.do?action=overview&filingTypeCode=ICORP&from=main) for [$350](https://www.corporateonline.gov.bc.ca/WebHelp/fee_schedule.htm).
1. Add your name and the business name to the
[example incorporation agreement and articles](https://www2.gov.bc.ca/assets/gov/employment-business-and-economic-development/business-management/permits-licences-and-registration/registries-other-assets/samp_02_-_incorporation_agreement_adopting_table_1.pdf).
Do not forget section 9.10's proxy designation.
Convert them to PDF and get them [notarized online with Notary Pro](https://www.notarypro.ca/notarize-online/book-now/).

This worked well for five years.

### 3. Watch BC close the door (2026)
The old [Corporate Online](https://www.corporateonline.gov.bc.ca/) is being merged into
[BC Registries and Online Services](https://www.bcregistry.gov.bc.ca/en-CA/).
On the surface that is good.
The bad news: you need a [BC Services Card](https://www2.gov.bc.ca/gov/content/governments/government-id/bc-services-card)
to do most things, which means you must be a resident.
Non-residents now need a registered agent. BC made itself less competitive.

### 4. Continue the company to Alberta: 11 weeks, 1,003.82 CAD
A continuance moves the same company to a new province, with the same company and history and no new incorporation.
I hired [The Registration Center](https://theregistrationcenter.ca/) to handle it.

- __July 5, 2026__: Ordered the continuance. 49 CAD e-filing, 399 CAD authorization to continue, 499 CAD Articles of Continuance, plus tax.
- __July 17__: The agent applied to BC for authorization to continue out.
- __September 16__: BC approved. That wait was most of the 11 weeks.
- __September 17__: The snag. The company was still registered in Alberta as an extra-provincial BC company,
which blocked the continuance. I logged in and cancelled that registration myself.
- __September 22__: Continued into Alberta. Certificate and Articles of Continuance in hand.

## Did It Work?
Yes. Same company, same tax number, now in a province with lower corporate tax, no provincial sales tax,
and no residency strings on directors.

Upkeep is light:

- __Annual return__: [pay a registered agent to file it](https://theregistrationcenter.ca/product/annual-return-filing/).
- __Meetings__: private Alberta corporations can act by
[written resolution](https://www.bennettjones.com/Insights/Blogs/2023/04/If-Its-Broke-Fix-It-Amendments-to-the-Alberta-Business-Corporations-Act).
As the only shareholder, I sign resolutions instead of holding meetings.
- __Taxes__: two returns now instead of one. See below.
- __Banking__: covered in [Bootstrapping personal Canadian financial services](/2026/07/bootstrapping-canadian-finances/).

### The catch: Alberta runs its own corporate tax
The Canada Revenue Agency collects provincial corporate tax
[for every province except Quebec and Alberta](https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/corporations/provincial-territorial-corporation-tax.html).
In BC, the federal T2 return covered everything. In Alberta, it does not.
The tax number stayed the same through the continuance, but Alberta adds a second, separate return:

- __Federal T2__ with the Canada Revenue Agency, same as before. I use <https://netfilet2.taxtron.ca/>.
First you have to file an individual $0 tax return to get into the CRA's system and be self-serve.
- __Alberta AT1__ with Alberta Tax and Revenue Administration (TRA), for any corporation with a
[permanent establishment in Alberta during the year](https://www.alberta.ca/corporate-income-tax).
It is due 6 months after year-end, and since 2025 it
[has to be net filed electronically](https://www.alberta.ca/corporate-income-tax), with a 1,000 CAD penalty if not.
TaxTron is on Alberta's
[list of software certified for AT1 net file](https://open.alberta.ca/dataset/ecb4beb2-a18e-43b5-8524-98f3f0230ce0/resource/e29fad96-f608-4414-8848-d4b71d57ef84/download/tbf-corporate-income-tax-software-certified-for-at1-net-file-2020-0527.pdf).
- __Alberta account number__: the [Corporate Access Number on the Alberta certificate](https://www.alberta.ca/corporate-income-tax) is the corporate tax account number.
- __TRACS__: Alberta's online account for corporate tax. It shows assessments, handles address changes,
direct deposit, and objections. You enrol with a TRACS Enrolment Request.

What this means for Vraid: upkeep went from one tax return to two, plus a second government account to watch.
I have not dug into the AT1 closely yet, so this part is still ONGOING.

__Lesson:__ check for an old extra-provincial registration in the destination province before you start.
It costs nothing to cancel, but it will stall the continuance until you do.
