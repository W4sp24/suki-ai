# Suki.ai

**From "mine!" to paid and packed: the order desk for Facebook Live sellers.**

Built for CSSprint 2026, the UST Computer Science Society hackathon.

> Status: initial draft. This README describes the idea and the planned solution. Sections marked `TODO` will be filled in as we build and test.

---

## The problem

**The hackathon's problem statement**

> "Develop practical and digital solution that address a specific problem faced by Filipino MSME's."
>
> CSSprint 2026, revealed on Day 1 (October 22, 2026)

**The specific problem we chose**

Filipino MSMEs that sell through Facebook Live lose money between the comment and the delivery. So our question is:

**How might we help Facebook Live sellers turn chaotic live comments into confirmed, paid orders, so they stop losing money to missed claims, buyers who never pay, and fake payment proofs?**

During a live, comments fly by fast. "Mine B12", "ate yung blue polo po", "cancel ko na B3". The seller has to catch every claim, remember who was first, and track who's next in line, all while talking to the camera.

After the live, the real work starts:
- Buyers send screenshots as proof of payment, and the seller has to check each one against her GCash or bank app by hand.
- Some screenshots are fake. If she misses one, she ships an item she was never paid for.
- Some buyers claim items and never pay ("joy reservers"), so the item is stuck instead of going to the next buyer.

## Who it's for

Micro and small businesses that sell mainly through Facebook Live, starting with ukay and fashion sellers.

**Meet Jessa.** She runs an ukay business in Tondo and goes live 3 times a week, with 40 to 80 items per live. She captures orders by scrolling through comments and writing them in a notebook. Checking payments takes her hours after every live, and she has been burned by fake receipts before.

> `TODO`: replace or support Jessa's story with quotes from the live sellers we interview.

## Proof of the pain

- **MSMEs are almost every business in the country:** 99.63% of all enterprises in 2023, and 90.43% are micro. ([DTI, from PSA 2023 data](https://dti.gov.ph/businesses/msmes/msmeresources/msme-statistics))
- **Facebook Live orders are still captured by hand.** Meta ended its official live shopping features, but sellers kept selling through comments. ([GMA News, 2022](https://gmanetwork.com/news/money/companies/841010/users-in-philippines-may-still-do-facebook-live-selling-meta/story))
- **Fake payment proofs cost real money.** Police arrested a bogus buyer who bought almost ₱230,000 worth of clothing online. ([DZRH](https://dzrh.com.ph/post/pnp-arrest-a-bogus-buyer-who-purchase-almost-p230000-worth-of-clothing-online))
- `TODO`: how many claims never get paid, and how many hours sellers spend after each live (from our seller interviews and survey).

## Our solution

Suki.ai follows a live order from the first comment to the packed parcel, in three steps.

**1. Capture orders during the live**
- Reads the live comments from the seller's own Facebook Live, with her permission, through Meta's official API.
- Turns comments into orders in real time. "Mine B12" claims item B12 for the first buyer.
- A second buyer for the same item joins a waitlist. If the first buyer cancels or doesn't pay, the next one gets it.
- Comments the system isn't sure about go to a review list instead of being guessed.

**2. Check payments before shipping**
- Each buyer gets a private order link with their total.
- The buyer checks the payment in their own wallet app first, then submits the reference number.
- The system matches it against the seller's own GCash or bank statement and shows a clear result: "Match: ref found, ₱850 = ₱850" or "Hold: reference not in your statement".
- The seller makes the final call with one click: Release or Hold.

**3. Pack with confidence**
- A packing list of verified orders only, grouped by buyer.
- Session totals after every live: sold, paid, unpaid, held.
- Buyers with a history of unpaid claims get flagged, so the seller knows who to watch next time.

## How it works

```
Seller goes live
      |
Live comments ----> Read the claim ----> Claim queue + waitlist ----> Order board
                    (rules first,
                     AI for messy ones)
                                                                          |
Buyer pays ----> Buyer self-check + reference no. ----> Match against seller's statement
                                                                          |
                                                          Seller confirms: Release / Hold
                                                                          |
                                                          Packing list + session report
```

## What makes it different

Existing tools help sellers collect claims from live comments. We close the loop to **paid**.

```
                              Notebook /    Live order tools     [Product name]
                              spreadsheet   (e.g. LiveOrder Pro,
                                            BigSeller)
Captures claims from comments  Manual        Yes                  Yes
Handles messy Taglish claims   Manual        Partly               Yes (rules + AI)
Waitlist when buyer backs out  Manual        TODO: check          Yes
Checks payment vs statement    Manual        No                   Yes
Catches fake payment proofs    Manual        No                   Yes
Flags repeat non-payers        Memory        TODO: check          Yes
```

`TODO`: confirm what each competitor actually does before we put this table on a slide.

## Where AI fits, and why

**Rules first, AI only where rules fail.**
- Simple comments like "mine B12" are handled by plain rules. They're instant and easy to explain.
- Messy Taglish comments like "ate yung blue polo po, 2 pcs" don't follow a pattern. Keywords break on these, so an AI model reads only the comments the rules couldn't.
- If the AI is unsure or unavailable, the comment goes to the review list. The seller never gets a wrong order silently.
- Every money decision (queue, payment match, Release/Hold) is plain logic, not AI, so it's predictable and explainable.

`TODO`: our benchmark result. Rules alone read __% of real comments correctly. Rules + AI read __%.

## Payment modes

- **Manual (default):** buyer self-check, reference number, statement match, seller confirms. Works for any seller with no setup.
- **Instant QR (optional):** the buyer pays a QR Ph code, and the order is verified in seconds with no statement upload. Exploring this through a payment gateway in test mode.

## Privacy and trust

- Uses only Meta's official API, with the seller's permission. No scraping and no posting on the seller's behalf.
- Buyer names are masked on shared or projected screens.
- The AI only sees the comment text and item codes, never buyer names or account details.
- We never ask for the buyer's account number.
- Each seller's buyer records stay in her own account. No shared blacklist between sellers.
- Built with the Data Privacy Act of 2012 (RA 10173) in mind, including a way for the seller to delete a buyer's data.

## Scope

**In the MVP**
- Catalog setup (item codes, prices, stock)
- Live order capture: comment reading, claim queue, waitlist, order board
- Payment check: buyer order link, statement match, Release / Hold
- Packing list and session totals



- Meta Graph API / Live Video API
- `TODO`: libraries and frameworks (from package.json and requirements.txt)
- `TODO`: payment gateway (if Instant QR is built)
- AI assistance: Claude was used for research, planning and coding help, as allowed by the event rules. All code was written during the event, and the team can explain every part of it.
