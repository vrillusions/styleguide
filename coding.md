# Coding

- [Consistency over convention](#consistency-over-convention)
- [Comments](#comments)
- [Versioning and tags](#versioning-and-tags)
- [Licensing](#licensing)

## Consistency over convention

This is your standard disclaimer of don't do something just because it's mentioned here. For example if code is using tab indentation and current convention is spaces then it would be best to not just start coding new areas with spaces instead of tabs. Especially with programs like python that could end badly. This also applies to contributions to other projects. If the other project uses 2 spaces while this guide mentions 4 spaces, contributed code should use 2 spaces.

## Comments

Commenting code is encouraged. A good comment is one that describes *why* something is happening. To know how something works you can just look at the code itself. If you have to explain exactly what the code is doing then perhaps you should see how to simplify. If it is complicated explain *why* it must be done that way and what other methods you looked into.

Comments should be kept up to date with the code. Even worse than no documentation is incorrect documentation.

Keywords like `TODO` are acceptable. They should be used sparingly. If you're writing a paragraph long `TODO` then may just want to make a note of it somewhere else. Also highly suggested to add a date and optionally a user id or your initials. For example `# TODO:2013-10-19: Rewrite this function to support Ubuntu`. Again the date should be in ISO8601 format. The `-` can be omitted if wanted. It is important to remain consistent though. Keeping this in a standard format allows to grep for them and even weed out old `TODO` messages. Aside from `TODO` another keyword is `NOTE` which is used to inform anyone that looks at the source to keep something in mind. None of the other common keywords you see are particularly of use.

## Versioning and tags

Versioning follows the [Semantic Versioning](http://semver.org) guide.  In short that means the version is 3 digits separated by periods.  For example '1.2.3'  This means all projects start at 0.1.0 and it's still a wild-wild west as far as changes go.  I can make whatever changes I want to the public api and just increment the minor number (the 2 in '0.2.3'). Optionally it may have a pre-release version afterwards. For example any new project will start out with it's version at '0.1.0-dev' to signify I'm developing towards 0.1.0.  Typically `-dev` versions don't have git tags.  In general a tag represents a release of a block of code.  It is possible for there to be release candidates and those are release for feedback.  Those take the form of '0.1.0-rc.1'.

There's a pretty big split in the dev community about whether a tag should have the v prefix or not.  Looking at my tag history you can see I keep alternating between the two. I'm now going to stick to not using the v prefix. The main reason for this is in some contexts I still didn't want the prefix. For example variables that contain the version string I wanted to just have the number. But within documentation I did want the prefix. To remove having to think if I need to add the prefix or not I'm just not going to have one.

## Licensing

Let me preface this by saying I'm not a lawyer but to the best of my knowledge this is correct in the United States.

For the most part I use two different licenses depending on how complex the project is.

For projects that aren't that complicated or just a collection of small scripts I'll just dedicate it to public domain. Lawyers tend to frown on public domain because it's not technically a license or something. Chances are if I'm saying it's public domain then it's probably not even complex enough to be licensed. Closest official method of declaring something public domain is [Creative Commons CC0](https://creativecommons.org/publicdomain/zero/1.0/). But the size of the license can be longer than some of the code it applies to. I tend to use the text from [unlicense](http://unlicense.org/) instead.

For projects that are more project like. Meaning they are dedicated to a single thing and may actually be useful to someone I'll license under the [MIT License](http://opensource.org/licenses/MIT). This is about as close to public domain you can get while still making the lawyers happy. If for some reason I was contacted about a project that is public domain I'd license it to them under the MIT license. I may even consider doing a dual license where you can choose between public domain or MIT if in your region you can't legally dedicated something to public domain.

Another option is what's called a dual permissive license. Typically dual licenses are used for commercial products where a section of the program is open source and provided to community for free, there is a "pro" version that has a commercial license.  A dual permissive license is just as it sounds.  The code is licensed under two different licenses.  Anyone that uses the code may choose one of those licenses as the license they'll adhere to.  Using this I can license something both as public domain and MIT.  This way the people that are afraid of public domain can use MIT and people that are fine with public domain can use that.  You can see an example of the license text in my [iptables-init](https://github.com/vrillusions/iptables-init) project at [LICENSE.txt](https://github.com/vrillusions/iptables-init/blob/master/LICENSE.txt).
