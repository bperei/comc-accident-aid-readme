# COMC AccidentAid™

A userscript that enhances the COMC experience with additional card management features thus allowing you to make educated, data driven accidents. I mean purchases.
Track your collection, monitor changes in prices, and customize how search results are displayed.

## Installation

- Works with Chrome, Firefox, Safari, and other modern browsers (Tested mostly violentmonkey/tampermonkey with chrome on windows)
- Install a userscript manager from your browser's extension marketplace (Tampermonkey, Greasemonkey, Violentmonkey (Recommended))
- Copy the source of the script (everything thats here)
- Open the userscript manager of your choice in your browser (under extensions) and click the Add Script/Create new script button (usually an icon with a plus symbol, or something similar)
- Overwrite the content of the script window with the source code of this script and hit save
- Voila!
- Some userscript managers might require additional settings to be enabled.
  - For example Tampermonkey on chrome needs to have developer mode enabled in the extension manager to allow html editing.

## Important Notes

- Works only when logged into COMC
- Price tracking starts when you first view a card
- Prices are only updated for a card when it is included in a search result
- It is not an automatic price scraper, it does not run in the background
- Functionality may be affected by future COMC website changes
- Price and other stored data is local to your browser. This means if you install the script in another browser you will start from scratch.
  There is a way to export/import the values of the local storage (indexeddb) if needed with a small amount of tinkering.

## Known Issues

- When clicking the page grid view selector on the top right, the control row snaps to the right. I spent a considerable amount of time trying to fix it with no success. Since all other view styles are pretty much useless, I don't think I'll bother fixing it in the future.

## Features

- Mark cards as owned (press 'o') or wishlist (press 'w')
- Hide owned cards with toggle checkbox
- Track price history change
  - **initial:** change in % compared to first observed price
  - **previous:** change in % compared to previous price before the latest observed change
  - **lowest:** change in % compared to lowest price observed
- Sort results by price type change (initial, previous, lowest)
- Visual indicators for owned/wishlist cards
- Duplicated pagination controls at top for easier navigation between pages
- Ability to set minimum levels for notifications 
  - They are mostly useless, so default is 'warning' to help with figuring out if there was a problem during the execution of the script

## Usage

- Hover over card + press 'o' = Mark as owned
- Hover over card + press 'w' = Add to wishlist
- Use "Hide owned" checkbox to hide owned cards
- Select (optional) sort method from "Page Sort By" dropdown


## Why am I seeing what I'm seeing?

### Price tracking and marking cards as owned/wishlist

Every card on COMC has an id which is a 7-8 digit number. A card can have different versions listed because of notes/grading notes. There are graded and ungraded cards. Price history and collection tracking works differently with these two types.

An ungraded card is usually your standard listing, with a 7-8 digit id. In some cases, COMC pregrades these cards if lets say they are not in NM-Mint condition. So if a card has rounded corners it might get pregraded by COMC as [EX to NM] which is always shown next to the player name.

(I'm not sure how long they have been doing this, but not since the beginning because there are some instances where the standard ungraded cards have lots of shitty examples for sale under the same id)

This means the same card can have multiple listings because of their difference in notes/conditions. (This is of course also true for graded cards, but they are handled differently)

For example: 
1997-98 Fleer Ultra - Ultrabilities - Starter #8 S
Kevin Garnett [EX to NM]

This card's price is tracked with the id '1479372/Ungraded/COMC/EX-NM' by the script, so it's not the same card price-wise as the ungraded, noteless version which is tracked as '1479372'. The reason for this is that you want to handle these cards separate as they have a different value based on the condition.

But when it comes to marking it owned with the script, it will mark all the ungraded versions of this card as owned, since you don't really want to collect a creased (or any other way pregraded) version of the same card. This also means that if you mark a card as owned/wishlist you might get multiple notifications of the action (if you have your min notification level set to success). So price is handled separately, but collection status is merged.

Graded cards are different, they are handled both in price and in collection as separate cards. So the same card in PSA 8 is a different card to the script as a PSA 9.

### Notifications

The current notification system is a basic implementation mostly for error and a bit of status visibility. It provides a simple way to display temporary messages during script execution.

They are not persistent and disappear after 4 seconds. Notification levels can be changed from the default (warning). Price update notifications are 'Debug' and lowest price notifications are 'Info'. Marking cards are 'Success'.
