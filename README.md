# geometrize.js

A configurable plugin to make a triangular interactive gradient background using canvas. Adjust the settings to make your own effect!

## Table of Contents

- [Live demo](#live-demo)
- [Documentation](#documentation)
  * [Installation](#installation)
  * [Usage](#usage)
    + [Method 1 - Canvas Id](#method-1---canvas-id)
    + [Method 2 - Parent Id](#method-2---parent-id)
    + [With a config](#with-a-config)
  * [Code Documentation](#code-documentation)
    + [Constructor](#constructor)
    + [Config Object](#config-object)
    + [GradientSetting](#gradientsetting)
      - [ColourProfile](#colourprofile)
    + [GridSetting](#gridsetting)
    + [EffectsSetting](#effectssetting)
  * [How It Works](#how-it-works)

## Live demo

[https://troberts0811.github.io/geometrize-js/](https://troberts0811.github.io/geometrize-js  "Geometrize") 

## Documentation

### Installation

For those using task runners (Webpack, Gulp e.t.c), you can download the package currently via NPM

    npm i -S @tomroberts/geometrize  

If your not then you can [save a minified production build here](https://raw.githubusercontent.com/troberts0811/geometrize-js/master/dist/production/geometrize.js) to add to your project.

### Usage

If you are using the task runners then you first need to import the library.

    import Geometrize from '@tomroberts/geometrize';

Else you can just take the file i mentioned above and put it in a script tag.

    <script src="/js/geometrize.js"></script>

And then just call the constructor and store it into a variable!

    let geometrize = new Geometrize('geometrize');

There are two ways to run Geometrize based first parameter of the constructor. The first parameter is an Id and it can be either the Id of a canvas you want it on or the id of a parent you want a canvas to sit in.

For both scenarios you will need to add to your stylesheet a canvas rule with width and height set because of the pixel ratio effect. For example you could use this setup.

    canvas{
	    width: 100%;
	    height: 600px;
    }

#### Method 1 - Canvas Id

    <div>
	    <canvas  id="geometrize"  width="100%"  height="600"></canvas>
	</div>

Geometrize will use this canvas as it has the id and paint the effect on here.

#### Method 2 - Parent Id

    <div id="geometrize"></div>

In this scenario Geometrize will add a new canvas within this element that will be sized to the same size of this element.

#### With a config

There is a second parameter for passing a config to Geometrize. You can copy one from the [live demo](https://troberts0811.github.io/geometrize-js  "Geometrize") or read more about the options below in the contructor section. The easiest way to use it is in the example below.

    import Geometrize from '@tomroberts/geometrize';
    
    let config = {
	    ...
	};
	
    let geometrize = new Geometrize('geometrize', config);

### Code Documentation

Here's a list of the different functions and data structures to understand what to call and the config structure.

#### Constructor

    Geometrize([targetId], [config])

|Field|Type|Required|Default value|
--- | --- | --- | ---
|targetId|String|Yes||
|config|Config Object|No|null|

The targetId is the element where Geometrize will be put. If the element is a canvas then it will use the canvas and use it's height and width. For anything else it will treat the Id as a parent and append a canvas as a child. The canvas will be set to be the width and height of Geometrize's parent element.

If no config is passed then the default options are used. For the data structure of the Config Object see the Config Object settings or export your own config from the [online demo](https://troberts0811.github.io/geometrize-js  "Geometrize").

#### Config Object

The Config Object is the master object and has all of it's properties made of the other models involved in Geometrize. Here is all of it's properties and types that go with it. You can find more information on each one below. No values in any of these objects are required as they all have default values.

    {
	    Gradient: GradientSetting
	    Grid: GridSetting
	    Effects: EffectsSetting
    }


#### GradientSetting

Gradient Settings control the colouring of the triangles. The object for it looks like below.

    {
	    Direction: 'right',
	    Milestones: [ColourProfile]
    }
|Field|Type|Values|Default value|
--- | --- | --- | ---
|Direction|String|left, right, up, down||
|Milestones|Array|ColourProfile Object|Array of 3 ColourProfile objects|

The direction value indicates how the gradient should appear when mapping the Milestones to the triangles in the canvas. The Milestones property indicates what gradient is being painted onto the triangles. 

when passing this array at least a ColourProfile object is required else it will switch to the default value. It is recommended though you have at least 2 colours or you will not see anything. You can experiment with colours on the [live demo](https://troberts0811.github.io/geometrize-js  "Geometrize"). Each ColourProfile object you pass must have a Hex and Position value or the GradientSetting will switch Milestones to its default value.

By default, 3 random Milestones are used so that every time Geometrize is instanced without a config you will get a new gradient. These 3 milestones will each get a random colour and then get placed within a certain area. The first milestone will be randomly within 0-6% of the gradient, the second within 40-61% and the last one between 95 and 100%. 

##### ColourProfile

The ColourProfile only has 2 properties to help markup a part of a gradient. The object looks like below.

    {
	    Hex: '#000000',
	    Position: 15
    }
|Field|Type|Description|
--- | --- | --- | ---
|Hex|String|A colour code as a string. Can be a hex or RGB string but will be converted and stored as a hex.|
|Position|Int|A position within the gradient. Has to be between 0 and 100 inclusive.|

#### GridSetting

The GridSetting object defines key information on how to build the grid and effects how many triangles are made and in what shape.

    {
	    Rows: 30,
	    Columns: 36,
	    PositionRandomPercentageX: 33.33,
	    PositionRandomPercentageY: 33.33
    }

|Field|Type|Description|
--- | --- | --- | ---
|Rows|Int|No of rows in the grid. More rows means smaller height of triangles.|
|Columns|Int|No of columns in the grid. More columns means smaller width of triangles.|
|PositionRandomPercentageX|Decimal|% randomicity in x axis of a grid point. See how it works for more info. |
|PositionRandomPercentageY|Decimal|% randomicity in y axis of a grid point. See how it works for more info.|

#### EffectsSetting

The EffectsSetting object defines interactivity on the canvas. The object looks like below.

    {
	    Hover: true,
	    Click: true,
	    HoverColour: '#666666',
	    ActiveColour: '#CC0000'
    }

|Field|Type|Default Value|Description|
--- | --- | --- | ---
|Hover|Boolean|true|Toggle triangle hover colour effects|
|Click|Boolean|true|Toggle triangle click colour effects|
|HoverColour|String|#666666|Colour for the triangles on hover if the effect is enabled|
|ActiveColour|String|#CC0000|Colour for the triangles on click if the effect is enabled|

It's important to note the more triangles you have generated then the more sluggish the plugin will perform. The tracking of the mouse and searching of triangles can become more intense as the volume increases. If you struggle for performance due to grid size then i recommend disabling the effects here.

### How It Works

The next few images helps describe the logic in how Geometrize works. The black rectangle is represented as the canvas. Geometrize is based on a grid that you can configure via the GridSetting object. The canvas grid is split evenly over n rows and n columns as shown in figure 1 below. Where the grid borders (grey dashed lines) cross with each other and the canvas boundary is where points (the green circles) get plotted. 

![The Grid Settings for Geometrize](https://troberts0811.github.io/geometrize-js/img/how-it-works-1.jpg)
*Fig 1: The canvas grid settings*

The blue cross-markers are the randomicity each point has based on the PositionRandomPercentageX and PositionRandomPercentageY of GridSetting. This helps distort the grid and create unique shapes. You will notice points along the canvas border cannot go randomly inwards. This is to ensure the canvas gets completely covered. The rows and columns you have, the higher the triangle density will be in the canvas and thus make your triangles smaller. Each point in the grid gets a random position that it can reach as shown in figure 2.

![The Randomicity of the Grid](https://troberts0811.github.io/geometrize-js/ismg/how-it-works-2.jpg)

*Fig 2: The randomicity of the points in the grid*

Once these positions are set then the grid has its points and it can start to build the triangles from the effect. The neighbouring point across and underneath each one is used to make a square and cut it diagonaslly to make two triangles. Once all the points are covered then the triangular effect starts to form like in figure 3.

![The Triangles put into the Grid](https://troberts0811.github.io/geometrize-js/img/how-it-works-3.jpg)

*Fig 3: The triangles put into the grid*

The triangles have their colour calculated based on how far they are into the grid and how their position relates to the milestones set in GradientSetting. The colour transitions in each triangle to match the gradient you want to show as the final product you see on screen.

![The Triangular Effect](https://troberts0811.github.io/geometrize-js/img/how-it-works-4.jpg)

*Fig 4: The triangular effect which would transition your gradient*