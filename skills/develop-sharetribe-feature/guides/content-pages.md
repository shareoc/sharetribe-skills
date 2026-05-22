# Sharetribe custom page schema — field reference

You can use a combination of the following attributes to generate a content page via the Sharetribe Console.
Use this as a reference to understand what can be achieved using content pages. 
Using custom code, you are able to modify how this data is rendered. The template fetches
the page data through an API call, and it is returned in JSON. By modifying the presentational components,
you can introduce custom sections. However, it is important to note, that pages are rendered using generic components.
When modifying a generic component, it will affect the rendering of all pages. Therefore, it is important to use conditionals if you only want the customisation to apply to a specific page.

meta:                                  # SEO & social tags
  pageTitle.content: string            # ≤255 chars; rec. 50–60
  pageDescription.content: string      # rec. 50–160
  socialSharing.title: string
  socialSharing.description: string
  socialSharing.image: image           # png/jpeg, ≤20MB, rec. 1200x630 (1.91:1)

sections: array<Section>               # max 24

Section:
  sectionName: string                  # internal label, Console only
  sectionId: string                    # anchor link id, pattern ^[a-z][a-z0-9_-]*$
  sectionType: enum                    # REQUIRED
    - hero                             # no blocks; title + description + CTA. Can be overlayed over an image.
    - article                          # vertical stack of blocks, "article" style
    - carousel                         # horizontal, swipeable blocks
    - columns                          # grid of blocks
    - features                         # alternating text or media blocks. rendered horizontally.
    - listings                         # up to 10 featured listings
  title:
    content: string
    fieldType: heading1 | heading2     # default heading2
  description.content: string          # paragraph
  callToAction:
    fieldType: none | internalButtonLink | externalButtonLink | search   # default none
    # internalButtonLink: { content: string, href: string }              # href: path only, no protocol
    # externalButtonLink: { content: string, href: string }              # href: must start http(s)://
    # search.searchFields: { categories?, keywordSearch?, locationSearch?, dateRange? }  # ≥1 true
  appearance:
    fieldType: defaultAppearance | customAppearance   # default defaultAppearance
    # customAppearance:
    backgroundColor: string            # hex ^#[A-Fa-f0-9]{6}
    backgroundImage: image             # png/jpeg, ≤20MB, min 1600x1200
    backgroundImageOverlay.preset: none | dark | darker
    textColor: black | white

  # Conditional on sectionType:
  numColumns:
    # columns | carousel: 1 | 2 | 3 | 4   (REQUIRED)
    # listings:            3 | 4          (default 4)
  listingSelection: newest | queryString    # listings only, default newest
  listingSearchQuery: string                # REQUIRED if listingSelection=queryString
  blocks: array<Block>                      # max 24; only for columns | carousel | article | features

Block:
  blockName: string                    # internal label
  blockId: string                      # anchor link id
  blockType: const "defaultBlock"      # 
  media:
    fieldType: none | image | youtube  # default none
    # image:
    image: imageRef                    # png/jpeg, ≤20MB               
    aspectRatio: "1/1" | "16/9" | "2/3" | "auto"   # default auto       
    alt: string                                                          
    link.fieldType: none | internalImageLink | externalImageLink
    # youtube:
    youtubeVideoId: string             # pattern ^[0-9A-Za-z_-]+$       
    aspectRatio: "1/1" | "16/9" | "2/3" | "auto"                         
  title:
    content: string
    fieldType: heading1 | heading2 | heading3   # default heading3
  text.content: markdown
  callToAction:
    fieldType: none | internalButtonLink | externalButtonLink   # default none
  alignment: left | center | right     # default left