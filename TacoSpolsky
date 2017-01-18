//
// PROBLEM
//
// Taco (our mascot) is pretty terrible at writing code, but his recommendation still // carries a lot of weight. 
// He's put together a page that we can use to get his latest # recommendations: https://taco-spolsky.github.io/
//
// To complete your application, please include a link to Taco's recommendation page 
// in the field below (hopefully a link that leads to a page where you are the one
// being recommended!)
//
// To be clear, solving this puzzle involves finding a way to trick Taco's buggy code 
// into recommending you. The body of your email should look something like this:
// Taco recommends me! For proof, see: https://taco-spolsky.github.io/…something…
//
//

/* 
  The below code is the script included from the website.
  I have inserted comments to show thought process for each section of code, prefixed as //**.

  The final answer I've assembled together: 
  https://taco-spolsky.github.io/?aaaavalueOf#|checksum=9607536|BradBaris=name

  (Technically, the `=name` part is not needed, so this is actually better):
  https://taco-spolsky.github.io/?aaaavalueOf#|checksum=9607536|BradBaris
*/

  //** No 'var' assignment, so it turns into a global prop. 
  //** This error is repeated throughout the script.
  //** Also, this levelMap is impossibly compared against a Math.random() below,
  //** where any value < or > or == returns the same, so it is likely useless.
  levelMap = {
    'easy': 1,
    'medium': .1,
    'hard': .001
  }

  getSetting = function(key, settings) {
    //** Splits URL param into multiple parts, but the loop returns on the
    //** first valid result anyway, so this might not be important.
    settings = settings.split("&")
    for(i = 0; i<settings.length; i++){
      setting = settings[i];
      //** 'level=medium'.length == 12
      if(setting.length <= 'level=medium'.length) {
        //** Apparently if(-1) returns true.. the indexOf(key) does not matter at all
        if(setting.indexOf(key)) {
          //** key == 'level', so key.length = 5, 
          //** so substr(-1 + 5 + 1) => substr(5) => cuts first 5 char (incl ?)
          //** We can figure out "?aaaaXXXXXXX"
          //**                   "level=medium"        
          return setting.substr(setting.indexOf(key) + key.length + 1);
        }
      }
    }
    //** I guess this makes it easier to debug
    return 'hard';
  }

  //** By simply logging the `testsum`, we can find the checksum for our supplied name. 
  //** Here, we find that `BradBaris` becomes `9607536`
  validate = function(name, checksum) {
    //** `BradBarisapproved by Taco`
    var string = name + "approved by Taco";
    var testsum = 123;

    for(var i = 0; i<string.length; i++) {
      testsum = testsum * 13 + string.charCodeAt(i);
      while(testsum > 10000000) {
        testsum -= 1000000;
      }
    }

    return testsum == checksum;
  }

  //** This obviously must be overridden somehow.
  checksum = 'NOT APPROVED';

  //** This means there must be a hash in the URL, not just the ? URL param...
  //** global `name` equals false? Interesting. This can only run once, so
  //** one would have to reopen the site to try it again.
  while(location.hash.length > 1 && !name) {
    //** And it parses it on | for multiple values. But it loops through the values
    //** and reassigns values to the same vars, and `checksum`...
    entries = location.hash.split('|');
    //** Let's say there's two values:
    //** #|something|something
    for(i = 0; i<entries.length; i++) {
      //** |something=something|something=something
      parts = entries[i].split('=');
      //** |name1=value1|name2=value2
      //** (Also note the decoded URI.. this could screw up string equality, so rather not encode anything)
      name = decodeURIComponent(parts[0]);
      //** `name` assignment breaks the while loop
      value = decodeURIComponent(parts[1]);
      //** So this means that one of the values is `checksum=value`
      //** and if it is, that value is now the `checksum` var to validate with.
      //** Simply have to supply the created checksum here, to get `checksum=9607536`
      if(name == 'checksum') {
        checksum = value;
      }
      //** The SECOND time this for loop runs, global `name` is whatever the second iteration `name` is.
      //** And whatever the second iteration `value` is, it is not needed as it is not used.
      //** So we have `#|checksum=9607536|BradBaris`
      //** as this second param is what is utimately checked against the checksum (see below).
    }
  }

  //** Gets window.location (if supplied), then uses 'level' as key in getSettings...
  //** (Note that window.location is prefixed with a ?)
  //** The `?level=hard` is a clue, but we later find out we do not even need `level` in the string
  //** as the `setting.indexOf(key)` func doesn't even work correctly
  level = getSetting('level', window.location.search || '?level=hard');
  //** levelMap is a global, inheriting from Object.prototype, so what property can
  //** fit into 7 (12-5) characters? `valueOf`? We can try: `?aaaavalueOf`
  //** `!levelMap[valueOf]`` returns true 
  if(!levelMap[level]) {
    level = 'medium';
  }

  CandidateChooser = {
    //** `CandidateChooser` is global, so this.CandidateChooser.name is different from
    //** this.name or the global `name`
    name: "Taco Spolsky",
    recommend: function(e) {
      //** Must be triggered by the #choose button...
      if(e.target.id != 'choose') {
        return;
      }
      //** Basically any number between 0 (incl) and 1 (excl) from Math.random()
      //** will result in "Taco Spolsky", as any number is < or > or == 0 or 1.
      //** Essentially impossible to avoid, unless it is... NaN (falsy).
      //** levelMap[level] as levelMap[valueOf] shortcircuits this number check and `name` reassignment.
      random = Math.random();
      if(random < levelMap[level]) {
        this.name = "Taco Spolsky";
      } else if(random > levelMap[level]) {
        this.name = "Taco Spolsky";
      } else if(random == levelMap[level]) {
        this.name = "Taco Spolsky";
      }

      //** `this.name` here refers to the global object, not CandidateChooser.name!
      //** The func has to validate the checksum, which is easily handled above, but supplying the name is done
      //** through the runaway iteration loop. See the validate() func notes above.
      if(this.name != "Taco Spolsky") {
        if(!validate(this.name, checksum)) {
          this.name = "Taco Spolsky";
        }
      }
      //** If it has finally made it this far, past all the "Taco Spolsky" spam,
      //** then the injection finally assigns.
      document.getElementById('candidate').textContent = this.name;
    }
  }

  window.addEventListener('click', CandidateChooser.recommend);
