---
title: Indention Wars
layout: post
published: false
---

Of all the things to debate in software, indention is curiously still one of the top five topics. Spacers and tabbers go back and forth, mostly in jest, and often it ends without any blood.

<blockquote class="twitter-video" data-lang="en"><p lang="en" dir="ltr">Tabs vs spaces<a href="https://t.co/FgGOgNH68x">https://t.co/FgGOgNH68x</a></p>&mdash; Practical Developer (@ThePracticalDev) <a href="https://twitter.com/ThePracticalDev/status/737185546020126720">May 30, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<br/>
Of all the reasons to go one way or another I'm actually a tabber for one reason and one reason only: I think everyone is entitled to their own opinion. Sure, tabs mean smaller file sizes, but I doubt anyone has ever cloned a git repository and said "This is taking forever! I bet they used spaces instead of tabs!" The one reason I think tabs are the right choice is because spaces leave room for flexibility which eventually means inconsistency. If you go with spaces then how many spaces? Two, four, or eight? Some lunatics even use three. Three! Using tabs eliminates that discussion. Users get to choose what their indents look like, no matter how ridiculous. I know some people who prefer two space indentions for everything. I like two spaces for JSON and that's about it. With everything else I go with four.

Whatever you choose in your project, be consistent. I think everyone can agree that mixed tabs and spaces are frustrating. Choose what you want and slap an [.editorconfig](http://editorconfig.org) file in the root of the project. Have your build system run a linter with the settings and fail the build for inconsistency. Yes, *fail*. If you don't fail the build then people will ignore it. Every editor has an editorconfig plugin so install it and all of the problems go away.

I applaud Go for [taking the discussion][gofmt] out of the hands of the users altogether.


  [gofmt]: https://golang.org/cmd/gofmt/

