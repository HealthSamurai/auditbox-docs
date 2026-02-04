---
description: Comprehensive test of all markdown features and GitBook directives
---

# Markdown Features Test

This page demonstrates all supported markdown features and GitBook directives.

## Basic Text Formatting

Regular paragraph text. This is how normal content appears with enough text to show line wrapping and paragraph spacing behavior.

**Bold text** and *italic text* and ***bold italic text***. Also ~~strikethrough text~~.

`Inline code` looks like this within a paragraph.

## Headings

### Level 3 Heading

#### Level 4 Heading

##### Level 5 Heading

###### Level 6 Heading

## Links

- [Internal link to Getting Started](getting-started/README.md)
- [External link to Health Samurai](https://health-samurai.io)
- [Link with title](https://aidbox.io "Aidbox FHIR Platform")

## Lists

### Unordered List

- First item
- Second item
  - Nested item 2.1
  - Nested item 2.2
    - Deeply nested 2.2.1
- Third item

### Ordered List

1. First step
2. Second step
   1. Sub-step 2.1
   2. Sub-step 2.2
3. Third step

### Task List (Checkboxes)

- [x] Completed task
- [ ] Incomplete task
- [x] Another completed task

## Blockquotes

> This is a blockquote. It can span multiple lines and contain **formatted** text and `code`.
>
> Multiple paragraphs work too.

> Nested blockquotes:
> > Can be nested
> > > Even deeper

## Code Blocks

### Inline Code

Use `SELECT * FROM users` to query all users. Variables like `$POSTGRES_URL` are also styled.

### Fenced Code Blocks

```sql
-- SQL example with syntax highlighting
SELECT
  ae.id,
  ae.resource->>'type' AS event_type,
  ae.ts AS timestamp
FROM auditevent ae
WHERE ae.resource @> '{"outcome": "0"}'
ORDER BY ae.ts DESC
LIMIT 10;
```

```typescript
// TypeScript example
interface AuditEvent {
  id: string;
  type: string;
  recorded: Date;
  outcome: "0" | "4" | "8" | "12";
}

async function fetchEvents(): Promise<AuditEvent[]> {
  const response = await fetch("/api/AuditEvent");
  return response.json();
}
```

```bash
# Bash example
curl -X POST https://auditbox.example.com/fhir/AuditEvent \
  -H "Content-Type: application/fhir+json" \
  -d @audit-event.json
```

```json
{
  "resourceType": "AuditEvent",
  "type": {
    "system": "http://dicom.nema.org/resources/ontology/DCM",
    "code": "110114",
    "display": "User Authentication"
  },
  "recorded": "2024-01-15T10:30:00Z",
  "outcome": "0"
}
```

```yaml
# YAML example
apiVersion: v1
kind: ConfigMap
metadata:
  name: auditbox-config
data:
  POSTGRES_URL: "postgresql://user:pass@localhost/auditbox"
  LOG_LEVEL: "info"
```

## Tables

| Column 1 | Column 2 | Column 3 |
|----------|:--------:|---------:|
| Left aligned | Center aligned | Right aligned |
| Cell A | Cell B | Cell C |
| Longer content here | Short | 12345 |

### Complex Table

| Event Type | Description | Outcome Codes |
|------------|-------------|---------------|
| User Login | Authentication attempt | `0` = Success, `4` = Minor failure |
| Data Access | Record viewed/modified | `0` = Success, `8` = Serious failure |
| System Event | Application events | `12` = Major failure |

## Images

![Auditbox Dashboard](../assets/ui_main.png)

Small inline reference to an image.

## Horizontal Rule

Content above the rule.

---

Content below the rule.

---

## Mixed Content Example

This section demonstrates a realistic documentation pattern: explanatory text followed by code, then more text, and finally tabbed examples.

Auditbox stores audit events in FHIR-compatible format. To query events from the last 24 hours, use the following API call:

```bash
curl -X GET "https://auditbox.example.com/fhir/AuditEvent?date=gt2024-01-15" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Accept: application/fhir+json"
```

The response contains a FHIR Bundle with matching AuditEvent resources. Each event includes the actor, action, outcome, and timestamp. You can filter by additional parameters like `agent`, `entity`, or `outcome`.

{% tabs %}
{% tab title="Filter by Outcome" %}
Filter for successful events only:

```bash
curl -X GET "https://auditbox.example.com/fhir/AuditEvent?outcome=0" \
  -H "Authorization: Bearer $TOKEN"
```

Outcome codes: `0` = Success, `4` = Minor failure, `8` = Serious failure, `12` = Major failure.
{% endtab %}

{% tab title="Filter by Agent" %}
Filter events by a specific user:

```bash
curl -X GET "https://auditbox.example.com/fhir/AuditEvent?agent=User/john-doe" \
  -H "Authorization: Bearer $TOKEN"
```

The `agent` parameter accepts a reference to the user resource.
{% endtab %}

{% tab title="Pagination" %}
Use `_count` and `_offset` for pagination:

```bash
curl -X GET "https://auditbox.example.com/fhir/AuditEvent?_count=50&_offset=100" \
  -H "Authorization: Bearer $TOKEN"
```

The default page size is 20. Maximum allowed is 1000.
{% endtab %}
{% endtabs %}

---

## GitBook Directives

### Hint Boxes

{% hint style="info" %}
**Info hint**: This is useful information that helps users understand a concept.
{% endhint %}

{% hint style="success" %}
**Success hint**: Operation completed successfully! Your AuditEvents are now being recorded.
{% endhint %}

{% hint style="warning" %}
**Warning hint**: Be careful with this operation. Make sure you have backups before proceeding.
{% endhint %}

{% hint style="danger" %}
**Danger hint**: This action cannot be undone. All data will be permanently deleted.
{% endhint %}

### Tabs

{% tabs %}
{% tab title="Docker" %}
Run Auditbox using Docker:

```bash
docker run -d \
  --name auditbox \
  -p 8080:8080 \
  -e POSTGRES_URL=postgresql://user:pass@host/db \
  healthsamurai/auditbox:latest
```

This starts Auditbox on port 8080.
{% endtab %}

{% tab title="Kubernetes" %}
Deploy to Kubernetes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: auditbox
spec:
  replicas: 3
  template:
    spec:
      containers:
        - name: auditbox
          image: healthsamurai/auditbox:latest
          ports:
            - containerPort: 8080
```

Apply with `kubectl apply -f auditbox.yaml`.
{% endtab %}

{% tab title="Binary" %}
Download and run the binary:

```bash
# Download
curl -L https://releases.auditbox.io/latest/auditbox-linux-amd64 -o auditbox

# Make executable
chmod +x auditbox

# Run
./auditbox serve --port 8080
```
{% endtab %}
{% endtabs %}

### Tabs with Hints Inside

{% tabs %}
{% tab title="Development" %}
{% hint style="info" %}
Development mode enables hot-reload and verbose logging.
{% endhint %}

```bash
AUDITBOX_ENV=development ./auditbox serve
```
{% endtab %}

{% tab title="Production" %}
{% hint style="warning" %}
Production mode requires proper TLS configuration.
{% endhint %}

```bash
AUDITBOX_ENV=production \
TLS_CERT=/path/to/cert.pem \
TLS_KEY=/path/to/key.pem \
./auditbox serve
```
{% endtab %}
{% endtabs %}

### Stepper (Step-by-Step Guide)

{% stepper %}
{% step %}
### Install Prerequisites

Make sure you have Docker installed:

```bash
docker --version
```

{% hint style="info" %}
Docker 20.10+ is required for multi-platform support.
{% endhint %}
{% endstep %}

{% step %}
### Configure Database

Create a PostgreSQL database:

```sql
CREATE DATABASE auditbox;
CREATE USER auditbox WITH PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE auditbox TO auditbox;
```
{% endstep %}

{% step %}
### Start Auditbox

```bash
docker run -d --name auditbox \
  -e POSTGRES_URL=postgresql://auditbox:secure_password@localhost/auditbox \
  -p 8080:8080 \
  healthsamurai/auditbox:latest
```
{% endstep %}

{% step %}
### Verify Installation

Open http://localhost:8080 in your browser. You should see the Auditbox dashboard.

{% hint style="success" %}
Congratulations! Auditbox is now running.
{% endhint %}
{% endstep %}
{% endstepper %}

### Code Block with Title

{% code title="docker-compose.yml" %}
```yaml
version: "3.8"
services:
  auditbox:
    image: healthsamurai/auditbox:latest
    ports:
      - "8080:8080"
    environment:
      POSTGRES_URL: postgresql://postgres:postgres@db/auditbox
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: auditbox
      POSTGRES_PASSWORD: postgres
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```
{% endcode %}

### Embed (YouTube Video)

{% embed url="https://www.youtube.com/watch?v=dQw4w9WgXcQ" %}
Example video embed
{% endembed %}

### Content Reference

{% content-ref %}
[Getting Started Guide](getting-started/README.md)
{% endcontent-ref %}

### File Download

{% file src="../assets/arch.png" %}
Download architecture diagram
{% endfile %}

## Combined Features

### Complex Example

{% hint style="info" %}
The following example shows how to configure **multiple** features:

1. Database connection
2. Authentication
3. Logging

See the [configuration reference](configuration/envs.md) for all options.
{% endhint %}

{% tabs %}
{% tab title="Environment Variables" %}
| Variable | Required | Description |
|----------|:--------:|-------------|
| `POSTGRES_URL` | Yes | Database connection string |
| `AUTH_PROVIDER` | No | OIDC provider URL |
| `LOG_LEVEL` | No | `debug`, `info`, `warn`, `error` |

```bash
export POSTGRES_URL="postgresql://user:pass@localhost/auditbox"
export AUTH_PROVIDER="https://auth.example.com"
export LOG_LEVEL="info"
```
{% endtab %}

{% tab title="Config File" %}
```yaml
database:
  url: postgresql://user:pass@localhost/auditbox
  pool_size: 10

auth:
  provider: https://auth.example.com
  client_id: auditbox

logging:
  level: info
  format: json
```
{% endtab %}
{% endtabs %}

## Mermaid Diagram

```mermaid
graph TD
    A[Start] --> B{Decision}
    B -->|Yes| C[OK]
    B -->|No| D[End]
```

## Edge Cases

### Very Long Code Line

```bash
curl -X POST "https://api.auditbox.example.com/fhir/AuditEvent?_format=json&_pretty=true&_summary=false" -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0" -H "Content-Type: application/fhir+json" -d '{"resourceType":"AuditEvent"}'
```

### Empty Code Block

```
```

### Deeply Nested List

1. Level 1
   1. Level 2
      1. Level 3
         1. Level 4
            1. Level 5

### Special Characters

Characters that need escaping: `<div>`, `&amp;`, "quotes", 'apostrophes'.

HTML entities: &copy; &reg; &trade; &mdash; &rarr;

### Math-like Content

Formula: E = mc² (Unicode superscript)

Fractions: ½ ¼ ¾

### Unicode

Emoji: 🚀 ✅ ⚠️ 🔒

International: 日本語 中文 한국어 العربية

## Math (KaTeX)

Inline math: The equation $E = mc^2$ is famous.

Display math:

$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$

Quadratic formula: $x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$

## Embed

{% embed url="https://github.com/Aidbox/examples/tree/main/aidbox-features/aidbox-with-ro-replica" / %}

---

*End of markdown features test*
