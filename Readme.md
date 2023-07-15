# The Messenger Game Framework

Messenger is a *message-oriented* (2D) game framework for Elm based on **[canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)**.

Messenger is **not** an elm package, it is a framework so some code will be generated by script in your project.

## Games made with Messenger

- [Reweave](https://github.com/linsyking/Reweave)
- Waiting for your project!

I recommend you to play the game Reweave. There are many examples explained with Reweave in the documentation.

> Hints on playing Reweave:
>
> You can press \` button to open the console and use some cheat commands:
>
> - `load <map>`: load the required map
> - `gete <num>`: get the required number of energy, unlimited

## Features

- Message based, (functional) object oriented programming.
  Faster development cycle, easy to divide work.
- Auto-adapting.
  Try resizing the web browser or play Reweave on your phone. There are no difference!
- LocalStorage support.
  You can easily manage the game data. Players don't have to finish your game in one run.
- Audio Manager.
  It's extremely easy to play audios in Messenger. You can choose to play one audio in loop mode or once mode.
- Easy textures and sprites.
  It's easy to import textures to your game. The game will not start until all textures are loaded, so the user won't have problems with failing assets or slow loading speed.
- Canvas rendering.
  You can use huge amounts of filters in canvas to render the game. It has better performance than the DOM rendering.
- Modular development.
  Every component, layer, scene is a module, no worry for the code management.
- Flexible framework design.
  Our game framework is *progressive*. You can use the most basic framework: only scenes, and mange the layers by yourself or don't use layers. If you want to use layers, then use it! Layers don't depend on any other things like components. You can add components to a layer but it is up to you. Scene prototypes are also optional, but it is often useful when making a game which has maps or repeated scenes.
- Flexible component design.
  The component in Messenger is quite flexible (but not so efficient as game components/customized components). You can embed component into a component and a component doesn't have to be a "real object", it can be a character manager, etc. See [Typer component](https://github.com/linsyking/messenger-examples/tree/main/components) for an example.
- Powerful configuration file.
  In `MainConfig.elm`, you can set your canvas size, initial scene, background color, etc. It doesn't have any limitations on your game. You can also enable the `debug` mode to debug your games more easily.
- Mouse event fully supported.
  Although we use canvas and the layer concept is abstract, you can also control the mouse event easily. [This layer example](https://github.com/linsyking/messenger-examples/tree/main/layers) shows that it is possible to block the mouse event. The core mechanism is that **the order of rendering layers/components/etc. is opposite to the order of updating them**. That is to say, the topmost object will be rendered last but will be updated first, so you can choose to block the mouse event in the topmost object so that the other objects below it won't see the mouse event. In the example above, when you click on the white circle zone, only the green rectangle can receive the click message.
  ![](docs/imgs/layer.png)
  It's also possible to make a drawing game, thanks to the [elm-canvas](https://github.com/joakin/elm-canvas/tree/5.0.0) package (however the original project doesn't update anymore so we use a fork project). See [the simple paint](https://chimeces.com/elm-canvas/drawing.html). More examples can be found on [examples](https://chimeces.com/elm-canvas/). If you need that you may need to add custom mouse events to the subscriptions in `Main.elm`.


## Canvas VS DOM/SVG

SVG/DOM is something like (need to be controlled by DOM):

```html
<svg viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="50" />
</svg>
<div>
    <b>Hi</b>, <a>I</a> am Bob.
</div>
```

Canvas, however is something like (fully controlled by JS):

```html
<canvas id="canvas" width="578" height="200"></canvas>
<script>
  var canvas = document.getElementById('canvas');
  var context = canvas.getContext('2d');
  // ...
</script>
```

There are many differences between them, like:

- Position.
  In DOM you can use many ways to position one object, like `flex`. However, in canvas, you can only use coordinates.
- Mouse events.
  DOM can automatically do that for you. Canvas is stateless, and it doesn't have default mouse event handler. However, Messenger provides a good solution.
- Tags.
  You can directly use `video` tag to play a video, `img` tag to display some image in DOM. In canvas everything is done by controlling the canvas `context`.
- Visual effects.
  Canvas allows you to manipulate pixel and apply filter effects. In DOM you have to use CSS but is also limited.
- Performance
  Canvas is faster when rendering many objects.

Read [the comparison](https://www.kirupa.com/html5/dom_vs_canvas.htm) or [this blog](https://blog.logrocket.com/when-to-use-html5s-canvas) to learn more. Generally, canvas is better for making video games while DOM is better for other daily-use websites.

## Conceptual Picture

![](docs/imgs/concept.png)

- Red arrow: messages that are sent positively
- Orange arrow: messages that are triggered passively

A **Scene** is a literally a *scene*. A game may have many different scenes. In Reweave, there are about 10 scenes:

- Home
- Level0 to Level5
- Level4boss and Level5boss
- Path
- End

You can use the console (component) in the game (press \` in the main game) to load different scenes by entering some command like `load Level2 `. However, writing a new scene is a heavy work, when some scenes are very similar, we can write a *SceneProto* that can generate scenes given some arguments. In fact, only `Home` scene in Reweave are written from scratch, and all the other level scenes are generated by the `CoreEngine` SceneProto. You probably need to write your own SceneProto when making a game.

When we are talking about "communicate", it is the behavior that one object sends some message to the other object.

A Scene will contain several **Layers**. Different layers have different orders when rendering, so you can separate one scene to background layer and frontground layer for rendering different objects. Layers can communicate with each other, and can also communicate with the parent scene. This pattern is the core mechanism in Messenger. We will see this again later. In short, layers are used to separate objects that are rendered with different orders.

A Layer (may) contain several **Components**. You can have no component and only use the layer as a background image layer. Components are also able to communicate with each other and communicate with the parent layer.

In the next chapter, we will see how to create a new Messenger project.

## Tutorial/Guide

[Full documentation](docs/doc.pdf)
