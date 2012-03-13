---
layout: post
title: "Hacking on the NCAA Tournament for Fun (Not For Profit)"
date: 2012-03-12 23:59
comments: true
categories: [node, postgres, twitter, hacks, flatiron, handlebars, javascript]
author: Luke Karrys
---

### Day 18 of the 30 Day Writing Challenge!

Hey everyone, it's me again. I'm sober this time, don't worry. Well actually I'm drunk on a crazy idea that I had last weekend. I've always been *really* into the NCAA Tournament. I once took up an entire wall in my kitchen with a giant bracket just so I could fill it in after every game. So rolling around in my head last weekend was an idea of combining the NCAA Tournament with my other love, code. I started to play around with a format to see how small (in terms of character length) I could make all my picks. This is what I came up with.

<!--more-->

### How To Get This Up and Running

1. Install [`node`](http://nodejs.org/)
2. Install [`npm`](http://npmjs.org/)
3. `git clone git@gist.github.com:2028007.git gist-2028007`
4. `cd gist-2028007`
5. `npm install`
6. `node app.js S18541137214112424W185463721532533E191213113102112111011111MW1854113728432828FFWMWW`

### Explanation

The argument passed to `app.js` is a string containing picks for the 63 games in the NCAA Tournament (after the play-in games). The example above can be looked at as divided into five parts:

`S18541137214112424` `W185463721532533` `E191213113102112111011111` `MW1854113728432828` `FFWMWW`

### Identifiers

Each of the first four parts are a region and the picks for that region. The last part is the picks for the Final Four. The Final Four identifier (`FF`) and picks must come at the end of the string. The alpha characters that start each part are the identifier for that region. The region identifier must be a key in a valid region key (by valid I mean it must exist in the data file).

### Picks

The numbers for the first four regions correspond to the picks being made for that region. Each number refers to a seed that you think will win their game. The picks must be made in a top-down, left-right order when viewing the games on a bracket. This means for the first round, the games must be picked in the order: `1 v 16, 8 v 9, 5 v 12, 4 v 13, 6 v 11, 3 v 14, 7 v 10, 2 v 15`. So if you wanted to pick all the higher seeded teams for the first round, your numbers would be `18546372`. To complete a region, keep picking winning seeds in this order. To finish our previous example of all the higher seeded teams winning, our numbers would be `185463721432121`. This might be easier to visualize if we look at the picks divided into rounds: `18546372` (first round winners) `2143` (second round winners) `12` (Sweet Sixteen winners) `1` (Elite Eight winner). Pair that with the identifier for the region and you have `E185463721432121` which is a valid region.

### Final Four

Once we have put our four regions together we can add the Final Four. For the Final Four 'region', you will pick the winners not be their seed but by the identifier of the region that they originally came out of. Make sure you know which regions are playing each other as well. In the example of the 2012 NCAA tournament, the Final Four was `S v W and E v SW`. So appropriate Final Four picks would be `WMWW` since we are first picking the winners of the two Final Four matchups and the picking the winner of the final game. Pair that with the Final Four identifier to get `FFWSEW`.

### Output

If your picks are valid, the output should display in an object contain all the regions, rounds and games.

If there are any errors, those will be displayed instead.

### Why?

I wanted a codified format to fit all my picks for the NCAA Tournament into as little characters as possible. I believe that even if all region identifiers were two characters and you picked the higher seed in every game, you could fit your picks in 132 characters (enough to fit in a tweet!). Obviously this could be shortened further by adding additional conventions (such as region order, etc.) but I believe this to be a good mix of brevity and flexibility.

This logic is the basis of [TweetYourBracket.com](http://tweetyourbracket.com) ([GitHub repo](https://github.com/lukekarrys/tweetyourbracket.com)). The idea is that there will be a Twitter watcher which will watch for a specific hashtag and then the bracket will be parsed from the tweet and saved.

### Not But Seriously, Why?

Because it was fun. Not let's show off a few code samples to prove my point.

*Note: all the code here is pulled from [the TweetYourBracket GitHub repo](https://github.com/lukekarrys/tweetyourbracket.com) and slightly modified for example purposes. I also made the [gist](https://gist.github.com/2028007) I referenced above as an easier way to play with the majority of the code.*

{% codeblock picksToArray.js https://github.com/lukekarrys/tweetyourbracket.com/blob/master/lib/plugins/validator.js#L23 %}
// Takes a string of the picks for a region and validates them
// Return an array of picks if valid or false if invalid
var picksToArray = function(picks, regionName) {
  
  var regexp,
      replacement = '',
      regExpStr = '',
      seeds = [1, 16, 8, 9, 5, 12, 4, 13, 6, 11, 3, 14, 7, 10, 2, 15],
      seedLength = seeds.length,
      regExpJoiner = function(arr) {
        return '(' + arr.join('|') + '|X)';
      },
      backref = function(i) {
        return regExpJoiner.call(this, _.map(_.range(i, i+2), function(n) { return "\\"+n; }));
      };

  // Create capture groups for the first round matchups of the region
  // Since we know what order the games should be selected in we can build a regex
  // by splitting the seeds array into groups of two
  // The regex will look like this:
  // (1|16|X)(8|9|X)(5|12|X)(4|13|X)(6|11|X)(3|14|X)(7|10|X)(2|15|X)
  // The X is the character used for a game that hasnt been picked yet
  for (var i = 0; i < seedLength; i += 2) {
    regExpStr += regExpJoiner.call(this, seeds.slice(i, i+2));
  }
  
  // Create capture groups using back references for the capture groups above
  // Since we just created 8 capture groups above and we know that during each subsequent round
  // the number of teams will be cut in half, we can use another loop to add to the regex.
  // We will use glorious back references to ensure that each later match only contains
  // teams that would have had the possibility of advancing to that game.
  // We will add this to our regex above:
  // (\1|\2|X)(\3|\4|X)(\5|\6|X)(\7|\8|X)(\9|\10|X)(\11|\12|X)(\13|\14|X)
  for (i = 1; i < seedLength - 2; i += 2) {
    regExpStr += backref.call(this, i);
  }
  
  regexp = new RegExp(regExpStr);
  replacement = _.map(_.range(1, seedLength), function(num) { return '$'+num; }).join();
  
  // Here we will test if our picks match the regex and if they do
  // we will use the 15 capture groups we created above to split
  // the picks into an array
  if (regexp.test(picks)) {
    return picks.replace(regexp, replacement).split(',');
  } else {
    this.logError(regionName, 'was unable to parse the picks');
    return false;
  }
};
{% endcodeblock %}

I had fun writing to the code above. The purpose of is to take a string of picks and validate them with a regex to ensure that they meet the necessary requirements. It gets a little inception-y as you are looping through and created capture groups in the regex that are using back references to capture groups you just created in the iteration before.

### Now For the Client Side

The code above is all used on the server-side in my application. I tried to leave as much up to server as I could, because I already had the code there and didn't want to deal with bugs bringing it to the browser. So what I did on my [client-side bracket](http://tweetyourbracket.com) was to save the status of each pick into the URL hash. That way, the entire state of the bracket is in the URL at all times. It's not perfect, because I didn't add a watcher for the hash that will change picks if you change the URL. But what it does, is if you load the page with a bracket in the url hash, it will send that hash to the server and return the HTML if the bracket was valid.


{% codeblock bracket.js https://github.com/lukekarrys/tweetyourbracket.com/blob/master/public/js/bracket.js#L103 %}
function stringBuilder() {
  var str = "";
  
  // I'm going through each region's pickable games
  // and taking either the team's region (for the Final Four)
  // or the team's seed (if it's a regular region)
  // and in the end saving the string to window.location.hash
  $('.region').each(function() {
    
    var $reg = $(this),
        regId = $reg.attr('id').replace('_region', '');
    
    str += regId; 
    
    $(this).find('.winners '+pickable).each(function() {
      var $this = $(this),
          text = $.trim($this.text());

      if (text) {
        text = (regId === 'FF') ? $this.data('fromRegion') : text.match(/[0-9]+/)[0];
      } else {
        text = 'X';
      }
      
      str += text;
    });
  });
  window.location.hash = str;
}

// This is called on dom ready
if (window.location.hash) {
  $.ajax({
    // I have server side routes set up with flatiron to render this with handlebars
    url: '/validate/' + window.location.hash.replace('#', ''),
    success: function(data) {
      $content.find('#bracket_holder').html($(data).html());
    }
  });
}
{% endcodeblock %}

### Rendering It All

The last thing I'm going to show you is the server-side JavaScript function that adds the necessary content to our tournament object and the Handlebars template that renders it.

{% codeblock addTeamContent.js https://github.com/lukekarrys/tweetyourbracket.com/blob/master/lib/plugins/validator.js#L23 %}
// Take validated tournament and add necessary content so it is ready for Handlebars
var addTeamContent = function(validatedPicks, editable) {
  
  // These are the master regions
  var ncaaRegions = NCAA.regions;
  
  _.each(validatedPicks.regions, function(region, regionIndex) {
    
    // These are the teams that played in this region
    var regionTeams = (typeof ncaaRegions[region.id] !== 'undefined') ? ncaaRegions[region.id].teams : [];
    
    _.each(region.rounds, function(round, roundIndex) {
      
      // Triple nested loops FTW! ;)
      _.each(round.games, function(game, gameIndex) {

        if (typeof round.teams === 'undefined') round.teams = [];
        
        var team = {
              seed: '',
              name: ''
            },
            isTop = (gameIndex % 2 === 0),
            lastRound = (roundIndex === region.rounds.length-1),
            classes = ['top', 'bottom'];
            
        if (region.id !== this.finalFourRegionName) {

          team.seed = parseInt(game, 10);
          team.name = regionTeams[team.seed - 1];
          team.fromRegion = region.id;
          
        } else {
          
          // These are selected winners in the final four
          var fromRegion = _.find(validatedPicks.regions, function(reg) { return reg.id === game; });
          
          if (fromRegion) {
            var finalFourTeam = _.first(_.last(fromRegion.rounds).teams);
            team.seed = finalFourTeam.seed;
            team.name = finalFourTeam.name;
            team.fromRegion = game;
          }
        }
        
        round.teams[gameIndex] = _.extend(team, {
          editable: editable,
          startMatchup: isTop,
          endMatchup: !isTop,
          classes: (lastRound) ? '' : classes[~~!isTop]
        });
        
      }, this);
    }, this);
  }, this);

  return validatedPicks;
};
{% endcodeblock %}

Check out the [Handlebars template](https://github.com/lukekarrys/tweetyourbracket.com/blob/master/templates/bracket.hbs) on GitHub to see how this was all rendered. Unfortunately I couldn't figure out how to embed Handlebars code with Octopress.

## In The End

I had a small dream of making this a production ready app in time for Selection Sunday (which was last Sunday). Obviously that didn't work out but  [TweetYourBracket.com](http://tweetyourbracket.com) was an extremely fun exercise even if it didn't get anywhere close to ready for prime time use. Some things I wanted to do with it:

- Make it not look like crap (using Bootstrap)
- Add some cooler interactions during the bracket (drag 'n' drop, keyboard shortcuts, auto scrolling)
- Mobile ready with some media queries
- Make the Postgres DB not break when a row doesn't exist (sounds easy, so no idea why I couldn't get it to work)
- Better hosting (for now it's on a free Heroku instance)
- Make it actually do what was advertised and setup the Twitter Streaming API watcher

I encourage anyone who is interested in this to take a look at the [GitHub repo](https://github.com/lukekarrys/tweetyourbracket.com). I just opened it up today and would never refuse a cool pull request or issue. Hopefully it will be ready by next year :)
