
# urlscan.io Bug Bounty Cheat Sheet

## 1. Search Basics
- **Logical Operators**: You can use `AND`, `OR`, `NOT` to combine filters.
- **Grouping Terms**: Group search terms using parentheses `( )`.
  ```
  (page.domain:foo.com OR page.domain:bar.com)
  ```

## 2. Time Filter (Date)
- Limit search to a specific date range.
  - To search within the last 7 days:
    ```
    date:>now-7d
    ```
  - To search between specific dates:
    ```
    date:[2022-01-01 TO 2022-01-31]
    ```

## 3. URL Search
- Search for URLs starting with a specific prefix:
  ```
  page.url:"https://www.paypal.com/*"
  ```

## 4. Domain Search
- Find all scans related to a specific domain:
  ```
  domain:example.com
  ```
  - Find subdomains related to the domain:
    ```
    domain:*.example.com
    ```

## 5. IP Filter
- Search for scans related to a specific IP or range of IPs:
  ```
  page.ip:148.251.0.0/16 AND NOT page.ip:148.251.45.170
  ```
  - Find IP scans from a specific year:
    ```
    page.ip:(148.251.0.0/16 AND NOT 148.251.45.170) AND date:[2018 TO 2019]
    ```

## 6. Regex Search
- Use regular expressions to search specific patterns:
  ```
  page.domain:(/payp.*/ AND NOT paypal.com)
  ```
  - This search finds all domains starting with "payp", except "paypal.com".

## 7. Fuzzy Search
- Perform fuzzy searches for similar domains:
  ```
  page.domain:(paypal.com~ AND NOT paypal.com)
  ```

## 8. ASN or AS Name Search
- Search based on an Autonomous System Number (ASN) or AS name:
  ```
  page.asn:AS24940 OR page.asnname:hetzner
  ```

## 9. Sensitive Files Search
- Search for sensitive files like JSON or XML:
  ```
  page.url:".json" AND domain:<domain.com>
  ```
  - For specific paths, e.g., wp-content/uploads:
    ```
    page.url:"wp-content/uploads/" OR filename:"wp-content/uploads/" AND date:>now-1M
    ```

## 10. Hash Search
- Search for resources downloaded with a specific hash (e.g., SHA256):
  ```
  hash:d699f303...
  ```

## 11. Security Headers
- Find missing or incorrect security headers. For example, to find missing `X-Content-Type-Options` header:
  ```
  headers.security.x-content-type-options:!nosniff
  ```

## 12. GeoIP Country Search
- Filter scans based on the country of the request:
  ```
  geoip.country:<Country>
  ```

## 13. Finding Related Assets
- Find connections between domains and their related assets:
  ```
  domain:<domain.com> AND task.reportDomain:*.domain.com
  ```

## 14. Suspicious Requests
- Search for suspicious **POST** or **GET** requests:
  ```
  task.method:POST OR task.method:GET
  ```

## 15. iFrame Search
- Search for suspicious iFrames:
  ```
  page.url:"iframe" AND domain:<domain.com>
  ```

## 16. SVG File Uploads
- Search for SVG file uploads which could contain vulnerabilities:
  ```
  page.url:".svg" AND domain:<domain.com>
  ```

## 17. Exposed Emails
- Search for exposed email addresses on a page:
  ```
  page.content:"@example.com" AND domain:<domain.com>
  ```

## 18. Comparing Scans
- Compare scans to identify changes in content, structure, or resources.

## Important Notes:
- **Reserved Characters**: Escape reserved characters with backslash: `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`.
- **Case Sensitivity**: Field names are case sensitive.
