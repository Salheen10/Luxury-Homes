# Luxury Homes in the Microsite editor: flow and fields

Prototype: [index.html](index.html) (open in a browser, demo data only).

## What it is

Luxury Homes is **SEO configuration only**, built on the **farm areas** in IDX Configuration.

- It does **not** choose listings. Each area page has a **Listings source**: one farm area that is set to **Active**. The page shows the Active listings in its place that match that farm area (its price range and property types).
- It sets up the pages: which places get a page, the content of each page, the /luxury-homes page content, and the order and pins.
- To change which listings show, the agent edits the farm areas in IDX Configuration.

Placement: **Editor › SEO Content › Luxury Homes** (NEW badge), a separate section like City Pages and Counties.

Design source: Figma "CRM Dashboard", node 14193:401588 (final). The prototype follows it: header with Preview and Publish, the editor nav from the design, and one Luxury Homes card. Icons are the design's own SVGs in `assets/icons/`.

## Screen 1: Luxury Homes

Header of the card: **Luxury Homes** with **Cancel** (red outline) and **Save** (dark).

1. **Notice** (yellow, always shown): "SEO configuration only. Luxury Homes is built on your farm areas in IDX Configuration…" with the link **Go to IDX Configuration**.
2. **Auto** line: "Page title: {Place} Luxury Homes for Sale · URL: listahomes.com/luxury-homes/{place}".
3. **Stats**: Luxury Pages, Published, Pinned (of 6), Active farm areas (names of the farm areas set to Active, or "None active" in red).
4. Link: "How are luxury pages displayed on my microsite?"
5. **Page content** (accordions, each with Cancel / Save Change):
   - **Hero** (badge Required until complete): Label *, Title *, Description *, CTA text 1 *, CTA text 2 *, Hero image *.
   - **Contact call to action** (Editable): Label *, Title *, Description *, CTA text 1 *, CTA text 2 *.
   - **News Letter** (Editable): Label *, Title *, Description *, Button text *.
6. **Configuration** (collapsible):
   - **Farm area *** single select: lists all farm areas with price range, property types, number of luxury pages and Active / Inactive. Only **Active** ones can be picked; the first Active one is selected by default. The chosen value shows its price range badge (for example "$1M – Any").
   - Stats for the selected farm area: Luxury Pages, Published, Pinned.
   - **Luxury Pages** with the green **Add Page** button (works only with an Active farm area) and the help text.
   - If a luxury page uses an Inactive farm area, a warning lists it with a link to open it.
   - Tabs **All / Published / Draft / Pinned** and "x of y in this farm area".
   - Cards in 3 columns: name and ⋯ menu (Edit, Pin / Unpin, Publish / Set as Draft, Duplicate, Delete), badges (Published or Draft, Pinned, Farm area inactive or "n to fix"), type and place, "Listings source: {farm area}", URL. Drag cards to reorder.

## Screen 2: New / Edit luxury page (Add Page)

Not in the Figma file; it uses the same design tokens (inputs, accordions, badges, buttons). Header: back arrow, "Luxury Homes / {place}", Cancel and Save. Form header: **Pinned** and **Published** toggles.

| Field | Rule |
|---|---|
| Listings source | **Read-only.** The farm area chosen on the Luxury Homes page, with its price range, property types and Active badge. If that farm area is Inactive, the form shows an error and "Move to {Active farm area}" options; moving keeps the place if it is in the new farm area, otherwise clears it. |
| Area type * | Switch with three options: **City**, **County** or **Community** (default City). Changing the type clears the place. |
| City / County / Community * | Search field like the IDX Configuration pickers, labelled after the chosen type. It lists only values of that type from the selected farm area. **Single select**: the chosen value shows as one pill with ✕, and picking another value replaces it. One page per place. |
| Page name, H1, URL | **Automatic, not editable.** Name = the place (counties get "County"). H1 = "{Place} Luxury Homes for Sale". URL = /luxury-homes/{place}. |

**Page content** rows: Hero (H1 read-only, tagline, hero 1920×800, card 800×500, alt text), About the area (60+ characters, Fair Housing warning), Community guide (H3 topics), FAQ, Contact call to action, Section order (on/off and move; Hero first, disclaimer last, "Homes for sale (from the farm area)" always on).

**Right column**:
- **Listings source**: the selected farm area, its Active status and price range, with the text "This page shows the Active listings in {place} that match this farm area… Nothing about listings is set here." Link to IDX Configuration.
- **Before you publish** checklist: listings source picked, farm area is Active, place picked, place is in that farm area, no duplicate place, hero and card images, alt text, about text, FAQ and guide items complete. Fair Housing wording only warns.

Footer: Cancel / Save. Save with Published on makes the page live; with Published off it saves a draft.

## Not in this section (removed on purpose)

- Preview of the public pages.
- Listing counts and sample listings.
- Price range, property sub types and the number of listings shown. All of them come from the farm area.
- SEO fields (meta title, description, social image). They are generated by the system.

## Edge cases

- **Farm area set to Inactive after the page was made** (Lake Mary in the demo uses "Seminole Golf Homes", which is Inactive): the card shows "Farm area inactive", the editor explains it, and the page cannot be published until the agent picks an Active farm area or sets it to Active again. What happens to a page that is already live is a decision below.
- **Place removed from the farm area**: the editor says the place is not in that farm area, and the page cannot be published.
- **Farm area changes** (price range, property types): the area pages follow automatically. Nothing to update here.
- **Duplicate place**: blocked, one page per place.

## What the public site gets

- /luxury-homes: the content above plus one card per published area (pinned first, then the card order).
- Area page: server-rendered, self canonical, `index, follow`, in the sitemap, with meta and schema generated from the H1 and content. Listings from the farm area.
- Draft area: 404, noindex, not on /luxury-homes, not in the menu or sitemap.
- Site menu: **Luxury Homes ▾** lists pinned published areas + "All luxury homes".

## Open decisions

1. **Communities**: farm areas have County, City and ZIP fields, with no Community field. Golden Oak, Bella Collina and Reunion are stored as cities in the live farm area today. Either add a Community (Subdivision) field to farm areas, or keep reading communities from the City field.
2. **Live page whose farm area becomes Inactive, or whose place is removed from it**: switch it to draft automatically, or keep it live with the no-results message.
3. **Scope vs User Story v2**: v2 limits Luxury Homes to listahomes.com with 11 fixed communities, $1M+ and an SEO sheet. This makes it an editor section for any Microsite with IDX and farm areas, with automatic SEO.
