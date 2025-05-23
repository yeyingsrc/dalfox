---
title: Result JSONL Format
redirect_from: /docs/jsonl/
nav_order: 5
parent: Resources
toc: true
layout: page
---

# Result JSONL Format
{: .d-inline-block }

New (v2.11.0) 
{: .label .label-blue }

This guide provides a detailed explanation of the JSONL (JSON Lines) format used for scan results generated by Dalfox. Understanding this format can help you better interpret the results and integrate them with other tools.

## JSONL Format Overview

JSONL (JSON Lines) is a convenient format for storing structured data that may be processed one record at a time. Each line is a valid JSON object, and lines are separated by a newline character. This makes it ideal for streaming and processing large datasets, as you can process each line independently.

## Scan Result

Here is an example of scan results in JSONL format:

```json
{"type":"R","inject_type":"inHTML","poc_type":"plain","method":"GET","data":"https://xss-game.appspot.com/level1/frame?query=%27%3E%3Ca+href%3Djavas%26%2399%3Bript%3Aalert%281%29%2Fclass%3Ddalfox%3Eclick","param":"query","payload":"'\u003e\u003ca href=javas\u0026#99;ript:alert(1)/class=dalfox\u003eclick","evidence":"13 line:  s were found for \u003cb\u003e'\u003e\u003ca href=javas\u0026#99;ript:alert(1)/class=dalfox\u003eclick\u003c/b\u003e. \u003ca","cwe":"CWE-79","severity":"Medium","message_id":174,"message_str":"Reflected Payload in HTML: query='\u003e\u003ca href=javas\u0026#99;ript:alert(1)/class=dalfox\u003eclick"}
{"type":"R","inject_type":"inHTML","poc_type":"plain","method":"GET","data":"https://xss-game.appspot.com/level1/frame?query=%27%22%3E%3Csvg%2Fonload%3D%26%2397%26%23108%26%23101%26%23114%26%2300116%26%2340%26%2341%26%23x2f%26%23x2f","param":"query","payload":"'\"\u003e\u003csvg/onload=\u0026#97\u0026#108\u0026#101\u0026#114\u0026#00116\u0026#40\u0026#41\u0026#x2f\u0026#x2f","evidence":"13 line:  s were found for \u003cb\u003e'\"\u003e\u003csvg/onload=\u0026#97\u0026#108\u0026#101\u0026#114\u0026#00116\u0026#40\u0026#41\u0026#x2f\u0026#x2f\u003c","cwe":"CWE-79","severity":"Medium","message_id":242,"message_str":"Reflected Payload in HTML: query='\"\u003e\u003csvg/onload=\u0026#97\u0026#108\u0026#101\u0026#114\u0026#00116\u0026#40\u0026#41\u0026#x2f\u0026#x2f"}
{"type":"V","inject_type":"inHTML","poc_type":"plain","method":"GET","data":"https://xss-game.appspot.com/level1/frame?query=%3C%2FScriPt%3E%3CsCripT+id%3Ddalfox%3Ealert%281%29%3C%2FsCriPt%3E","param":"query","payload":"\u003c/ScriPt\u003e\u003csCripT id=dalfox\u003ealert(1)\u003c/sCriPt\u003e","evidence":"13 line:  s were found for \u003cb\u003e\u003c/ScriPt\u003e\u003csCripT id=dalfox\u003ealert(1)\u003c/sCriPt\u003e\u003c/b\u003e. \u003ca href='?","cwe":"CWE-79","severity":"High","message_id":162,"message_str":"Triggered XSS Payload (found DOM Object): query=\u003c/ScriPt\u003e\u003csCripT id=dalfox\u003ealert(1)\u003c/sCriPt\u003e"}
```

## PoC

Here is a detailed explanation of the PoC (Proof of Concept) section in the JSON result:

```json
{
      "type":"Type of PoC (G/R/V)",
      "inject_type":"Injected Point",
      "poc_type":"plain/curl/httpie/etc...",
      "method":"HTTP Method",
      "data":"PoC URL",
      "param":"Parameter",
      "payload":"Attack Value",
      "evidence":"Evidence with response body",
      "cwe":"CWE ID",
      "severity": "Severity (Low/Medium/High)",
      "message_id": "Message ID",
      "message_str": "Message String (POC)",
      "raw_request": "Raw HTTP Request (require --output-request flag)",
      "raw_response": "Raw HTTP Response (require --output-response flag)"
}
```

### Explanation of Fields

| Key           | Description                 | List                                                         |
| ------------- | --------------------------- | ------------------------------------------------------------ |
| type          | Type                        | - G (Grep)<br />- R (Reflected)<br />- V (Verified)          |
| inject_type   | Injected point              | - inHTML-none (Injected in HTML area)<br />- inJS-none (Injected in Javascript area)<br />- inJS-double (Injected within `"` in Javascript area)<br />- inJS-single (Injected within `'` in Javascript area)<br />- inJS-backtick (Injected within backtick in Javascript area)<br />- inATTR-none (Injected within in Tag attribute area)<br />- inATTR-double (Injected within `"` in Tag attribute area)<br />- inATTR-single (Injected within `'` in Tag attribute area) |
| poc_type      | Type of PoC code            | - plain (URL)<br />- curl (Curl command)<br />- httpie (HTTPie command) |
| method        | HTTP Method                 | - GET/POST/PUT/DELETE, etc.                                  |
| data          | PoC (URL)                   | - PoC URL                                                    |
| param         | Parameter name              | - Weak parameter name                                        |
| payload       | Parameter value             | - Attack code in value                                       |
| evidence      | Evidence with response body | - Simple code view of where it's injected in response body.  |
| cwe           | CWE ID                      | - Mapping CWE ID                                             |
| severity      | Severity                    | - Severity (Low/Medium/High)                                 |
| raw_request   | Raw HTTP Request            | - Raw HTTP Request                                           |
| raw_response  | Raw HTTP Response           | - Raw HTTP Response                                          |

### Example PoC

```json
{
    "type": "V",
    "inject_type": "inHTML-URL",
    "poc_type": "plain",
    "method": "GET",
    "data": "http://testphp.vulnweb.com/listproducts.php?cat=%27%22%3E%3Cimg%2Fsrc%2Fonerror%3D.1%7Calert%60%60+class%3Ddalfox%3E",
    "param": "cat",
    "payload": "'\"><img/src/onerror=.1|alert`` class=dalfox>",
    "evidence": "48 line:  syntax to use near ''\"><img/src/onerror=.1|alert`` class=dalfox>' at line 1",
    "cwe": "CWE-79",
    "severity": "High"
}
```
