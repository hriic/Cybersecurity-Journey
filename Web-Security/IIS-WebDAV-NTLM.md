# IIS, WebDAV & NTLM — Detailed Study Notes

> Notes from authorized training environments. The focus is understanding protocols, configuration, evidence, and defensive implications.

## IIS

Microsoft Internet Information Services (IIS) is Microsoft's web-server platform. IIS can serve static content and host server-side applications including ASP.NET.

A header such as Server: Microsoft-IIS/10.0 is useful fingerprinting evidence. It identifies technology information but does not itself prove a vulnerability.

## ASP.NET

ASP.NET applications can execute server-side code through the IIS application pipeline. This makes file permissions, upload locations, handler mappings, and execution permissions important parts of secure configuration.

## WebDAV

WebDAV extends HTTP with remote content-management capabilities. Methods I studied include GET, PUT, DELETE, OPTIONS, PROPFIND, and MKCOL.

WebDAV itself is not a vulnerability. Risk appears when unnecessary functionality, weak access control, exposed information, and unsafe execution permissions combine.

## HTTP method analysis

OPTIONS can reveal supported methods and capabilities. PROPFIND is associated with WebDAV resource properties. PUT can create a resource when the server permits it.

A successful creation response such as 201 Created proves that a resource was created. It does not by itself prove that the resource can execute code.

## Understanding 401 Unauthorized

A 401 response is useful evidence. It can indicate that the server and path responded and that authentication is required. Authentication headers may also reveal the supported authentication scheme.

A 401 response does not mean the authentication is weak.

## NTLM

NTLM is a Microsoft challenge-response authentication mechanism.

I keep these concepts separate:

- NTLM authentication — challenge-response authentication.
- NTLM credential material/hash — password-derived credential representation.
- Pass-the-Hash — authentication using compatible hash material in a supported context.
- NTLM Relay — forwarding an authentication exchange under specific conditions.

They are related concepts but not synonyms.

## IIS 8.3 / tilde enumeration concept

Legacy Windows filesystems can support 8.3 short filenames. Some IIS configurations historically produced observable response differences for specially formed short-name requests containing a tilde.

The reusable lesson is response-difference enumeration:

candidate input → compare response → infer whether a naming pattern may exist → verify the real resource independently.

This is an information-disclosure concept. A candidate short name is not enough; the inferred resource must be verified.

## Authentication, write access, and execution are separate facts

One of the most important lessons from the lab was to separate each claim:

1. Does the path exist?
2. Is authentication required?
3. Do valid credentials grant access?
4. Is write functionality permitted?
5. Was a resource actually created?
6. Is the resource reachable?
7. How does the server handle that file type?
8. Under which process identity does the application run?

This prevents overstating impact.

## IIS process context

IIS commonly runs application code inside worker processes such as w3wp.exe and application-pool identities. If a security test demonstrates server-side execution, the current identity and privileges determine the real impact.

The methodology is:

current identity → assigned privileges → operating-system/service context → possible security impact → verify prerequisites.

## Defensive indicators

Useful defensive signals include unusual WebDAV methods, unexpected file creation in web directories, executable server-side file types in upload locations, repeated short-name enumeration patterns, suspicious authentication activity, and unusual child processes originating from IIS worker processes.

## Main lesson

The strongest lesson was attack-chain reasoning. IIS, WebDAV, NTLM, ASP.NET, write permissions, and information disclosure are individual technologies or conditions. Security impact depends on how they are configured and how the conditions connect together.