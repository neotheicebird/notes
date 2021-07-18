## 1 - History Mode

The default routing mode is `hash`. In hash mode, a URL looks like https://mywebsite.com/#/user/@neotheicebird

This `hash` has nothing to do with cryptography, its just the character `#` and browsers ignore them. i.e anything starting from `#/...` doesn't exist to the browser and the route is not reloaded.

You can load up any page on the internet, say `https://www.programmersought.com/article/96443919199` and try adding a `#/1234` to the end of the URL and press enter. Nothing happens. But if you add say `/user/1234`, that changes the page, you might see some 404 page.

This caveat is neatly used by vue-router, to route SPAs. While there isn't anything inherently wrong with hash mode, its a bit ugly. 

The other mode we have is `history`. This uses the `history` API which browser provides and the URLs look great, like `https://mywebsite.com/user/@neotheicebird`. So why not use this from start? 

The main issue is, this needs server support. If a user loads `https://mywebsite.com` and navigates to user view, things work well here as well, but if they have bookmarked the user page URL and try to directly load it like we do normally using the internet, we get a 404 page. So we must add to the server a simple catch-all fallback route to your server. If the URL doesn't match any static assets, it should serve the same `index.html` page that your app lives in. Beautiful, again!

To do this for Amazon S3 and Cloudfront, we can add a Customer Error Response and redirect user to index.html. Ref: https://stackoverflow.com/questions/43095823/vue-js-router-history-mode-and-aws-s3-routingrules

