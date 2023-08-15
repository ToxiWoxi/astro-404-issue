# Issue: Changes to 404 handling have resulted in server-rendered 404 pages no longer working on Cloudflare.
## Behaviors in different versions
### Expected behavior:
1) The 404 page loads when going to a non-existent route
2) The 404 page can perform redirects to case-corrected versions of pages or aliases will redirect to correct page
    - Examples: 
        - `/eXaMPLe` can be redirected to `/example`
        - `/index` can be redirected to `/`
- These behaviours are tested to work in Astro v2.9.6 with Cloudflare adapter v6.6.2
### Updated behavior with Astro v2.9.7 and higher
1) Cloudflare sends a default 404 error with no page when accessing a non-existent route
2) The 404 page still can be accessed from /404
3) Case-corrections and aliases will not be redirected
- These behaviours are tested to work in Astro v2.9.7 and higher with Cloudflare adapter v6.6.2
### Updated behavior with Cloudflare adapter v6.7.0 and higher
1) Cloudflare returns with the contents of the index route without redirecting or updating the url when accessing a non-existent route
2) The 404 page still can be accessed from /404
3) Case-corrections and aliases still will not be redirected
- These behaviours are tested to work in Astro v2.10.5 and higher with Cloudflare adapter v6.7.0 and higher and is the current behavior on latest versions of both.
### This issue does not happen on latest when prerendering the 404 page
However, this doesn't allow you to use the 404 page to do case-corrections and handle aliases
- The `latest-prerendered` branch dedicated to this behavior
## Public links
`Links will be put here when deployments for each version are done`
## Testing yourself
You can test this on your own by cloning this repo. There is a branch for each of the versions mentioned above.
### Instructions
1) Clone this repo 
2) Run `npm install`
3) Run `npm run preview`
    - I've changed this to build then preview in wrangler
4) Open the preview link in your browser
- Ideal behavior:
    - `/` - The index
    - `/inDEx` - redirects to `/`
    - `/404` - The 404 page
    - `/literallyanyotherpage` - renders the 404 page
    
***When changing versions, make sure to run `npm install` again before previewing***
### Branches and their behaviors:
| Branch                          | Bad Routes            | Aliases                   | Case-correction |
|---------------------------------|-----------------------|---------------------------|-----------------|
| `astro@2.9.6`                   | Renders `404.astro`   | Working                   | Working         |
| `astro@2.9.7`                   | Returns nothing       | Configured redirects only | Not working     |
| `astro@2.10.5-cloudflare@6.6.2` | Returns nothing       | Configured redirects only | Not working     |
| `astro@2.10.5-cloudflare@6.7.0` | Returns `index.astro` | Configured redirects only | Not working     |
| latest                          | Returns `index.astro` | Configured redirects only | Not working     |
| latest-prerendered              | Renders `404.astro`   | Configured redirects only | Not working     |