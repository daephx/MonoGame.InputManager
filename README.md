# MonoGame-InputManager

An easy-to-use input manager for MonoGame!

```plain
                      ___                   _   __  __
                     |_ _|_ __  _ __  _   _| |_|  \/  | __ _ _ __   __ _  __ _  ___ _ __
                      | || '_ \| '_ \| | | | __| |\/| |/ _` | '_ \ / _` |/ _` |/ _ \ '__|
                      | || | | | |_) | |_| | |_| |  | | (_| | | | | (_| | (_| |  __/ |
                     |___|_| |_| .__/ \__,_|\__|_|  |_|\__,_|_| |_|\__,_|\__, |\___|_|
                               |_|                                       |___/

                               _=====_                               _=====_
                              / _____ \                             / _____ \
                            +.-'_____'-.---------------------------.-'_____'-.+
                           /   |     |  '.           _           .'  |  _  |   \
                          / ___| /|\ |___ \       ,'" "',       / ___| (_) |___ \
                         / |      '      | ;      | A1r |      ; | _         _ | ;
                         | | <--     --> | |  __  '.___.'  __  | |(_)       (_)| |
                         | |___   .   ___| ; |__|         |__> ; |___   _   ___| ;
                         |\    | \|/ |    /  _              _   \    | (_) |    /|
                         | \   |_____|  .','" "',        ,'" "', '.  |_____|  .' |
                         |  '-.______.-' /       \      /       \  '-._____.-'   |
                         |               |       |------|       |                |
                         |              /\       /      \       /\               |
                         |             /  '.___.'        '.___.'  \              |
                         |            /                            \             |
                         \           /                              \           /
                          \_________/                                \_________/
```

## Table of contents

- [Goals](#goals)
- [Installation](#installation)
- [Usage](#usage)
- [References](#references)
  - [Enums](#enums)
  - [Properties](#properties)
  - [Method](#method)
- [More Information](#more-information)
  - [Local Multiplayer](#local-multiplayer)
  - [Handling player selection](#handling-player-selection)
  - [Porting Cross-platform](#porting-cross-platform)
- [Licence](#licence)

## Goals

- Controller first input manager
- Easily to use and configured
- Easy to port, alternate bindings for gamepad and mouse/keyboard
- Seamless multiplayer handling multiple input devices

## Installation

1. Copy `InputManager.cs` into your MonoGame project
2. ???
3. PROFIT!

## Usage

Basic example for getting started!

The InputManager can be registered as a GameComponent or you can manually the
Initialize and Update methods from within the game class.

```csharp
// MonoGame class
public class Peace : Game
{
    InputManager iM;

    public Peace()
    {
        // Construct a new InputManager to use it in the Update function.
        iM = new InputManager(this);
    }

    protected override void Initialize()
    {
        // Register GameComponent or call `iM.Update(gameTime)` in the Game's Update function
        this.Components.Add(iM);
    }

    protected override void Update(GameTime gameTime)
    {
        // Manually handling update calls
        // iM.Update(gameTime);

        // Setting up the right values
        var acceleration = 1.337f;
        var pos = new Vector2(x, y);
        // Check for an Input and handle it accordingly
        if (iM.IsPressed(Input.Home))
            Exit();

        // Accelerate based on user input
        if (iM.IsPressed(Input.Right))
            pos.X += acceleration;

        if (iM.IsPressed(Input.Left))
            pos.X -= acceleration;

        if (iM.IsPressed(Input.Up))
            pos.Y -= acceleration;

        if (iM.IsPressed(Input.Down))
            pos.Y += acceleration;
    }
}
```

## References

### Enums

**For Gamepads**

Use the `Microsoft.Xna.Framework.Input.Buttons` or the more generic `Input` enum
to identify which action has been pressed

```csharp
enum Input
{
    // Center
    Home,
    Start,
    Back,

    // D-Pad
    Up,
    Left,
    Down,
    Right,

    // Face buttons
    FaceButtonUp,
    FaceButtonLeft,
    FaceButtonDown,
    FaceButtonRight,

    // Shoulder buttons
    LeftShoulder,
    RightShoulder,

    // Triggers
    LeftTrigger,
    RightTrigger,

    // Left Stick
    LeftStick,
    LeftStickUp,
    LeftStickLeft,
    LeftStickDown,
    LeftStickRight,

    // Right Stick
    RightStick,
    RightStickUp,
    RightStickLeft,
    RightStickDown,
    RightStickRight
}
```

**For Keyboard**

Use the `Microsoft.Xna.Framework.Input.Keys` enum to identify which action has
been pressed

**For Mouse**

Use the `MouseInput` enum to identify which action has been pressed

```csharp
public enum MouseInput
{
    None,
    LeftButton,
    MiddleButton,
    RightButton,
    Button1,
    Button2
}
```

### Properties

**States**

These properties can be used for getting the raw values from the MonoGame states

```csharp
KeyboardState currentKeyboardState;
KeyboardState previousKeyboardState;
MouseState currentMouseState;
MouseState previousMouseState;
```

**Deadzones**

> Controller deadzone is an area where the input of your controller is not
> registered. It is used to adjust the analog sticks so that they do not drift

![Axial Deadzone](https://web.archive.org/web/20150513185817if_/http://www.third-helix.com/images/2013/04/axial-deadzone.jpg)

```csharp
float DeadzoneSticks = 0.25f;
float DeadzoneTriggers = 0.25f;
```

InputManager will try to find connected controllers based on this variable.

```csharp
int PlayerCount = 1;
```

### Methods

**General methods**

These methods are also used for mapped keyboards or a mapped mouse.

```csharp
bool IsPressed(Input input)
bool IsPressed(Input input, int index)
bool IsHeld(Input input)
bool IsHeld(Input input, int index)
bool JustPressed(Input input)
bool JustPressed(Input input, int index)
bool JustReleased(Input input)
bool JustReleased(Input input, int index)
bool SomethingDown()
bool SomethingDown(int index)
float GetRaw(Input input)
float GetRaw(Input input, int index)
bool SetGamepadVibration(int index, float left, float right)
GamePadCapabilities GetCapabilities(int index)
void AddKeyboardPlayer(Dictionary<Input, Keys> map)
void AddMousePlayer(Dictionary<Input, MouseInput> map)
bool AllPlayersConnected()
int GetPlayerCount()
void SetUsablePlayers(int[] playerIds)
```

**Gamepad methods**

```csharp
bool IsPressed(Buttons button)
bool IsHeld(Buttons button)
bool JustPressed(Buttons button)
bool JustReleased(Buttons button)
```

**Keyboard methods**

```csharp
bool IsPressed(Keys key)
bool IsHeld(Keys key)
bool JustPressed(Keys key)
bool JustReleased(Keys key)
```

**Mouse methods**

```csharp
bool IsPressed(MouseInput input)
bool IsHeld(MouseInput input)
bool JustPressed(MouseInput input)
bool JustReleased(MouseInput input)
Point GetMousePosition()
int GetMouseScroll()
```

**MonoGame component**

Methods inherited from the parent class: `Microsoft.Xna.Framework.GameComponent`

```csharp
public void Initialize()
public void Update(GameTime gameTime)
```

## More information

### Local Multiplayer

To implement multiple keyboard/controller players you can do something like
this:

```csharp
public class Peace : Game
{
    // Simple player with actions
    struct myPlayer
    {
        public Input Run;
        public Input Jump;
    }

    myPlayer[] myPlayerArray;

    // In multiplayer initialisation code
    public Peace()
    {
        iM = new InputManager(this)
        {
            PlayerCount = 2
        };

        // Set up an simple player array with re-mappable input actions.
        myPlayerArray = new myPlayer[]
        {
            new myPlayer() { Run = Input.FaceButtonLeft, Jump = Input.FaceButtonDown }
            new myPlayer() { Run = Input.FaceButtonLeft, Jump = Input.FaceButtonDown }
        };

    }

    // In Update code
    protected override void Update(GameTime gameTime)
    {
      // Check if the number of available players has changed. You can handle it your way.
      if (!iM.AllPlayersConnected())
            // Please connect controller

        for (int i = 0; i < myPlayerArray.Length; i++)
        {
            var player = myPlayerArray[i];
            if (iM.IsPressed(player.Run, i))
                // Let the player run

            if (iM.IsPressed(player.Jump, i))
                // Let the player jump
        }
    }
}
```

### Handling player selection

```csharp
public class Peace : Game
{
    Input JoinGame = Input.FaceButtonDown;
    Input LeaveGame = Input.FaceButtonRight;
    Input StartGame = Input.Start;
    List<int> activePlayerIds;

    // In multiplayer initialisation code
    public Peace()
    {
        activePlayerIds = new List<int>();
        iM = new InputManager(this)
        {
            PlayerCount = 8 // Max joinable players
        };
    }

    // In Update code
    protected override void Update(GameTime gameTime)
    {
        var count = iM.GetPlayerCount();
        bool start = false;
        for (int i = 0; i < count; i++)
        {
            if (activePlayerIds.Contains(i))
            {
                if (iM.IsPressed(LeaveGame, i))
                    activePlayerIds.RemoveAll(id => id == i);
                else if (iM.IsPressed(StartGame, i))
                    start = true;
            }
            else if (iM.IsPressed(JoinGame, i))
                activePlayerIds.Add(i);

        }
        if (start)
        {
            iM.SetActivePlayers(activePlayerIds);
            var playerCount = iM.GetPlayerCount();

            // Do something with player count...
        }
    }
}
```

## Porting Cross-platform

If you plan to write an controller first game you may end up supporting other
platforms. To add keyboard or mouse support

```csharp
iM.AddKeyboardPlayer(new Dictionary<Input, Keys>
{
    { Input.Up, Keys.Up },
    { Input.Left, Keys.Left },
    { Input.Right, Keys.Right },
    { Input.Down, Keys.Down },
    { Input.Back, Keys.Space }
});

iM.AddMousePlayer(new Dictionary<Input, MouseInput>
{
    { Input.Up, MouseInput.Button2 },
    { Input.Left, MouseInput.LeftButton },
    { Input.Right, MouseInput.RightButton },
    { Input.Down, MouseInput.Button1 },
    { Input.Back, MouseInput.MiddleButton }
});
```

# TODO

- Remove keyboardStates (&& mouse) if platform != Computer (just a simple
  boolean)
- Add
  [more deadzone options](https://web.archive.org/web/20150619053552/http://www.third-helix.com/2013/04/12/doing-thumbstick-dead-zones-right.html)

# Licence

This project is licenced under the MIT licence, Check [LICENCE](LICENCE) for
further details.
