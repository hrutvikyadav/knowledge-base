---
id: Hosting
aliases:
  - Hosting
tags: []
---

# Hosting ASP Net core webapi

## Publish app to folder

## Install appropriate runtime on prod server

## Replicate database

Make sure database exists on prod server, relations can be restored either:
- With `dump` file
- With migrations if sdk is available

> [!info] /home/hrutvik_/work/arb-asset-management/publish is working revision at this point

## Setup SSL certificate for server
> [!important] Without this step, requests can be made to the server only from something like POSTMAN App 
> If you want to use https, the ssl certificate must be trusted by server.

> [!hint] This step is complex, can be skipped by disabling `HttpsRedirection` in `Program.cs` for Development environment and setting `ASPNETCORE_ENVIRONMENT=Development` on the server.
> This enables the application to run on an insecure endpoint (http)

Visit your http endpoint from browser and make sure to check in Network logs that the request was **NOT** redirected
> This will confirm if the redirection was disabled for Development environment

## Access the app from your client

> [!info] /home/hrutvik_/work/arb-asset-management/publish-wohttps is working revision at this point

## Configure a reverse proxy server

### Nginx/
Requests are forwarded to our http server by the nginx reverse proxy, so we need to configure `ForwardedHeaders` Middleware before other middleware for our app

> [!info] Proxies running on the same server as the http server are trusted by default.
> If there is another proxy that handles requests between clients and our http server, add it to the list of `KnownProxies`
