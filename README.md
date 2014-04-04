# Notes 

stimulus -> action 
variables -> sensors
game
  brutality
  basic psychology of movements 
  
     function oldDraw() {
        p.fill(0, 15);
        p.rect(p.width / 2, p.height / 2, p.width, p.height);

        grid(gridSize, gridColor, activeRow, activeCol);
        trace(activeCol * gridSize, activeRow * gridSize, pactiveCol * gridSize, pactiveRow * gridSize);

        txtW = p.textWidth(txt);
        p.fill(255);
        p.text(txt, ((activeCol - 0.5) * gridSize), ((activeRow - 0.5) * gridSize));
        //p.println(p.__frameRate);

        if (p.__keyPressed && (p.keyCode == '0')) {
            p.stroke(gridColor);
            p.noFill();
            p.strokeWeight(5);
            p.triangle(data[1][0] * gridSize, data[1][1] * gridSize, data[2][0] * gridSize,
                data[2][1] * gridSize, data[3][0] * gridSize, data[3][1] * gridSize);
            p.strokeWeight(1);
        }
    }

    p.mousePressed = function() {
        p.mouseDragged();
        gridColor = p.color(p.random(255), p.random(255), p.random(255), 128);
    }

    p.mouseReleased = function() {
        p.mouseMoved();
    }

    p.mouseMoved = function() {
        pactiveCol = activeCol;
        pactiveRow = activeRow;

        activeCol = p.floor(p.mouseX / gridSize) + 0.5;
        activeRow = p.floor(p.mouseY / gridSize) + 0.5;
    }

    p.mouseDragged = function() {
        pactiveCol = activeCol;
        pactiveRow = activeRow;

        activeCol = gridW - p.floor(p.mouseX / gridSize) - 0.5;
        activeRow = gridH - p.floor(p.mouseY / gridSize) - 0.5;
    }

    p.keyPressed = function() {
        pactiveCol = activeCol;
        pactiveRow = activeRow;

        switch (p.keyCode) {
            case 37:
                activeCol -= 1;
                if (activeCol < 0.5) activeCol = 0.5;
                break;
            case 39:
                activeCol += 1;
                if (activeCol > gridW - 0.5) activeCol = gridW - 0.5;
                break;
            case 38:
                activeRow -= 1;
                if (activeRow < 0.5) activeRow = 0.5;
                break;
            case 40:
                activeRow += 1;
                if (activeRow > gridH - 0.5) activeRow = gridH - 0.5;
                break;
            default:
                if ((p.keyCode > 64) && (p.keyCode < 123)) {
                    txt = String.fromCharCode(p.keyCode);
                }
                break;
        }

    }

    function grid(gridSize, gridColor, activeRow, activeCol) {
        gridW = p.width / gridSize;
        gridH = p.height / gridSize;

        for (var row = 0.5; row < gridH; row++) {
            for (var col = 0.5; col < gridW; col++) {
                var dotSize;
                if (typeof(activeCol) == 'undefined' || typeof(activeRow) == 'undefined') {
                    dotSize = gridSize * 0.9;
                } else {
                    dotSize = gridSize * p.dist(row, col, activeRow, activeCol) / p.max(gridW, gridH);
                }
                p.noStroke();
                p.fill(0, 64);
                p.rect(col * gridSize, row * gridSize, gridSize, gridSize);
                p.fill(gridColor);
                p.ellipse(col * gridSize, row * gridSize, dotSize, dotSize);
            }
        }
    }

    function trace(x1, y1, x2, y2) {
        p.fill(255);
        p.stroke(255);
        p.ellipse(x1, y1, 10, 10);
        p.line(x1, y1, x2, y2);
        p.ellipse(x2, y2, 10, 10);
    }


# Project-3

![A screenshot from the app](https://raw.githubusercontent.com/DGMD-E-15/Project-3/master/README_media/Screenshot.png)

In `Project-3`, we're going to look at some of the basics of interacting with APIs.  We'll be focusing on two: [Spotify's Web API](https://developer.spotify.com/technologies/web-api/) and [Echo Nest's API](http://developer.echonest.com/docs/v4).  We use these APIs to construct a small, Spotify playlist which visualizes some EchoNest data, lets you sort by things like title and tempo, and displays album art.

We've tried something a bit new in this project.  In addition to [building the project up commit-by-commit](https://github.com/DGMD-E-15/Project-3/commits/master), we've added many more comments to the code than we have in the past, and [we've attempted to categorize the comments](https://github.com/DGMD-E-15/Project-3/blob/master/beat.js#L1) to make reading through the function and structure of the app a little easier.

![API sources from which we construct the app](https://raw.githubusercontent.com/DGMD-E-15/Project-3/master/README_media/APISources.png)

To use the Spotify Play Button embeds, you'll need to [install Spotify](http://spotify.com/) (you can sign up for a free account).  Spotify's Play Button interacts with your local Spotify installation enabling you to click a Play Button on a website and have your local Spotify app begin playing the song.

---

## Extensions

### (1) Add another EchoNest attribute for sorting

Using EchoNest's API, add another column of information on each track and modify the sorting logic to let you sort by the new data.

![Bring in a new EchoNest attribute for sorting](https://raw.githubusercontent.com/DGMD-E-15/Project-3/master/README_media/NewSortExtension.png)

### (2) Visualize another EchoNest attribute

Choose another piece of data from EchoNest's API to visualize using a Processing sketch, either for a currently playing song or for all the songs.

![Visualize a new EchoNest attribute](https://raw.githubusercontent.com/DGMD-E-15/Project-3/master/README_media/NewVisualizationExtension.png)

### (3) Use the EchoNest API to hide the Spotify logo

In each Spotify embed iframe, you'll notice a small, Spotify logo in the lower right.  Using the [`coverURL` function in `spotify.js`](https://github.com/DGMD-E-15/Project-3/blob/master/spotify.js#L186), you can access an album cover _without_ a Spotify logo.  Use this to modify [the `.inter` overlay](https://github.com/DGMD-E-15/Project-3/blob/master/spotify.js#L59) and include a circular `div` with an appropriately positioned background to hide the Spotify logo until you click on the `.inter` overlay.

![Hide the Spotify logo using the full album art](https://raw.githubusercontent.com/DGMD-E-15/Project-3/master/README_media/HideSpotifyLogoExtension.png)

### (4) Lyrics from musixmatch with song

[MUSIXMATCH](http://musixmatch.com/) offers [an API](https://developer.musixmatch.com/) which, among other things, lets you query songs' lyrics.  Add functionality to the project where the lyrics from the current track are faded in when [`currentTrack`](https://github.com/DGMD-E-15/Project-3/blob/master/beat.js#L11) is switched.

![Add lyrics from MUSIXMATCH to the interface](https://raw.githubusercontent.com/DGMD-E-15/Project-3/master/README_media/LyricsExtension.png)

### (5) Fix the UI issues created by so many asynchronous loads

You'll notice that this app sucks.  Which is to say, the user experience is very painful because we basically have to wait for a Spotify iframe and three API queries to EchoNest and Spotify _for every song_ 'til we display anything.  Design a solution to this.  You might indicate the page is loading with some sort of progress bar or sequentially inserts songs as they are loaded so that users aren't left hanging.

![Make the user experience of waiting for content to load less shitty](https://raw.githubusercontent.com/DGMD-E-15/Project-3/master/README_media/AsyncExtension.png)


---

## Resources

+ [Spotify's Play Button Embed Documentation](https://developer.spotify.com/technologies/widgets/spotify-play-button/)
+ [Spotify's Web API Documentation](https://developer.spotify.com/technologies/web-api/)
+ ["Deconstructing Spotify's built-in HTTP server"](http://cgbystrom.com/articles/deconstructing-spotifys-builtin-http-server/)
+ [EchoNest's API Documentation](http://developer.echonest.com/docs/v4)
+ [EchoNest's Analyzer Documentation](http://developer.echonest.com/docs/v4/_static/AnalyzeDocumentation.pdf)
+ [JSON versus JSONP Tutorial - Live](http://json-jsonp-tutorial.craic.com/index.html)
+ ["What's in an HTTP request?"](http://rve.org.uk/dumprequest) and ["HTTP Request/Response Basics"](http://devhub.fm/http-requestresponse-basics/) are good places to start to understand how we might use HTTP to interact with APIs.  For a lot more detail about what's going on behind the scenes, you can check out ["How Does the Internet Work?"](http://www.stanford.edu/class/msande91si/www-spr04/readings/week1/InternetWhitepaper.htm)
+ ["Ways to circumvent the same-origin policy"](https://stackoverflow.com/questions/3076414/ways-to-circumvent-the-same-origin-policy) is a nice overview of some of the ways people handle the limits on cross-domain communication which we struggled with in this project.

--

## Questions?

Please either [file an issue on this repository](https://github.com/DGMD-E-15/Project-3/issues/new) if you have a question _which does not concern your code_, or please file an issue on your own repository if you'd like us to take a look at a problem you have.