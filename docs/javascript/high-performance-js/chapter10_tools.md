## Chapter 10 - Tools

This chapter focuses on some of the free tools available for:

- Profiling
  Timing various functions and operations during script execution to identify areas for optimization

- Network analysis
  Examining the loading of images, stylesheets, and scripts and their effect on overall page load and rendering

### 1. JavaScript Profiling

### 2. Firefox - firebug add-on

- console panel
- console API
- Net panel

### 3. IE Developer tools

### 4. Safari Web Inspector

- profiles panel
- resources panel

### 5. Chrome Developer Tools ❤️

### 6. PageSpeed

Page Speed is a tool initially developed for internal use at Google and later released as a Firebug addon that, like Firebug’s Net panel, provides information about the resources being loaded on a web page

Visit http://code.google.com/speed/page-speed/ for installation instructions and other product details.

### 7. YSlow

The YSlow tool provides performance insights into the overall loading and execution of the initial page view. This tool was originally developed internally at Yahoo! by Steve Souders as a Firefox addon (via GreaseMonkey). It has been made available to the public as a Firebug addon, and is maintained and updated regularly by Yahoo! developers. Visit http://developer.yahoo.com/yslow/ for installation instructions and other product details.

### 8. Summary

- Use a network analyzer to identify bottlenecks in the loading of scripts and other page assets; this helps determine where script deferral or profiling may be needed.

- Although conventional wisdom says to minimize the number of HTTP requests, **deferring scripts** whenever possible allows the page to render more quickly, providing users with a better overall experience.

- Use a profiler to identify slow areas in script execution, examining the time spent in each function, the number of times a function is called, and the callstack itself provides a number of clues as to where optimization efforts should be focused.

- Although time spent and number of calls are usually the most valuable bits of data, looking more closely at how functions are being called might yield other optimization candidates.
