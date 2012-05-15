### [Jackal](http://practicingruby.com/articles/57) ([code](https://github.com/elm-city-craftworks/jackal))

##### A bunch of regular expression stuff

I never really learned Perl, and so crazy global variables are not in my wheelhouse. Whenever a pattern match occurs in Ruby, some global variables are set:

```
irb> require 'English'
  => 15
irb> "Minnesota Twins, 10-25, 120 runs" =~ /, (\d+-\d+), /
  => 15
irb> $LAST_MATCH_INFO
  => #<MatchData ", 10-25, " 1:"10-25">
irb> $PREMATCH
  => "Minnesota Twins"
irb> $POSTMATCH
  => "120 runs"
irb> $1
  => "10-25"
irb> $MATCH
  => ", 10-25, "
```

It's possible to use even more obscure global variables and not `require 'English'`.

<table>
  <tr>
    <td>$~</td>
    <td>$LAST_MATCH_INFO</td>
  </tr>
  <tr>
    <td>$`</td>
    <td>$PREMATCH</td>
  </tr>
  <tr>
    <td>$'</td>
    <td>$POSTMATCH</td>
  </tr>
  <tr>
    <td>$&</td>
    <td>$MATCH</td>
  </tr>
</table>

When you think about it, though, Ruby has a better way. [`MatchData`](http://www.ruby-doc.org/core-1.9.3/MatchData.html) (`"Minnesota Twins, 10-25, 120 runs".match(/, (\d+-\d+), /)`) has similar methods like `#post_match`, `#pre_match` and so on. You can even hash away your matches with named keys (RE `?<record>`):

```
irb> md = "Minnesota Twins, 10-25, 120 runs".match(/, (?<record>\d+-\d+), /)
  => #<MatchData ", 10-25, " record:"10-25">
irb> md[:record]
  => "10-25"
```

I also learned of the `x` modifier (`/regex/x`) which ignores whitespace and comments in a regular expression. This would be handy for monster expressions that need to flow over multiple lines and require inline documentation.

##### File processing

Yeah, I'm a bad web programmer and don't ever do this stuff. I see that Ruby 1.9.3 has improved the beauty of writing files. Was:

```
File.open(filename, "w") { |f| f << contents }
```

Now:

```
File.write(filename, contents)
```

`Dir.mktmpdir` also looks useful for building up a temporary directory tree in tests, for instance.