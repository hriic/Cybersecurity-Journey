# Django & SQL Injection — Study Notes

## Django fingerprinting

Django is a Python web framework. In training applications I learned to use clues such as csrfmiddlewaretoken, cookie behavior, URL patterns, and error responses as framework indicators.

A framework indicator guides investigation; it does not prove a vulnerability.

## CSRF concept

Cross-Site Request Forgery protection helps ensure that state-changing requests originate from an expected application/session context. Django commonly uses CSRF tokens.

The presence of CSRF protection does not protect against unrelated weaknesses such as SQL injection or broken authorization.

## Input-to-query reasoning

My SQL injection mental model is:

HTTP parameter → application code → query construction → database parser → database response.

The security problem appears when untrusted input can change the intended query structure.

## Error-based SQL injection

In a legal lab I studied MySQL error-based SQL injection, including how database-specific error behavior can become an information channel.

The reusable lesson is to recognize when input reaches a database context, whether syntax changes the response, whether errors identify the database technology, and whether the application exposes excessive database detail.

## Manual understanding before automation

My preferred learning workflow is:

identify input → capture exact request → establish normal response → test a small hypothesis → compare behavior → identify likely database technology → validate carefully within scope → use automation only when justified.

## SQLMap

SQLMap can automate authorized SQL injection testing, but it should not replace understanding the request.

Before using automation I want to understand which parameter is suspected, where the input appears, whether authentication/session state is required, whether CSRF state must be preserved, and what evidence supports the hypothesis.

## Defensive controls

Important controls include parameterized queries, safe query APIs, strict authorization, controlled error handling, least-privilege database accounts, secure secret management, and monitoring for abnormal input patterns.