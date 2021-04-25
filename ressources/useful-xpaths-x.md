---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# useful xpath's x

```text

xpaths <- c("//meta[@name='viewport']/@content",
            "//meta[@charset]/@charset",
            "//link[@rel='canonical']/@href",
            "//link[@rel='alternate']/@href",
            "//link[@rel='alternate']/@hreflang",
            "//meta[starts-with(@property, 'og:')]/@property",
            "//meta[starts-with(@property, 'og:')]/@content",
            "//meta[starts-with(@name, 'twitter:')]/@name",
            "//meta[starts-with(@name, 'twitter:')]/@content",
            "//iframe/@src",
            "//script[contains(@src, 'googletagmanager.com/gtm.js?id=')]/@src",
            "//iframe[contains(@src, 'googletagmanager.com/ns.html?id=')]/@src",
            "//link[@rel]/@rel",
            "//link[@rel]/@href",
            "//link[@rel='stylesheet']/@href",
            "//link[contains(@href, '.css')]/@href",
            "//footer//a/text()",
            "//footer//a/@href",
            "//header//a/text()",
            "//header//a/@href",
            "//script[@type='text/javascript']/@src",
            "//script[@type='text/javascript']/text()",
            "//script//@src",
            "name(//link[@rel='canonical']/..)")



names <- c("viewport",
           "charset",
           "canonical",
           "alt_href",
           "alt_hreflang",
           "og_props",
           "og_content",
           "twtr_names",
           "twtr_content",
           "iframe_src",
           "gtm_script",
           "gtm_noscript",
           "link_rel_rel",
           "link_rel_href",
           "link_rel_stylesheet",
           "css_links",
           "footer_links_text",
           "footer_links_href",
           "header_links_text",
           "header_links_href",
           "js_script_src",
           "js_script_text",
           "script_src",
           "canonical_parent")

```

