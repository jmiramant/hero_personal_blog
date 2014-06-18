title: 'My Sass Affair'
date: 2014-06-04 04:43:54
categories: 'code'
tags:
- sass
---

I took a couple hours last week to leave my beloved CoffeeScript land, and dive into some awesome SASS refactors. It reminded me how powerful the compiler really is. At times, I found myself swearing at the syntax, but then I remembered that I was writing CSS, quit my whining and fell a little more in love...

Here's the starting code. Get ready. It's ugly. Don't run:

``` bash
  #gem-progress
    float: left
    width: 30%
    .prog-gem
      width: 20%
      height: 45px
      float: left
      &.locked
        background-image: url('/assets/journey/grey-gem.png')
        background-size: 89%
        background-repeat: no-repeat
        background-position: 50% 60%
      &.current
        &#strengths
          background-image: url('/assets/journey/blue-gem-outline.png')
          background-size: 86%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#others
          background-image: url('/assets/journey/green-gem-outline.png')
          background-size: 86%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#networking
          background-image: url('/assets/journey/pink-gem-outline.png')
          background-size: 86%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#approach
          background-image: url('/assets/journey/yellow-gem-outline.png')
          background-size: 86%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#coworker
          background-image: url('/assets/journey/purple-gem-outline.png')
          background-size: 86%
          background-repeat: no-repeat
          background-position: 50% 60%
      &.complete
        &#strengths
          background-image: url('/assets/journey/blue-gem.png')
          background-size: 100%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#others
          background-image: url('/assets/journey/green-gem.png')
          background-size: 100%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#networking
          background-image: url('/assets/journey/pink-gem.png')
          background-size: 100%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#approach
          background-image: url('/assets/journey/yellow-gem.png')
          background-size: 100%
          background-repeat: no-repeat
          background-position: 50% 60%
        &#coworker
          background-image: url('/assets/journey/purple-gem.png')
          background-size: 100%
          background-repeat: no-repeat
          background-position: 50% 60%
```

...pheww. That was painful to look at. But here was my refactor:

``` sass
$journey_states: complete, in-progress, current
$joureny_packets: strengths, others, networking, approach, 
  coworker
$joureny_colors: blue, green, pink, yellow, purple
  @each $packet in $joureny_packets
    @each $state in $journey_states
      $i: index($joureny_packets, $packet)
      @if $state == 'complete'
        .#{$state}
          &##{$packet}
            background:
              image: url('#{nth($joureny_colors, $i)}-gem.png')
              size: 100%
              repeat: no-repeat
              position: 50% 60%
      @if $state == 'in-progress' or $state == 'current'
        .#{$state}
          &##{$packet}
            background:
              image: url('#{nth($joureny_colors, $i)}-gem-outline.png')
              size: 87%
              repeat: no-repeat
              position: 50% 60%
```
[Gist](https://gist.github.com/jmiramant/62011e913b0cc31816e3)

After the rewrite, I was a little (lot) excited to haul that piece of junk code out of my, otherwise DRY, perfectly written app (haha), so I sent a tweet:

![sass_tweet](/../images/sass_tweet.png)

...and got some love back from none other than the creator of SASS, [@chriseppstein](https://twitter.com/chriseppstein) with this sweet little refactor: 

``` bash
$journey: (
states: complete in-progress current, packets: 
  ( strengths: blue, 
    others: green, 
    networking: pink, 
    approach: yellow, 
    coworker: purple
  )
)
 
@function journey-image($state, $packet-color)
  @if $state == complete
    @return url('#{$packet-color}-gem.png')
  @else
    @return url('#{$packet-color}-gem-outline.png')
 
@each $packet, $packet-color in map-get($journey, packets)
  @each $state in map-get($journey, states)
    .#{$state}
      &##{$packet}
        background:
          image: journey-image($state, $packet-color)
          size: if($state == complete, 100%, 87%)
          repeat: no-repeat
          position: 50% 60%
```

### Lessons: 
* The support in the dev communitinty is awesome!
* SASS HAS MAP!!! aka...read the docs.