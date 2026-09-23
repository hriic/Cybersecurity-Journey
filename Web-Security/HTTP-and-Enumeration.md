# HTTP and Web Enumeration

When I encounter a web service in an authorized lab, I start by understanding what is actually running.

## Things I have practiced checking

- HTTP title
- Server information
- Headers
- robots.txt
- Application/version information
- Directories and files
- Login functionality
- Supported HTTP methods

## Tools

I use curl for quick HTTP inspection, Gobuster for content discovery, and Burp Suite to inspect requests and responses in training applications.

The important part is understanding why a discovered path, header or request matters instead of treating enumeration as a list of commands.