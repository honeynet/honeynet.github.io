## The Honeynet Project Website

This repository houses the next version of https://honeynet.org/. 
The main objective is to migrate from Wordpress to a static site generator (Hugo) and to make the site more maintainable.

### Run site locally

1. Clone this repository.
2. Get the latest version of Hugo extended from https://github.com/gohugoio/hugo/releases/. [^hugo-install]  
   **You need the _Hugo extended_ binaries**, which are typically hidden behind _"Show all ... assets"_.
3. Run `hugo server` in the root directory of this repository.
4. Point your browser to <http://localhost:1313>. 

[^hugo-install]: See https://gohugo.io/installation/ for more detailed installation instructions.

### Migrating content

- To keep old URLs working, we can specify `url: ...` in the page's front matter: https://gohugo.io/content-management/urls/#url
- If you want to add particular styling/structure to static pages, we're using Bootstrap 5.3: https://getbootstrap.com/

### Adding content

To add a new blog post, run the following command in the root directory of this repository:

```
hugo new blog/YYYY/MM/DD/some-title/index.md
```

You can then edit the newly created file with your favorite text editor, and
add images/resources next to it.

### Contributing

Contributions are super welcome! A lot of stuff can be done by pretty much anyone, it just needs to get done!

#### TODO (necessary for replacing the existing website)

 - [x] Setup Hugo scaffolding. (@mhils)
 - [x] Write a new template from scratch to get rid of the existing cruft. (@mhils)
 - [x] Setup CI to automatically build the site to https://honeynet.github.io/. (@mhils)
 - [x] Add docs for how to add a blog post. (@mhils)
 - [x] Migrate existing content
   - [x] Front page (remove hot topics + active projects for now?)
   - [x] About Us
     - [x] About Us
     - [x] CoC
     - [x] Funding (needs minor updates)
     - [x] Papers
   - [x] News (what can we do with auto-migration here?)
   - [ ] GSoC
   - [x] Projects
   - [x] FAQ (@mhils)
   - [x] Workshops (@mhils)
   - [x] Challenges (partially broken on the current website, update with a pointer to https://github.com/honeynet/forensic_challenges)
 - [x] Fix social icons in the footer. (@mhils)

#### TODO (stretch goals)

 - Can we do something smart about "active projects"? Scrape activity stats from GitHub maybe?
 - Write new blog entries.
