# 6. HTTP, TLS, Cookies, CORS and Reverse Proxies

## HTTP
HTTP is an application request/response protocol. A request has method, target, headers and optional body; response has status, headers and optional body.

Methods: GET is safe; PUT and DELETE are intended to be idempotent; POST is not inherently idempotent; PATCH semantics depend on operation. Idempotency keys can protect payment/order creation from duplicate effects after retries, if enforced and persisted server-side.

Status families: 2xx success, 3xx redirection, 4xx request/client issue, 5xx server/gateway issue. Common:
- 401 authentication required/failed.
- 403 understood but refused.
- 404 resource/route not found.
- 429 rate limited.
- 500 application error.
- 502 gateway got invalid/unavailable upstream response.
- 503 unavailable/overloaded.
- 504 gateway timed out waiting for upstream.

HTTP/1.1 and HTTP/2 commonly use TCP; HTTP/3 uses QUIC over UDP. HTTP/2 multiplexes streams but TCP loss can block delivery across streams on the shared TCP connection. QUIC provides independent stream delivery behavior.

## TLS
TLS provides confidentiality, integrity and authentication. A simplified TLS 1.3 exchange: ClientHello (versions, key shares, SNI), ServerHello, encrypted handshake messages/certificate/proof, Finished, then protected application data. SNI lets a client indicate hostname so a multi-host server can choose the right certificate/configuration. Certificate validation checks trust chain, hostname and validity period.

Command: `openssl s_client -connect example.com:443 -servername example.com`.

## Headers and cookies
`Content-Type` describes body type; `Accept` describes acceptable response types; `Authorization` carries credentials; `Set-Cookie` sets browser cookie; `Origin` identifies initiating origin; `Cache-Control` controls caching.

Cookie flags:
- HttpOnly blocks ordinary JavaScript access.
- Secure limits sending to secure transport.
- SameSite controls cross-site sending.
- Domain/Path scope cookie.
- Max-Age/Expires define lifetime.

HttpOnly reduces token theft via JS but does not remove XSS impact. SameSite helps with CSRF but is not the only defense.

## CORS
CORS is a browser-enforced cross-origin access policy, not authentication or a firewall. Certain requests trigger an OPTIONS preflight. Credentialed CORS needs an explicit origin; wildcard `*` cannot be used with credentialed browser access. curl and server-to-server clients are not restricted by browser CORS enforcement.

## Reverse proxy
A reverse proxy receives requests for backend services; it can terminate TLS, route paths/hosts, load balance and centralize logs.

Nginx example:

```nginx
location /api/ {
    proxy_pass http://backend:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

A trailing slash in `proxy_pass` changes URI rewriting. Express `trust proxy` must match known proxy hops; blindly trusting forwarded headers permits spoofing.

## Review
1. Why is retrying a POST payment request risky?
2. Why is CORS not authentication?
3. What is SNI?
4. Difference between 502 and 504?
