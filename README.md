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
- Download: https://www.mapbox.com/tilemill/


### Mapbox-Studio

- Was originally developed as Tilemill 2 but was rebranded before release.
- Was only released a few weeks ago, though has been in dev for long before
  that.
- Harder, better, faster, stronger than Tilemill.
- Requires a Mapbox account (there's a free tier).
- Includes really easy access to global datasets (e.g. streets, terrain,
  imagery)
- Not as good as Tilemill for publishing maps outside of the Mapbox service (though
  theoretically possible if you're using only your own data).
- Open source (BSD license).
- Download: https://www.mapbox.com/mapbox-studio


## CartoCSS

A rule-based language for applying styles to maps. Has a lot of similarity to plain old CSS (used for styling web pages).


Looks like so:

```css
#water {
    polygon-fill: #639EB7;
    line-color: #407E99;
    line-width: 1;
}
```


### Breaking it down

Structure of a CSS rule:

```css
selector {
    attribute: value;
    another-attribute: another-value;
    ...
}
```

Note that the semicolons `;` at the end of each attribute line are important.


#### Attributes and Values

These are describe specific stylistic elements (color, size, etc) that should be
applied to data. They have some general categories depending on the type of data
being styled (e.g.  points, lines, polygons, text).

```css
#water {
    polygon-fill: #639EB7;
    line-color: #407E99;
    line-width: 1;
}
```

In that example, we can assume water features are polygons and this rule says
that the inside of the polygon (`polygon-fill`) should be colored `#639EB7`. The
outside of the polygon (`line-color`) should be colored `#407E99`, and that
outline should have a width (`line-width`) of 1 pixel.

There are a lot of different attribute names. Way too many to go over them one
by one, but they follow a logical naming scheme and the example projects are a
good way to get a feel of the most common ones. 

A full reference list is here:
https://github.com/mapbox/carto/blob/master/docs/latest.md


#### Selectors

There are two main types of selectors: ID selectors and Class selectors.

##### ID Selectors

ID selectors target a dataset by its ID and are prefixed by the `#` symbol. For
example:

```css
#myRoadDataset {
    ...
}
```

Note that IDs should be *unique*.

##### Class selectors

Class selectors target potentially multiple datasets all of the same "class".
Class selectors are prefixed by the `.` symbol. For example:

```css
.roads {
    ...
}
```

Classes are determined by you as the designer and data keeper. For example you
might have several specific road datasets, each with their own ID
(`#austinRoads`, `#sanAntonioRoads`, etc.), but you want to apply the same rules
to all of them. In this case, give each dataset the class `.roads`.

##### Other Special Selectors

`Map` is used to set the background attributes (wherever there is no
data to style):

```css
Map {
    background-color: #fefef1;
}
```


##### Filters

Filters make selectors more specific. For example, they can be used to apply
rules at certain zoom levels:

```css
#road[zoom>=13] {
    ...
}
```


Or refer to features in a dataset that match some attribute:

```css
#road[type='major'] {
    ...
}
```


##### Multiple Selectors

When separated by a comma, the rule is applied if either filter is true:

```css
#road[zoom>=13], #road[type='major'] {
    ...
}
```


When chained together, the rule is applied when both filters are true:

```css
#road[type='major'][zoom>=13] {
    ...
}
```


Rules are applied in order of appearance, and style attributes will be
over-written by subsequent rules:

```css
#road[type='major']{
    line-color: #1E2A2F2;
}

#road[type='major'][zoom>=13] {
    line-width: 1;
}

#road[type='major'][zoom>=17] {
    line-width: 2;
}
```


##### Nesting Selectors

Selectors can be nested inside of other rules. This helps keep things organized:

```css
#road[type='major']{
    line-color: #1E2A2F2;

    [zoom>=13] {
        line-width: 1;
    }
    [zoom>=17] {
        line-width: 2;
    }
}
```


##### More info

More technical details on selectors: https://www.mapbox.com/tilemill/docs/guides/selectors/




### Variables

Variables are place-holders where values can be stored so they can be referred
to later in rules. They allow some value to be defined once and reused multiple
times. They are used extensively in the example projects that come with Tilemill
and Mapbox Studio.

Example:

```css
@water: #639EB7;

#river {
  polygon-fill: @water;
}

#stream {
  line-fill: @water;
}
```



### Colors!

Colors can be defined a few ways:
 - name: `blue` (note that not all colors have names)
 - hex: `#47DAD3`
 - rgb (red, green, blue): `rgb(71, 218, 211)`
 - rgba (red, green, blue, alpha): `rgba(71, 218, 211, 0.5)`
 - hsl (hue, saturation, lightness): `hsl(177, 67%, 57%)`
 - hsla (hue, saturation, lightness, alpha): `hsla(177, 67%, 57%, 0.5)`


See this link to play with CSS colors:
https://developer.mozilla.org/en-US/docs/Web/CSS/Tools/ColorPicker_Tool


To help come up with some color palettes:
 - http://paletton.com/
 - http://design-seeds.com/index.php/search
 - https://kuler.adobe.com/create/color-wheel/
 - http://www.colourlovers.com/


### Resources

- Screencast of styling in Mapbox Studio: https://vimeo.com/mapbox/review/104757116/dc2bb1a46c
- Big list of CartoCSS attributes: https://github.com/mapbox/carto/blob/master/docs/latest.md
