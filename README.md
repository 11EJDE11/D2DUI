# D2DUI - Direct2D UI Library

**D2DUI** is a Direct2D.1-based UI window manager and control library. It provides hardware-accelerated rendering, Unicode support (including color emojis), and a set of UI controls.
![D2DUI](https://github.com/user-attachments/assets/5074dc1c-ffa3-4c11-ba65-a7941bb4f2b4)

## Features

- **Hardware-accelerated rendering** via Direct2D
- **Unicode support** including color emojis
- **Theme support**
- **Tab navigation** and Z-order management
- **Modal controls support**
- **Scrollable forms and panels**
- **Asset management** for images and audio
- **DPI support**

## Available Controls

| Control | Class Name | Description |
|---------|------------|-------------|
| **Form** | `clsForm` | Main UI Manager - the starting point for any D2DUI application |
| **Button** | `clsButton` | Standard push button with image support |
| **Checkbox** | `clsCheckbox` | Checkbox and radio button functionality |
| **Context Menu** | `clsContextMenu` | Right-click context menus |
| **Dropdown** | `clsDropDown` | Dropdown selection box |
| **Label** | `clsLabel` | Text labels with auto-size and ellipsis support |
| **Listview** | `clsListview` | Multi-column list with grid lines and multi-select |
| **Message Box** | `clsMessageBox` | Modal message dialogs |
| **Panel** | `clsPanel` | Container control with draggable property |
| **Progress Bar** | `clsProgressBar` | Progress indicator |
| **Scrollbar** | `clsScrollbar` | Horizontal and vertical scrollbars |
| **Slider** | `clsSlider` | Value selection slider with tooltips |
| **Textbox** | `clsTextbox` | Single and multi-line text input |
| **Tooltip** | `clsTooltip` | Hover tooltips |
| **Video Player** | `clsVideoPlayer` | Video playback control using IMFMediaFoundation |

## Quick Start Example

Here's a minimal example showing how to create a D2DUI form with a button:

```vb
Option Explicit

Private m_UIManager As D2DUI.clsForm
Private WithEvents m_Button As D2DUI.clsButton

Private Sub Form_Load()
    'Create and initialize the UI manager
    Set m_UIManager = New D2DUI.clsForm
    With m_UIManager
        .Width = 800
        .Height = 600
        .UseVSync = False
        .Initialize Me.hWnd, False, False
    End With
    
    'Create a button
    Set m_Button = New D2DUI.clsButton
    m_UIManager.AddControl m_Button
    With m_Button
        .Caption = "Click Me!"
        .Width = 100
        .Height = 30
        .Left = 20
        .Top = 20
        .Enabled = True
        .Visible = True
    End With
    
    StartRenderLoop
End Sub

Private Sub StartRenderLoop()
    Do While True
        m_UIManager.Render
        DoEvents
    Loop
End Sub

Private Sub m_Button_Click()
    MsgBox "Button clicked!"
End Sub

Private Sub Form_Unload(Cancel As Integer)
    Set m_Button = Nothing
    Set m_UIManager = Nothing
End Sub
```

## Control Events

All controls provide these standard events:

```vb
Public Event Click()
Public Event DoubleClick()
Public Event TripleClick()
Public Event MouseDown(Button As Integer, Shift As Integer, x As Single, y As Single)
Public Event MouseUp(Button As Integer, Shift As Integer, x As Single, y As Single)
Public Event MouseMove(Button As Integer, Shift As Integer, x As Single, y As Single)
Public Event MouseHover(x As Single, y As Single)
Public Event MouseEnter()
Public Event MouseLeave()
Public Event MouseWheel(delta As Long)
Public Event GotFocus()
Public Event LostFocus()
Public Event ValueChanged(IsPressed As Boolean)
Public Event Drag(x As Single, y As Single)
Public Event DragStart(x As Single, y As Single)
Public Event DragEnd(x As Single, y As Single)
Public Event KeyDown(keyCode As Integer, Shift As Integer)
Public Event KeyPress(KeyAscii As Integer)
Public Event KeyUp(keyCode As Integer, Shift As Integer)
```

## Theming

Create custom themes to style all controls consistently:

```vb
Private Function CreateTheme() As clsTheme
    Dim theme As clsTheme
    Set theme = New clsTheme
    
    With theme
        .BorderColor = ColorF(SteelBlue)
        .BackColor = ColorF2(0.9, 0.9, 0.9)
        .ForeColor = ColorF2(0, 0, 0)
        .PressedColor = ColorF2(0.7, 0.7, 0.8)
        .HoverColor = ColorF2(0.8, 0.8, 0.9)
        .DisabledBackColor = ColorF2(0.85, 0.85, 0.85)
        .DisabledForeColor = ColorF2(0.6, 0.6, 0.6)
        .FontName = "Segoe UI"
        .FontSize = 12
        .FontWeight = DWRITE_FONT_WEIGHT_NORMAL
        .BorderCornerRadius = 3
        .ShowBorder = True
        .BorderThickness = 4
    End With
    
    Set CreateTheme = theme
End Function

'Apply theme to UI Manager
Set m_UIManager.Theme = CreateTheme()
```

## Creating Custom Controls

To create custom controls, implement one or both interfaces:

- **`ID2DUIControl`** - For standard controls
- **`ID2DUIContainer`** - For controls that contain other controls
- Forward events to the base control or base container (base container automatically forwards to all child controls)

### ID2DUIControl Interface

#### Properties
- `Left`, `Top`, `Width`, `Height` - Control positioning
- `Visible`, `Enabled` - Control state
- `HasFocus` - Focus state
- `Tag` - User-defined data
- `ToolTipText` - Tooltip text
- `ShowBorder`, `BorderThickness`, `BorderCornerRadius`, `BorderStyle` - Border styling
- `BackColor`, `BorderColor` - Colors
- `Cursor` - Cursor type when hovering
- `TabStop`, `TabOrder` - Tab navigation
- `ZOrder` - Drawing order
- `Parent` - Parent container
- `ownerForm` - Owner form reference
- `Tooltip` - Tooltip object
- `RenderTarget` - Direct2D render target

#### Methods
```vb
Public Sub ApplyTheme(Theme As clsTheme)
Public Sub RemoveControl()
Public Sub Invalidate()
Public Sub Initialize()
Public Sub Render()
Public Function HitTest(x As Single, y As Single) As Boolean
Public Function GetAbsoluteLeft() As Single
Public Function GetAbsoluteTop() As Single
Public Function IsDragging() As Boolean

'Mouse event handlers
Public Sub HandleMouseDoubleClick(Button As Integer, Shift As Integer, x As Single, y As Single)
Public Sub HandleMouseMove(Button As Integer, Shift As Integer, x As Single, y As Single)
Public Sub HandleMouseDown(Button As Integer, Shift As Integer, x As Single, y As Single)
Public Sub HandleMouseUp(Button As Integer, Shift As Integer, x As Single, y As Single)
Public Sub HandleMouseEnter()
Public Sub HandleMouseLeave()
Public Sub HandleMouseHover(x As Single, y As Single)
Public Sub HandleMouseWheel(delta As Long)

'Keyboard event handlers
Public Sub HandleKeyDown(keyCode As Integer, Shift As Integer)
Public Sub HandleKeyUp(keyCode As Integer, Shift As Integer)
Public Sub HandleKeyPress(KeyAscii As Integer)

'Drag event handlers
Public Sub HandleDragStart(x As Single, y As Single)
Public Sub HandleDrag(x As Single, y As Single)
Public Sub HandleDragEnd(x As Single, y As Single)
```

### ID2DUIContainer Interface

#### Properties
- `Theme` - Container theme
- `HandlesTabbing` - Whether container manages tab navigation

#### Methods
```vb
' Cursor management
Public Function GetCursorAtPoint(x As Single, y As Single) As D2DUICursorType

'Radio button group management
Public Sub SetRadioGroupValue(groupName As String, checkedControl As clsCheckbox)

'Modal control management
Public Sub SetModalControl(Control As ID2DUIControl)
Public Sub ClearModalControl()

'Layout management
Public Sub TabOrderChange()
Public Sub ZOrderChange()
Public Sub HandleResize()

'Mouse capture
Public Sub CaptureControl(Control As ID2DUIControl)
Public Sub ReleaseCapture()
Public Function IsCapturing(Control As ID2DUIControl) As Boolean

'Control management
Public Function GetControlCount() As Long
Public Function GetControlAt(Index As Long) As ID2DUIControl
Public Function GetControlByName(Name As String) As ID2DUIControl
Public Sub AddControl(Control As ID2DUIControl)
Public Sub RemoveControl(Control As ID2DUIControl)
Public Sub BringToFront(Control As ID2DUIControl)
Public Sub SendToBack(Control As ID2DUIControl)
Public Function GetControlAtPoint(x As Single, y As Single) As ID2DUIControl
Public Sub SetFocusControl(Control As ID2DUIControl)
```

## Cursor Types

```vb
Public Enum D2DUICursorType
    CursorDefault = 0
    CursorArrow = 1
    CursorHand = 2
    CursorIBeam = 3
    CursorCross = 4
    CursorWait = 5
    CursorHelp = 6
    CursorSizeNS = 7     ' North-South resize
    CursorSizeWE = 8     ' West-East resize
    CursorSizeNWSE = 9   ' Northwest-Southeast resize
    CursorSizeNESW = 10  ' Northeast-Southwest resize
    CursorSizeAll = 11   ' Move/size all directions
    CursorNo = 12        ' Not allowed
    CursorUpArrow = 13
End Enum
```

## Border Styles

```vb
Public Enum D2DUIBorderStyle
    d2dSolid = 0
    d2dDotted = 1
End Enum
```

## Advanced Features

### Asset Management
```vb
Set m_Assets = New clsAssetManager
m_Assets.Initialize m_UIManager.RenderTarget

'Load an image
m_Assets.LoadImage "my_image", "C:\path\to\image.png", True
Set myButton.Image = m_Assets.GetImageProxy("my_image")
```
Note: Images are stored as a clsBitmap - you can access the underlying ID2D1Bitmap1 through the get Bitmap method.
Audio uses DirectSound.

### Container Controls
Containers (Form, Panel) can host other controls and automatically forward events to child controls:

```vb
'In your custom container control
Public Sub ID2DUIControl_HandleMouseMove(Button, Shift, X, Y)
    'Forward to base container which handles child controls
    m_BaseContainer.ForwardMouseMove Button, Shift, X, Y
End Sub
```
Controls containing other controls (such as the textbox which is made up of a text input control and a scrollbar control) will need to implement ID2DUIContainer.
