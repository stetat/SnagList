# SnagList: Stakeholders and Requirements

**Version:** 0.1 (draft)
**Date:** November 2026
**Stage:** 2, following the [Vision](README.md#vision)

---

## 1. About this document

The vision explains why SnagList should exist. This document is about what it actually has to do. We start with the people involved, walk through a typical job, and then list the requirements in a form we can refer back to when we get to use cases, design, and testing.

Every requirement has an ID (FR for functional, NFR for non-functional), so later documents can point to it. Priorities use MoSCoW:

- **Must:** the first version is useless without it
- **Should:** important, but we could ship a week later without it
- **Could:** nice to have if time allows
- **Won't:** deliberately left out of this version (see section 7)

This is a draft. Some decisions are still open, and we've listed those at the end instead of guessing.

---

## 2. Stakeholders and users

| Stakeholder | Who they are | What they want | What goes wrong today |
|---|---|---|---|
| **Contractor** | Owner of a small building or renovation business, often a team of one plus regular subcontractors. Pays for SnagList. | Finish jobs on time, get the final payment, stop arguing about what was or wasn't agreed | Defects live in a notebook, a camera roll, and five WhatsApp chats. Things get forgotten, and the contractor ends up driving back to the site for small fixes. |
| **Subcontractor** | Tiler, electrician, plumber, painter, and so on. Works for several contractors at once. | A clear list of what they personally need to fix, where, and by when | Gets vague messages like "fix the socket in the kitchen" with no photo, and finds out about extra items on the last day |
| **Client / homeowner** | The person who ordered the work and holds back the final payment until they're happy | To see progress without chasing anyone, and to be sure nothing was quietly skipped | Has to keep their own list and keep asking "is it done yet?" |
| **Inspector / architect** *(optional)* | Appears on bigger jobs or where a formal handover is required | A tidy record of defects and how each one was resolved | Receives a pile of photos and has to reconstruct the story |
| **SnagList team** | Us: developers and administrators of the service | A system that is cheap to run and easy to support | — |

A few things stood out when we wrote this table:

- Only the contractor pays, but the subcontractor and the client decide whether the product feels easy or annoying. If they have to install an app or create an account, many of them simply won't.
- Subcontractors are not loyal to one contractor. One tiler might get links from three different SnagList users in the same week, and that has to stay simple for them.
- The client is not technical, and is often a little suspicious. Their view should be read-only and calm, not a dashboard.

---

## 3. A typical job, start to finish

To keep the requirements grounded, here is the scenario we kept coming back to.

> Marek is renovating a two-bedroom apartment. Two weeks before handover he walks through it with the owner, Anna. He opens the project in SnagList on his phone, picks a room, taps a spot on the floor plan, takes a photo of a chipped tile, and types "replace chipped tile next to bathtub." He assigns it to his tiler with a deadline of Friday. By the end of the walk there are 27 items.
>
> Each subcontractor gets a message with a link. The tiler opens it, sees his four items with photos and locations, fixes them over two days, and marks each one as fixed with an "after" photo.
>
> Marek checks the fixes. Three are fine, so he closes them. One isn't, because the grout colour doesn't match, so he reopens it with a comment. Anna checks the progress page in the evening and sees "21 of 27 closed."
>
> When everything is closed, Marek generates a handover report with before and after photos. Anna signs it on her phone, and Marek sends the final invoice the same day.

Almost every functional requirement below comes from a step in this story.

---

## 4. Functional requirements

### 4.1 Accounts and projects

| ID | Requirement | Priority |
|---|---|---|
| FR-01 | A contractor can sign up with an email address or phone number and log in. | Must |
| FR-02 | A contractor can create a project with a name, address, and optional client name. | Must |
| FR-03 | A contractor can split a project into areas (rooms, floors, or zones) and name them. | Must |
| FR-04 | A contractor can upload a floor plan (image or PDF) for an area. | Should |
| FR-05 | A contractor can archive a finished project. Archived projects stay readable but can't be edited. | Should |
| FR-06 | A contractor can keep a list of regular subcontractors (name, trade, phone or email) to reuse across projects. | Should |

### 4.2 Defect items

| ID | Requirement | Priority |
|---|---|---|
| FR-07 | A contractor can create a defect item with a short title, an optional description, an area, and at least one photo. | Must |
| FR-08 | Photos can be taken directly with the phone camera from inside the app, not only uploaded from the gallery. | Must |
| FR-09 | If the area has a floor plan, the contractor can place a pin on it to mark the defect's exact position. | Should |
| FR-10 | Each item has one of these statuses: *Open, Assigned, Fixed, Verified, Reopened, Closed*. | Must |
| FR-11 | Only allowed status changes are possible (for example, a subcontractor can't mark their own fix as *Closed*). The full set of transitions will be specified in the state diagram in stage 3. | Must |
| FR-12 | A contractor can edit or delete an item while it is still *Open*. After that, changes are recorded in the item's history rather than overwriting it. | Must |
| FR-13 | The contractor can filter the item list by status, area, subcontractor, and overdue items. | Must |
| FR-14 | Items can be shown as coloured pins on the floor plan, with the colour showing status. | Could |

### 4.3 Assigning work to subcontractors

| ID | Requirement | Priority |
|---|---|---|
| FR-15 | A contractor can assign an item to one subcontractor and set a deadline. | Must |
| FR-16 | Each subcontractor receives a personal link for the project. Opening it shows only the items assigned to them, without creating an account. | Must |
| FR-17 | A subcontractor can mark an item as *Fixed* and must attach at least one "after" photo when doing so. | Must |
| FR-18 | A subcontractor can leave a comment on an item (for example, "need the client to choose a grout colour"). | Should |
| FR-19 | A contractor can reassign an item to a different subcontractor. The previous assignee loses access to it. | Should |

### 4.4 Verification, client view, and sign-off

| ID | Requirement | Priority |
|---|---|---|
| FR-20 | A contractor can review a *Fixed* item and either close it or reopen it with a required comment explaining why. | Must |
| FR-21 | A contractor can share a read-only client link showing overall progress (for example, "21 of 27 closed") and the list of items with photos. | Must |
| FR-22 | The client can comment on an item but can't change its status. | Should |
| FR-23 | When all items are closed, the contractor can generate a handover report (PDF) listing every item with its before and after photos and dates. | Must |
| FR-24 | The client can sign the handover report on screen. The signature, date, and time are stored with the report. | Should |

### 4.5 Notifications

| ID | Requirement | Priority |
|---|---|---|
| FR-25 | A subcontractor is notified when an item is assigned to them or reopened. | Must |
| FR-26 | The contractor is notified when an item is marked *Fixed*. | Must |
| FR-27 | Everyone with an assigned item gets a reminder one day before its deadline and another when it becomes overdue. | Should |
| FR-28 | Notifications can be sent by email, SMS, or Telegram. The contractor chooses the default channel per subcontractor. | Should (email: Must) |

### 4.6 History and records

| ID | Requirement | Priority |
|---|---|---|
| FR-29 | Every change to an item (creation, assignment, status change, comment, new photo) is recorded with who did it and when. | Must |
| FR-30 | History entries can't be edited or deleted by any user. | Must |
| FR-31 | A contractor can export a project's items and history to CSV. | Could |

---

## 5. Non-functional requirements

| ID | Category | Requirement | Priority |
|---|---|---|---|
| NFR-01 | Usability | A contractor using SnagList for the first time can create a project and log their first defect in under 5 minutes, without a tutorial. | Must |
| NFR-02 | Usability | A subcontractor can open their link and mark an item as fixed in under 1 minute, on first use. | Must |
| NFR-03 | Usability | All main screens work one-handed on a phone screen of 360 px width or more. | Must |
| NFR-04 | Performance | An item list of up to 200 items loads in under 3 seconds on a mobile 3G connection. | Should |
| NFR-05 | Reliability | If the connection drops while a defect is being created, the entered text and photo are not lost and are sent once the connection returns. | Should |
| NFR-06 | Security | Share links contain a random token that can't realistically be guessed, and the contractor can revoke any link at any time. | Must |
| NFR-07 | Security | Users only ever see data from projects they belong to. A subcontractor's link never reveals other subcontractors' items or the contractor's other projects. | Must |
| NFR-08 | Privacy | Personal data is processed in line with GDPR. A contractor can delete their account and all its data. | Must |
| NFR-09 | Data retention | Projects, photos, and history are kept for at least 3 years after archiving, to cover typical warranty periods. | Should |
| NFR-10 | Storage | Photos are compressed on upload to a reasonable size (around 1 MB) while staying clear enough to see small defects. | Should |
| NFR-11 | Availability | The service is available 99.5% of the time, measured monthly. | Should |
| NFR-12 | Localisation | The interface supports English and Polish, and can be extended with more languages without code changes. | Should |
| NFR-13 | Compatibility | Works in the current versions of Chrome, Safari, Firefox, and Samsung Internet, on both Android and iOS. | Must |

---

## 6. Constraints and assumptions

### Constraints

- The first version is a **web application** that works well in a mobile browser. We are not building native iOS or Android apps.
- The project is built by a small student team within one semester, so scope has to stay modest.
- Hosting and third-party services (SMS especially) must stay within a small budget, which is one reason email is the only *Must* channel.
- Data is stored in the EU because of GDPR and because our first likely users are in Poland.

### Assumptions

- Contractors and subcontractors have a smartphone with a camera and mobile internet, at least some of the time on site.
- A contractor usually has no more than 5 active projects and 200 items per project. If this turns out to be wrong, NFR-04 needs revisiting.
- Subcontractors are willing to tap a link and take a photo, but not to install an app or create an account. This is the most important assumption in the document, and we plan to test it by talking to real contractors.
- Clients are fine with signing a handover report on screen rather than on paper.

---

## 7. Out of scope for the first version

We've deliberately left these out. They come up naturally when you think about construction software, so we're writing them down to stop scope creep.

- Invoicing and payments
- Scheduling of the whole construction project (Gantt charts, material orders)
- Native mobile apps
- Offline mode beyond what NFR-05 covers
- Integrations with large construction platforms or BIM models
- Subcontractor ratings or a marketplace for finding subcontractors

---

## 8. Open questions

- Should the client be able to create defect items themselves, or only comment? Allowing it is more honest, but contractors may not like it.
- Do we need several contractor accounts per company (for example, an owner and a site manager) in the first version, or is one login per business enough?
- Is a signature drawn on screen legally meaningful enough for a handover in Poland, or should it be treated only as confirmation?
- Which notification channel do subcontractors actually read? WhatsApp would probably win, but its business API is expensive and has approval requirements.

We'll come back to these after talking to a few contractors and before stage 3 (use cases and UML models).
