---
title: "DataPrime: Specifying Timestamp Formats"
date: "2023-09-06"
---

The DataPrime functions `parseTimestamp` and `formatTimestamp` accept a `format` argument that can be used to specify the format for parsing, respectively printing a timestamp to a string. The syntax is based on the `strftime` function from programming languages such as Python, C or Rust. It can be any valid string with embedded format specifiers as detailed in the table below. Other parts of the string which are not format specifiers are reproduced verbatim.

| **Specifier** | **Example** | **Meaning** |
| --- | --- | --- |
| **Dates** |  |  |
| %Y | 2023 | Full year since BCE, 4 digits |
| %G | 2023 | Week-based full year, 4 digits |
| %C | 20 | Century since BCE (first 2 digits of %Y) |
| %y | 23 | Short year since BCE (last 2 digits %Y) |
| %g | 23 | Week based short year (last 2 digits of %G) |
| %m | 08 | Month number (01-12), zero-padded to 2 digits |
| %B | August | Full month name |
| %b | Aug | Abbreviated month name, 3 letters |
| %h | Aug | Alias for %b |
| %d | 07 | Day of the month (01-31), zero-padded to 2 digits |
| %e | 7 | Alias for %\_d (space-padded) |
| %j | 219 | Day of the year (001-366), zero-padded to 3 digits |
| %u | 1 | Day of the week number, one-based (1-7), ISO 8601 |
| %w | 0 | Day of the week number, zero-based (0-6) |
| %A | Monday | Full day of the week |
| %a | Mon | Abbreviated day of the week, 3 letters |
| %U | 32 | Week number starting on Sunday (00-53), zero-padded to 2 digits |
| %W | 32 | Week number starting on Monday (00-53), zero-padded to 2 digits |
| %V | 32 | Week number according to ISO 8601 (01-53), zero-padded to 2 digits |
| %D | 08/07/23 | Alias for %m/%d/%y (”month / day / year”) |
| %F | 2023-08-01 | Alias for %Y-%m-%d (”year - month - day”), ISO 8601 |
| %v | 7-Aug-2023 | Alias for %e-%b-%Y (”day - month - year”) |
| **Time** |  |  |
| %H | 19 | Hours, 24-based (00-23), zero-padded to 2 digits |
| %k | 19 | Alias for %\_H (space-padded) |
| %I | 07 | Hours, 12-based (01-12), zero-padded to 2 digits |
| %l | 7 | Alias for %\_I (space-padded) |
| %P | pm | am or pm |
| %p | PM | AM or PM |
| %M | 06 | Minutes (00-59), zero-padded to 2 digits |
| %S | 42 | Seconds (00-60), 60 on leap seconds, zero-padded to 2 digits |
| %f | 82259081 | Nanoseconds since the last whole second |
| %3f | 082 | Milliseconds since the last whole second, zero-padded to 3 digits |
| %6f | 082259 | Microseconds since the last whole second, zero-padded to 6 digits |
| %9f | 082259081 | Nanoseconds since the last whole second, zero-padded to 9 digits |
| %.f | .082259081 | Fractional part of a second (3, 6 or 9 decimals) |
| %.3f | .082 | Fractional part of a second, 3 decimals (milliseconds) |
| %.6f | .082259 | Fractional part of a second, 6 decimals (microseconds) |
| %.9f | .082259081 | Fractional part of a second, 9 decimals (nanoseconds) |
| %R | 19:06 | Alias for %H:%M (”hours : minutes”) |
| %T | 19:06:42 | Alias for %H:%M:%S (”hours : minutes : seconds”) |
| **Time Zones** |  |  |
| %Z | Europe/Athens | Time zone identifier (only when parsing), IANA |
| %z | +0300 | Offset from UTC in hours and minutes, zero-padded to 4 digits |
| %:z | +03:00 | Offset from UTC in “hours : minutes” format |
| %::z | +03:00:00 | Offset from UTC in “hours : minutes : seconds” format |
| %:::z | +03 | Offset from UTC in hours, zero-padded to 2 digits |
| **Escaping** |  |  |
| %t | \\t | Literal tab character |
| %n | \\n | Literal new line character |
| %% | % | Literal percent sign |
| **Modifiers** |  |  |
| %-? | %-I → 7 | Disables padding |
| %\_? | %\_M → 6 | Pads with spaces |
| %0? | %0e → 07 | Pads with zeroes |

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/dataprime-query-language/">DataPrime Query Language</a></strong><br><strong><a href="https://coralogixstg.wpengine.com/docs/dataprime-cheat-sheet/">DataPrime Cheat Sheet</a></strong><br><strong><a href="https://coralogixstg.wpengine.com/docs/glossary-dataprime-operators/">Glossary: DataPrime Operators &amp; Expressions</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
