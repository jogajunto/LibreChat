# Deploy in Dokku

Create an application name using `librechat-<COMPANY_NAME>`.

## Add Traefik proxy
```bash
dokku proxy:set <APP_NAME> traefik
```

## Directives Traefik

- entrypoints http
```bash
-l traefik.http.routers.<APP_NAME>-web-internal-http.entrypoints="http"
-l traefik.http.routers.<APP_NAME>-web-internal-http.rule="Host(\`<APP_NAME>.<SERVER_ADDRESS>\`)"
-l traefik.http.routers.<APP_NAME>-web-internal-http.service="<APP_NAME>-web-internal-http"
```

- entrypoints https
```bash
-l traefik.http.routers.<APP_NAME>-web-internal-https.entrypoints="https"
-l traefik.http.routers.<APP_NAME>-web-internal-https.rule="Host(\`<APP_NAME>.<SERVER_ADDRESS>\`)"
-l traefik.http.routers.<APP_NAME>-web-internal-https.service="<APP_NAME>-web-internal-https"
```

- disable index application by search
```bash
-l traefik.http.middlewares.<APP_NAME>-robots.headers.customresponseheaders.X-Robots-Tag="noindex, nofollow, nosnippet, noarchive"
-l traefik.http.routers.<APP_NAME>-web-internal-https.middlewares="<APP_NAME>-robots"
```

- generate certified by internal application <APP_NAME>.<SERVER_ADDRESS>
```bash
-l traefik.http.routers.<APP_NAME>-web-internal-https.tls.certresolver="leresolver"
```

- set loadbalancer for application port
```bash
-l traefik.http.services.<APP_NAME>-web-internal-http.loadbalancer.server.port="<APP_PORT>"
-l traefik.http.services.<APP_NAME>-web-internal-https.loadbalancer.server.port="<APP_PORT>"
```