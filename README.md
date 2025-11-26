# Kent Schaeffer Production Profile Site

Technical repository and site with my personal portfolio. Code and content follows standard coding practices, including some WCAG rules

## Hosted at

https://www.kentknowsme.com

## Project Structure

```
www.kentknowsme.com/
|- index.html               # dev home page
|- LICENSE                  # MIT license file
|- portfolio.css            # dev style sheet
|- README.md                # this file
|- robots.txt               # a file to tell robots "nothing here to see, move along"
```

## Installation

1. copy `index.html` and `portfolio.css` to your repository --OR-- fork this repo
2. make the files yours (unless you like sharing my information)
3. commit and push
4. host somewhere and share the link

## Languages and Tools Used

- HTML/CSS to begin with
- Started with Bootstrap framework, but then migrated to using Vue as frontend
- Vue.js in a single HTML file (may be separated at some point)
- Visual Studio Code is now the industry standard IDE, but feel free to let me know if you find something better
- AI chatbots. Started with [Anthropic Claude](https://claude.ai) and [Proton Lumo](https://lumo.proton.me) and [DuckDuckGo Duck](https://duck.ai) and now am also using [GitHub CoPilot](https://github.com/copilot)

## Release Notes

### 26 Nov 2025 | v 2.0
First development version in a very long time. I should have been keeping better track of versioning, but this one is a big release, so I've aptly numbered it 2.0.
#### Important notes, fixes, changes,additives:
- The previous site uses HTML, CSS, Bootstrap, local images, and mostly static content
- The new site uses these new technologies and tools: 
  - Vue.js, vanilla JavaScript (to replace Bootstrap) and dynamic content presentation
  - AI chatbots (noted in Languages and Tools Used above) to help teach me and fill in the gaps in my memory and knowledge
- New features:
  - Portfolio, Project & Education items are pulled from an array of objects to place the boxes arranged in a circle pattern (Vue.js)
  - Page view modes introduced, defaulting to dark mode but with options to choose light mode or monochrome mode for the whole page
  - Migrated from using locally stored images to links for images hosted on [Unsplash](https://unsplash.com/) - a free image site
  - Removed about 10,000 lines from .css file, making the site a tad faster to load and respond
  - Changed color schemes, and matching page modes
  - Added several element tags to be more compliant with WCAG 2.2 standards. This and page view modes to make my site a little more accessible for those using assistive technologies
  - Gallery modals for Portfolio items that link to external sites (when I get to building those sites, pages, section anchors)

#### Known Issues:
- Many of the links in the Portfolio section do not work because I haven't built those pages yet
- Contact form is not tested so I don't know if it works or not (afraid to try right now)
- Untested outside of my environment (MacOS, Safari)
