---
created: 2024-09-05T21:19:23 (UTC -03:00)
tags: []
source: https://damienbod.com/2024/08/12/add-a-swagger-ui-using-a-net-9-json-openapi-file/
author: damienbod
---

# Add a Swagger UI using a .NET 9 Json OpenAPI file | Software Engineering

> ## Excerpt
> This post shows how to implement a Swagger UI using a .NET 9 produced OpenAPI file. The Swagger UI is deployed to a secure or development environment and is not deployed to a public production targ…

---
`namespace` `WebApiOpenApi;`

`public` `static` `class` `SecurityHeadersDefinitions`

`{`

    `public` `static` `HeaderPolicyCollection GetHeaderPolicyCollection(``bool` `isDev)`

    `{`

        `var` `policy =` `new` `HeaderPolicyCollection()`

            `.AddFrameOptionsDeny()`

            `.AddContentTypeOptionsNoSniff()`

            `.AddReferrerPolicyStrictOriginWhenCrossOrigin()`

            `.AddCrossOriginOpenerPolicy(builder => builder.SameOrigin())`

            `.AddCrossOriginEmbedderPolicy(builder => builder.RequireCorp())`

            `.AddCrossOriginResourcePolicy(builder => builder.SameOrigin())`

            `.RemoveServerHeader()`

            `.AddPermissionsPolicy(builder =>`

            `{`

                `builder.AddAccelerometer().None();`

                `builder.AddAutoplay().None();`

                `builder.AddCamera().None();`

                `builder.AddEncryptedMedia().None();`

                `builder.AddFullscreen().All();`

                `builder.AddGeolocation().None();`

                `builder.AddGyroscope().None();`

                `builder.AddMagnetometer().None();`

                `builder.AddMicrophone().None();`

                `builder.AddMidi().None();`

                `builder.AddPayment().None();`

                `builder.AddPictureInPicture().None();`

                `builder.AddSyncXHR().None();`

                `builder.AddUsb().None();`

            `});`

        `AddCspHstsDefinitions(isDev, policy);`

        `policy.ApplyDocumentHeadersToAllResponses();`

        `return` `policy;`

    `}`

    `private` `static` `void` `AddCspHstsDefinitions(``bool` `isDev, HeaderPolicyCollection policy)`

    `{`

        `if` `(!isDev)`

        `{`

            `policy.AddContentSecurityPolicy(builder =>`

            `{`

                `builder.AddObjectSrc().None();`

                `builder.AddBlockAllMixedContent();`

                `builder.AddImgSrc().None();`

                `builder.AddFormAction().None();`

                `builder.AddFontSrc().None();`

                `builder.AddStyleSrc().None();`

                `builder.AddScriptSrc().None();`

                `builder.AddBaseUri().Self();`

                `builder.AddFrameAncestors().None();`

                `builder.AddCustomDirective(``"require-trusted-types-for"``,` `"'script'"``);`

            `});`

            `policy.AddStrictTransportSecurityMaxAgeIncludeSubDomains(maxAgeInSeconds: 60 * 60 * 24 * 365);`

        `}`

        `else`

        `{`

            `policy.AddContentSecurityPolicy(builder =>`

            `{`

                `builder.AddObjectSrc().None();`

                `builder.AddBlockAllMixedContent();`

                `builder.AddImgSrc().Self().From(``"data:"``);`

                `builder.AddFormAction().Self();`

                `builder.AddFontSrc().Self();`

                `builder.AddStyleSrc().Self().UnsafeInline();`

                `builder.AddScriptSrc().Self().UnsafeInline();`

                `builder.AddBaseUri().Self();`

                `builder.AddFrameAncestors().None();`

            `});`

        `}`

    `}`

`}`
