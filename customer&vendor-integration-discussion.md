This is a big, complex problem. I'm trying to lay it out in a way that we can all understand the nuances of what's going on without getting too technical or hand-wavy. I've got a summary of the problems, at the top... then I've laid out the individual problems with this relationship ([CUSTOMER 1] :: KazooWeb :: [VENDOR 1]). A the bottom, I get into some potential solutions.

I don't have a recommendation right now because I need some more time to think about this. John and I are leaning towards `Solution 4`, but it's a nascent idea and we've a lot of due diligence to do before we can comfortably commit to it.

## Problem Summary
[CUSTOMER 1]'s rewards experience is atypical and has been highly customized to meet their needs. The [VENDOR 1] integration in KazooWeb was built for a more typical experience our other customers have. Through technology, all things are possible... but with the timelines and resource constraints we have, our current implementation doesn't meet [CUSTOMER 1]'s needs.

Ultimately, [CUSTOMER 1] wants to have their HR/Benefits team fund **Anniversaries** while allowing flexibility for their individual departments to supply additional funding for its employees. Unfortunately, YEI does not support this type of accounting and [CUSTOMER 1] has very real budgetary concerns, hence their unique implementation of a rewards catalog. While the desired budgetary and accounting functionality has already been proposed for KazooWeb, it's not scheduled to complete until Q4 2021 at the earliest.

The implementation of [CUSTOMER 1]'s rewards experience in YEI is so bespoke to the particulars of YEI & [VENDOR 2] that we cannot sustainably replicate that experience with [VENDOR 1], full stop. This is due, in part, to the limitations of [VENDOR 1]'s filtering capabilities, the frequency at which their suppliers change offerings and the fickleness of [CUSTOMER 1]'s desired experience.

The rollout of the first few [VENDOR 1] Cohorts did not inspire confidence in their ability to handle these complex scenarios, and the risk associated with [CUSTOMER 1]'s migration failing is too high to commit to without some structural changes to how the KazooWeb :: [VENDOR 1] integration works.

### Problems

`Problem 1`: [CUSTOMER 1] has really has two types of rewards programs, but we don't really have a way to differentiate the `point` for these programs. YEI has a single point type. YEI cannot support department-budgets.
	- **Anniversary** is their main experience, wherein an employee receives a large number of points on their anniversary and has access to a curated catalog.
	- **Small dollar/Experiential** is their day-to-day experience. An employee receives a small number of points each quarter and can redeem for free or nearly-free rewards like `Coffee from the Boss` or a `$5 Gift Card`. Individual departments may supply funding for not-free rewards, but the accounting/budgeting of that is managed through a combination of reports from YEI and internal invoicing.

`Problem 2`: [CUSTOMER 1] heavily curates their Anniversary catalogs on both SKUs and prices, rounding the point cost of everything to help accommodate an employee exhausting their "Anniversary Points" in the minimum number of transactions.

`Problem 3`: [VENDOR 1]'s rewards catalogs are dynamic, real-time feeds from a variety of suppliers. An individual SKU's availability changes daily or hourly and [VENDOR 1] cannot guarantee their supplier will always have access to that item.

`Problem 4`: [VENDOR 1] _can_ create a custom catalog with SKUs from their **Merchandise** feed, but the SKUs' availability is subject to the suppliers' availability. (See Problem 3)
  - If [CUSTOMER 1] chooses a specific pair of Ray-Ban sunglasses on October 1, it may go out of stock on October 15 _and never return_. Or it may return on November 11. We have zero guarantees and [VENDOR 1] can't offer one either.
  - We _could_ have [CUSTOMER 1] choose `n-number` of SKUs of a specific type to mitigate the risk of all items going out of stock (e.g. 15 pairs of sunglasses instead of 3)
  - Unfortunately, we'd run into an issue with seasonality. The 2020 iPad model is only going to be valid for a finite period of time, then it'll never be sold again. And now we're back to an issue where all the selected SKUs may be discontinued and there's no possibility they'll return, resulting in a catalog with zero rewards in it.

`Problem 5`: [VENDOR 1] cannot provide filtering on individual rewards, only on catalogs. If we **do** create these customized catalogs for [CUSTOMER 1], we'd require a unique one for _each_ anniversary.
  - Currently [CUSTOMER 1] has 9 groups (00 to 45, every 5 years). [VENDOR 1] splits SKUs by _type_.
  - If [CUSTOMER 1] wants both Merchandise and Gift Cards available for the `05` anniversary group, we'll need _two_ catalogs.
  - That results in dozens of catalogs that'd need to be created and managed.

`Problem 6`: Each of these catalogs needs to be managed manually.
  - [VENDOR 1] seemed unwilling to commit to that work
  - [CUSTOMER 1] _cannot_ commit to that work because [VENDOR 1] does not really have a siloed way for them to do it without [CUSTOMER 1] having access to all of our other customers' data
  - It'd be a large burden to put on an account manager

`Problem 7`: The timing to shift their rewards model is really, really bad.
  - [CUSTOMER 1]'s renewal fast approaching
  - [CUSTOMER 1] is shifting HRIS providers in January, creating a distraction for their HR team
  - [CUSTOMER 1] is migrating to `KazooWeb` at a similar time, creating another distraction for their team to deal with

### Potential Solutions
`Solution 1`: Keep [CUSTOMER 1] on [VENDOR 2] until we can update, enhance or expand the KW & [VENDOR 1] experiences to meet their needs over the next 12-15 months.
  - `Risk 1`: The main project that enables [CUSTOMER 1]'s use case is the `Points Microservice`, but many preceding projects block its start and delays in their delivery will ultimately delay the delivery of a solution that enables [CUSTOMER 1] to migrate.
  - `Risk 2`: [VENDOR 2]'s revenue from our customers has evaporated. There's a very real risk their company goes out of business before we can finish our work to migrate [CUSTOMER 1].
  - `Opportunity 1`: The least amount of shuffling to the Kazoo 2021 AOP
  - `Opportunity 2`: Provides a long timeline for iterative updates.

`Solution 2`: Tell [CUSTOMER 1] to suck it up and change their methodology. (Clearly not a viable option.)
  - `Risk 1`: While we may see some movement on aspects of their program, the root cause (budgeting/accounting) will persist.
  - `Opportunity 1`: Zero updates to the [VENDOR 1] integration
  - `Opportunity 2`: Deep satisfaction in telling them to suck it up

`Solution 3`: Shuffle the 2021 AOP to address some outstanding issues with Points & accounting sooner
  - `Risk 1`: We can't start this work before both `kwUsers` and `kwProvisioning` are complete, meaning a January _start_ time
  - `Risk 2`: This deprioritizes some key foundational work like `Emails`, `Actions`, `Notifications`,  `Recognition Enhancements`
  - `Risk 3`: Extensive updates to the BI dashboards will be required to use the new data
  - `Risk 4`: The Points Microservice is a fairly large ball of wax. We have spent the _least_ amount of time discussing how to implement its technical intricacies, but we **do** have a good idea of the functional requirements.
  - `Opportunity 1`: The Points Microservice has been in discussion for nearly a year. It's a project we know and _want_ to do. Completing it soon serves _many_ customers, not just [CUSTOMER 1].

`Solution 4`: Embrace [VENDOR 1] as the Points Bank, offloading the accounting of points to their platform
  - `Risk 1`: Still requires work from Kazoo engineers, meaning a delay to `Emails`, `Actions`, `Notifications`,  `Recognition Enhancements`
  - `Risk 2`: Still cannot reasonably start before January
  - `Risk 3`: Solidifies [VENDOR 1] as our only vendor option. Ties our functionality to what they support _or_ are willing to build to meet our needs.
  - `Risk 4`: Requires some creative thinking about how to solve some technical challenges with both the current state and future state in mind.
  - `Risk 5`: Still requires BI work to update existing dashboards/reports
  - `Opportunity 1`: Speeds up the delivery time of sophisticated Points accounting, including variated point `types`.
  - `Opportunity 2`: Reduces the amount of complex code required for Kazoo to own/produce
  - `Opportunity 3`: Any delay to other AOP initiatives would likely be much less than building our own Points Service.
