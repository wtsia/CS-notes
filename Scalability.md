# Scalability: A Summary

Qualities to look for in a hosting website: SFTP, caution of 'unlimited' offers

Shared Web-Host:
VPS: Virtual Private Servers
-Takes

## Clones
*Golden Rule of Scalability*: every server contains exactly the same codebase and does not store any user-related data, like sessions or  profile pictures, on local disc or memory

- sessions should be stored in centralized, accessible database or persistent cache like Redis (better perf. than external database, or db near the data center of app servers)

- in deployment, solving the issue of applying code changes to multiple servers can be solved by Capistrano (requires some learning esp if not into Ruby on Rails)

-when codebases are uniform, you can create an image (Amazon Machine Image for AWS) as a 'super clone' from which to draw from

## Database
