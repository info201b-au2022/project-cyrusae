# MTG cardknower

## Metadata

| Data | Value |
| ---- | ---- |
|Code name | alpha-krasis |
|Project title| MTG Cardknower |
|Authors | Cyrus Eosphoros (`cyrusae@uw.edu`) |
|Affiliation |  INFO-201: Technical Foundations of Informatics - The Information School - University of Washington |
|Date | Autumn 2022|

### Abstract

## Introduction

## Problem domain

### *Magic: the Gathering* in context

### Stakeholders

## The dataset 

### Notes

The *Bulk Data* files served by Scryfall's API (all of those used in this project aside from `sets` data) have dynamic URLs that change daily. These URLs must be obtained by querying the [Bulk Data API](https://scryfall.com/docs/api/bulk-data) at `https://api.scryfall.com/bulk-data`. 

All responses to requests by the Scryfall API are received in JSON format. **The majority of these files are too large to be uploaded directly to GitHub**, and as such are physically impossible to include in this repository. The ETL script in the provided RMarkdown notebook will, with the appropriate dependencies installed, generate an equivalent local copy of the data if you have cloned this repository.

**Local copies will not be identical to the files being used by the author at the time of any given update to this repository.** Scryfall's API updates every 12 hours with new cards, sets (when announced), rulings, and errata. By the time changes have been committed to this repository, the data used will overwhelmingly qualify as "stale" by internal standards. The full ETL structure for this project and [Scryfall itself](https://scryfall.com/docs/api/migrations) provide (mutually complementary as needed) infrastructure for reconciling changes to existing records. These features (in both cases) are in active development beta at this time. While all involved parties can be assumed to be attempting to make our respective systems as resillient against future changes to the game as possible, it is also entirely possible for Wizards of the Coast itself to release new cards that constitute breaking changes to the game, or for Scryfall to alter the schema of their upstream database.

Some elements of the data are more affected by the above eventuality than others. **Mechanical information should be considered the most brittle/volatile of available variables.** This is due to multiple factors: the game's rules and templated syntax are what changes as cards are released; the existence and ongoing development of a digital-only version of the game via *Magic: the Gathering Arena* introduces novel forms of rules complexity unavailable in an analog format; the mechanical factors tracked by Scryfall are limited to a subset of *keywords*, such that the ongoing inclusion of other features of rules text is contingent on machine-readable access to the Comprehensive Rules. This last resource must be generated by scraping web pages maintained by third parties where available and is subject to all limitations that would imply.

Finally, support for *Arena* includes the release of not only digital-only supplementary sets with unique cards and mechanics, but "Rebalanced" iterations of otherwise mechanically unique cards which share a human-legible name. At this time, no means of attaining information about Rebalanced cards programmatically exists. (A human-curated list is available, contingent on updates, [on the fan-curated wiki](https://mtg.fandom.com/wiki/Rebalanced_card).) In addition, cards can have *multiple* iterative balance changes over time. While "Rebalancing" creates a mechanically unique card name with the `A-` prefix (by Scryfall's tracking), the Scryfall repository includes only the most recent iteration of information about any specific card, as opposed to tracking changes after a Rebalanced card's first iteration or changes made after release to born-digital cards. 

**Compensating for this lack of data is a major goal in the ongoing development of `cardknower`'s ETL process**, but is contingent on a high grade of initial complexity before development of those features can begin in earnest, will create records whose completeness is affected by the state of development at their time of creation, and depends on the status of the upstream data at any given time of import. The nature of updates also limits my ability to distinguish between changes to card data caused by Scryfall's internal errata and actual mechanical change. Failing official support for programmatic tracking of changes to the game, a complete historical timeline can only be compiled via an excursion into Wizards' update announcements, assuming stable access to that material online continues. The second-best solution, reliance on the fan labor of the aforementioned wiki, depends on that fan labor, their access to a platform, and the reliability of the wiki editors.

The above factors mean that:

- a) Any filepath directing you to data within this repository will contain placeholders or otherwise be approximate: the files do not exist on your machine prior to further intervetion, and the download process appends unique timestamps to new files for the purposes of identification and comparison.
- b) Code dependent on the existence of expected data *must* be run in order--and, where environmental variables are involved, as part of the same R session--for it to be allowed to create the necessary files.
- c) References to number of observations (rows) are approximate, will become increasingly inaccurate over time, and can be expected to increase exclusively.
- d) Any variables contingent on scraping the Comprehensive Rules depend on an additional third party to provide an HTML page that can be queried at runtime, and thus are additionally vulnerable to URL changes/link rot, intentional or unintentional changes to the material, or any lapse in attention on the part of the third party mirroring the content of Wizards' Comprehensive Rules updates, which are released as PDFs.
- e) During any period of development in which one is being used, **SQLite databases used for persistent storage must be created at compile/setup time** and that persistence will be circumscribed by the above limitations.
- f) Upstream development may create breaking changes to expected filepaths and database schema, impacting any local copy of this repository. These changes will be noted on release when possible.
- g) Ability to track evolution of cards in digital formats (see above) in a local iteration of this repository will be limited to comparisons made against the earliest local iteration available. API access to the "canonical" `cardknower` data, as opposed to end users being restricted to the Shiny analytics frontend, is a long-term development goal, but constitutes a major undertaking contingent on (among other things) availability on the part of the developer, reliable access to the upstream resources involved, migration of the Shiny app from a SQLite database to a hosted database engine, and access (technical and financial) to production-grade public-facing hosting for said resulting database. Proposals for and contributions to efforts toward surmounting these issues are especially welcome.
- h) Even under ideal circumstances for the caveats listed in *(g)*, **Rebalanced and born-digital cards' records must be understood to be more incomplete than their analog counterparts'.**

### Original files

|Header |Test |Test |
|------|-----|-----
|X | X| X|

`*`"Observations" consist of printed and reprinted cards, and the set releases to which they belong. These 

Raw data files are stored in `/data/scry/` on download: JSON in `./json`, 

### Expected cleaned data files *(as of October 2022)*

#### Directories

- */data/canon/*: Most recent cleaned copies from upstream.
- */data/new/*: Staging area for prospective updates.

#### Form of `.csv` files

### Database schema after ETL process

*TBD contingent on ongoing development and refactoring relative to the sibling `deckdater` project.*

## Bibliography 

Biderman, S. (2020). Magic: the Gathering is as Hard as Arithmetic. arXiv. 
https://doi.org/10.48550/arXiv.2003.05119

Churchill, A., Biderman, S., and Herrick, A. (2019). Magic: the Gathering is Turing Complete. arXiv. https://doi.org/10.48550/arxiv.1904.09828

Magruder, D. S. (2022). A Conservative Metric of Power Creep. Games and Culture, 17(5), 721???751. https://doi-org.offcampus.lib.washington.edu/10.1177/15554120211050812

Manis, M. (2012). The ???Magic: The gathering??? lexicon (Order No. 1510404). ProQuest Dissertations & Theses Global. (1016132546). https://www.proquest.com/dissertations-theses/magic-gathering-lexicon/docview/1016132546/se-2