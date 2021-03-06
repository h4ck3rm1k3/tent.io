## Tent Server-Server Communication Protocol

This document describes the protocol that Tent servers use to communicate with
each other.

###Basics

Every Tent user is represented by a server. Servers establish and maintain relationships and exchange content between users. 

Applications send new content to servers and receive content from the server.

Servers should always be online and be accessible over HTTPS.

### Server Discovery

Users and applications discover Tent servers in one of two ways: HTTP `Link` Headers and HTML `link` tags. Users can provide multiple links to access their Tent server. Multiple links are usually provided in case a particular address becomes unavailble. When multiple links are listed, they should be listed in order of preference from most preferred to least preferred, and contact attempted in the same order.


#### HTTP `Link` Header

The HTTP header allows discovery of Tent servers by performing a HEAD request
instead of retrieving the entire page and parsing for the `link` tag. It should be
added to all responses associated with the Tent entity. Other pages associated with the user may also serve the same header.


```text
Link: <https://tent.titanous.com/profile>; rel="https://tent.io/rels/profile"
```
or
```text
Link: <https://tent.titanous.com/profile>; rel="https://tent.io/rels/profile", <https://titanous.tent.is/profile>; rel="https://tent.io/rels/profile", <https://tent.jonathan.cloudmir.com/profile>; rel="https://tent.io/rels/profile"
```

#### HTML `link` tag

The `link` tag should be placed in the `head` tag of all HTML pages associated
with the Tent entity.

```html
<link href="https://tent.titanous.com/profile" rel="https://tent.io/rels/profile" />
```


### Follow an entity

To follow a user, send a POST request with acceptable licenses, post types, and views. Accepted requests will respond 200 OK and provide authentication credentials
### POST /followers

{create_follower example}


## Authentication


All requests should be made using SSL.

Authentication is required to access resources that are not marked as public.

Tent uses [MAC Access
Authentication](http://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-01)
for all requests.

All requests must be signed using MAC, and all notifications from the server
are signed as well.


## Get Current Following

Some details are available on who is following whom. A server can request information about a specific following, including your own current details, as specified below. Only your apps and the follower herself can see the specific details of their following.

### GET /followers/:id

{get_follower example}


## Edit Following

When a user wants to change the licenses, views, or post types she receives, she makes a PUT request.

### PUT /followers/:id

{update_follower example}


## Stop Following

If one user wants to stop following another, a DELETE request will end their relationship.

### DELETE /followers/:id

{delete_follower example}


## Fetch Posts

Following is not the only way to get posts or other content from a user. Anyone can request posts from another user with a GET request. If the requesting user is not authenticated, only public posts will be available. 

### GET /posts

There are a number of parameters available to limit the scope of the request.

{follower_get_posts example}

### GET /posts/:id

It is also possible to retrieve a single post by that post's id. This is useful in retrieving reposted content and 

{follower_get_post example}


## Notifications

New content is sent as a POST request to `/posts` and
authenticated using the negotiated credentials.
