# map-styling-basics

This is a short collection of notes about how to do map styling with CartoCSS!



## What you'll need

Software-wise, you've got two good options:


### Tilemill

- The original not-terrible way to make web maps.
- Open source (BSD license).
- Developed by Mapbox.
- Soon to be old hat. These days virtually all development attention is focused
  on Mapbox Studio.



### Mapbox-Studio

- Was originally developed as Tilemill 2 but was rebranded before release.
- Was only released a few weeks ago, though has been in dev for long before
  that.
- Harder, better, faster, stronger than than Tilemill.
- Requires a mapbox account (there's a free tier).
- Includes really easy access to global datasets (e.g. streets, terrain,
  imagery)
- Not as good as Tilemill for publishing maps outside of mapbox service (though
  theoretically possible if you're using only your own data).
- Open source (BSD license).




## CartoCSS

A rule-based language for applying styles to maps.


Looks like so:

    #water {
      polygon-fill: #639EB7;
      line-color: #407E99;
      line-width: 1;
    }


### Breaking it down

Structure of a CSS rule:

    selector {
        attribute: value;
        another-attribute: another-value;
        ...
    }


#### Selectors

`Map` is used to set the background attributes (wherever there is no
data to style):

    Map {
        background-color:#fefef1;
    }


But far more often, they are used to point to specific datasets by ID:

    #road {
        ...
    }


They can contain filters to get more specific. For example, zoom levels:

    #road[zoom>=13] {
        ...
    }


Or refer to attributes on the dataset:

    #road[type='major'] {
        ...
    }


When separated by a comma, the rule is applied if either filter is true:

    #road[zoom>=13], #road[type='major'] {
        ...
    }


When chained together, the rule is applied when both filters are true:

    #road[type='major'][zoom>=13] {
        ...
    }


They can also be nested - this helps keep things organized:

    #road[type='major']{
        line-color: #1E2A2F2;

        [zoom>=13] {
            line-width: 1;
        }

        [zoom>=17] {
            line-width: 2;
        }
    }


More technical details on selectors: https://www.mapbox.com/tilemill/docs/guides/selectors/




### Variables

Variables are place-holders where values can be stored so they can be referred
to later in rules. They allow some value to be defined once and reused multiple
times. They are used extensively in the example projects that come with Tilemill
and Mapbox Studio.

Example:

    @water: #639EB7;

    #river {
      polygon-fill: @water;
    }

    #stream {
      line-fill: @water;
    }



### Colors!

Colors can be defined a few ways:
 - hex: #47DAD3
 - rgb (red, green, blue): rgb(71, 218, 211)
 - rgba (red, green, blue, alpha): rgba(71, 218, 211, 0.5)
 - hsl (hue, saturation, lightness): hsl(177, 67%, 57%)
 - hsla (hue, saturation, lightness, alpha): hsla(177, 67%, 57%, 0.5)


See this link to play with CSS colors:
[https://developer.mozilla.org/en-US/docs/Web/CSS/Tools/ColorPicker_Tool]


To help come up with some color palettes:
 - http://paletton.com/
 - http://design-seeds.com/index.php/search
 - https://kuler.adobe.com/create/color-wheel/
 - http://www.colourlovers.com/

