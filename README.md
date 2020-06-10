Project 1
================
Ciara Whipp
6/10/2020

## Introduction to JSON Data

### What is JSON and what is its use?

JavaScript Object Notation or
[JSON](https://www.tutorialspoint.com/json/json_overview.htm#:~:text=JSON%20or%20JavaScript%20Object%20Notation,stands%20for%20JavaScript%20Object%20Notation.)
is a text-based, data-interchangable format used to store and transport
data. It is often used to transmit data between a server and web
applications, and web services and APIs use JSON to provide data. We
will explore JSON provided by an API in this document.

### Why use JSON?

JSON is language independent, so major programming languages are cable
of handling JSON data - they can get data out of JSON and turn data into
JSON. The intuitive nature of the syntax is easy to read and write, and
since the syntax is derived from and similar to existing programming
languages, it is easy for machines to generate and parse JSON data. This
[video](https://www.youtube.com/watch?v=0IoG-mSvWSo) provides a brief
overview of JSON data: what it is, advantages to JSON, and an
introduction to JSON syntax.

### R Packages to Accomodate JSON Data

There are three major R packages used to deal with JSON data:
`jsonlite`, `rjson`, and `RJSONIO`.  
Some helpful functions included in all three packages are `fromJSON()`
to convert JSON data into an R object, `toJSON()` to convert an R object
into JSON. Addtionally, `flatten` is a useful argument that creates a
single non-nested data frame from a nested data frame (common in JSON
data retrieved from the web). I will choose to use `jsonlite` since the
R documentation for this package is more robust than the doucmentation
for the other two packages.
