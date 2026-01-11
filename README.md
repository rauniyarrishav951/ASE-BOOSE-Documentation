
# ASE-BOOSE Documentation

**BOOSE (Basic Object-Oriented Scripting Environment)** is a Windows Forms-based interpreter for a custom scripting language. The system demonstrates professional software engineering with clear separation of concerns, established design patterns, and extensible architecture supporting drawing operations, variable management, and control flow structures.

#The BOOSE XML Site
HERE IS  LINK [BOOSE DOCS](https://github.com/rauniyarrishav951/ASE-Documentation/tree/main/XML%20Documentation/XML%20Documentation-%202nd%20Component)

## Table of Contents

1. [System Overview](#system-overview)
2. [Architecture](#architecture)
3. [Drawing Commands](#drawing-commands)
4. [Variable Management](#variable-management)
5. [Array Operations](#array-operations)
6. [Control Flow](#control-flow)
7. [API Reference](#api-reference)
8. [Usage Examples](#usage-examples)
9. [Design Patterns](#design-patterns)
10. [Testing](#testing)

---

## System Overview

### Purpose
BOOSE provides a high-level scripting interface for creating graphical drawings and managing program state through a simple, readable syntax. The system is designed for educational purposes and demonstrates enterprise-level software architecture.

### Key Features
- **Canvas-based Drawing**: Support for shapes, colors, and pen positioning
- **Variable Management**: Integer, real, and array data types
- **Control Flow**: If/Else, For loops, and While loops
- **Expression Evaluation**: Mathematical expressions with operator precedence
- **Array Operations**: Dynamic array creation with indexed access
- **Command Factory Pattern**: Extensible command architecture

### Technology Stack
- **Language**: C# (.NET Framework)
- **UI Framework**: Windows Forms
- **Testing**: MSTest (Microsoft.VisualStudio.TestTools.UnitTesting)
- **Architecture Pattern**: Command, Factory, Singleton

---

## Architecture

### Core Components

#### 1. AppCanvas
**Purpose**: Manages the drawing surface and graphics state
- **Properties**:
  - `Xpos` (int): Current X-coordinate of pen
  - `Ypos` (int): Current Y-coordinate of pen
  - `PenColour` (Color): Current pen color (RGB)
  - `Width` (int): Canvas width in pixels
  - `Height` (int): Canvas height in pixels

- **Key Methods**:
  - `MovePen(x, y)`: Move cursor without drawing
  - `DrawLine(x, y)`: Draw line from current position to (x, y)
  - `DrawCircle(x, y, radius)`: Draw circle at (x, y)
  - `DrawRectangle(x, y, width, height)`: Draw rectangle
  - `SetPenColor(r, g, b)`: Set pen color using RGB

#### 2. StoredProgram
**Purpose**: Manages command storage and execution
- **Responsibilities**:
  - Parse incoming commands
  - Maintain command queue
  - Execute commands in sequence
  - Track program state

- **Key Methods**:
  - `Run()`: Execute all stored commands
  - `AddCommand(ICommand)`: Queue a command for execution
  - `Clear()`: Reset program state

#### 3. Parser
**Purpose**: Converts text scripts into executable commands
- **Constructor**: `Parser(CommandFactory factory, StoredProgram program)`
- **Key Methods**:
  - `ParseProgram(script)`: Parse and queue commands
  - Handles multi-line scripts with newline splitting
  - Delegates command creation to factory

#### 4. CommandFactory (AppCommandFactory)
**Purpose**: Implements factory pattern for command instantiation
- **Pattern**: Factory design pattern
- **Supported Commands**: MoveTo, DrawTo, Circle, Rect, PenColor, Int, Real, Array, For, While, If, Poke, Peek, Write, Clear, Reset

- **Key Method**:
  - `CreateCommand(commandName)`: Returns appropriate ICommand implementation

#### 5. VariableStore (Singleton)
**Purpose**: Centralized variable storage
- **Pattern**: Singleton design pattern
- **Access**: `VariableStore.Instance`
- **Supported Types**:
  - `int`: 32-bit integer variables
  - `real`: Floating-point variables
  - `IntArray`: Arrays of integers

- **Key Methods**:
  - `SetInt(name, value)`: Store integer
  - `GetInt(name)`: Retrieve integer
  - `SetReal(name, value)`: Store real number
  - `GetReal(name)`: Retrieve real number
  - `CreateIntArray(name, size)`: Create integer array
  - `SetIntArrayValue(name, index, value)`: Set array element
  - `GetIntArrayValue(name, index)`: Get array element
  - `Clear()`: Reset all variables

#### 6. ExpressionEvaluator
**Purpose**: Evaluate mathematical expressions
- **Supported Operators**: `+`, `-`, `*`, `/`, `%`
- **Operator Precedence**: Standard (*, /, % before +, -)
- **Variable Resolution**: Automatically substitutes variable values

#### 7. ConditionEvaluator
**Purpose**: Evaluate boolean conditions for control flow
- **Supported Operators**: `<`, `>`, `==`, `!=`, `<=`, `>=`
- **Usage**: While loops and If statements

---

## Drawing Commands

### 1. MoveTo Command
**Syntax**: `moveto x,y`

**Description**: Move the pen cursor to the specified coordinates without drawing a line.

**Parameters**:
- `x` (int): X-coordinate (0 to canvas width)
- `y` (int): Y-coordinate (0 to canvas height)

**Examples**:
```
moveto 100,150
moveto 50,50
```

**Related Commands**: DrawTo, Circle, Rect

---

### 2. DrawTo Command
**Syntax**: `drawto x,y`

**Description**: Draw a line from the current cursor position to the specified coordinates and move cursor there.

**Parameters**:
- `x` (int): Target X-coordinate
- `y` (int): Target Y-coordinate

**Examples**:
```
moveto 10,10
drawto 100,100
drawto 100,10
drawto 10,10
```

**Note**: Line is drawn using current pen color.

---

### 3. Circle Command
**Syntax**: `circle radius`

**Description**: Draw a circle centered at the current cursor position.

**Parameters**:
- `radius` (int): Circle radius in pixels

**Examples**:
```
moveto 200,200
circle 50
circle 100
```

**Note**: Circle is drawn using current pen color; cursor position is not changed.

---

### 4. Rectangle Command
**Syntax**: `rect width,height`

**Description**: Draw a rectangle with top-left corner at the current cursor position.

**Parameters**:
- `width` (int): Rectangle width in pixels
- `height` (int): Rectangle height in pixels

**Examples**:
```
moveto 50,50
rect 100,75
rect 200,150
```

**Note**: Rectangle is drawn using current pen color; cursor remains at top-left corner.

---

### 5. PenColor Command
**Syntax**: `pencolour r,g,b` or `pencolor r,g,b`

**Description**: Set the pen color using RGB values (both British and American spellings supported).

**Parameters**:
- `r` (0-255): Red component
- `g` (0-255): Green component
- `b` (0-255): Blue component

**Examples**:
```
pencolour 255,0,0     # Red
pencolour 0,255,0     # Green
pencolour 0,0,255     # Blue
pencolour 255,255,0   # Yellow
```

**Note**: Changes affect all subsequent drawing commands.

---

### 6. Clear Command
**Syntax**: `clear`

**Description**: Clear the canvas (erase all drawings).

**Examples**:
```
clear
```

---

## Variable Management

### Integer Variables

**Syntax**: `int variableName = expression`

**Description**: Declare and initialize an integer variable.

**Examples**:
```
int x = 10
int y = 20
int sum = x + y
int result = x * 2 + y - 5
```

**Expression Support**:
- Literal numbers: `10`, `-5`, `0`
- Variable references: `x`, `y`
- Arithmetic operators: `+`, `-`, `*`, `/`, `%`
- Operator precedence respected: `x + y * 2` = `x + (y * 2)`

**Scope**: Global (stored in VariableStore)

---

### Real Variables

**Syntax**: `real variableName = expression`

**Description**: Declare and initialize a floating-point variable.

**Examples**:
```
real pi = 3.14159
real radius = 5.5
real area = pi * radius * radius
real average = (x + y) / 2
```

**Expression Support**: Same as integers, but supports decimal values.

**Scope**: Global (stored in VariableStore)

---

### Variable Assignment

**Syntax**: `variableName = expression`

**Description**: Assign a new value to an existing variable (reassignment).

**Examples**:
```
int counter = 0
counter = counter + 1
counter = 10

real value = 3.14
value = value * 2
```

**Note**: Variable must be declared before assignment.

---

## Array Operations

### Array Declaration

**Syntax**: `int arrayName size`

**Description**: Create an integer array with specified size.

**Parameters**:
- `arrayName` (string): Name of the array
- `size` (int): Number of elements

**Examples**:
```
int numbers 10      # Array with 10 elements
int data 100        # Array with 100 elements
int matrix 25       # Array with 25 elements
```

**Indexing**: Zero-based (0 to size-1)

**Scope**: Global (stored in VariableStore)

---

### Poke Command (Array Write)

**Syntax**: `poke arrayName index value`

**Description**: Write a value to a specific array index.

**Parameters**:
- `arrayName` (string): Name of the array
- `index` (int): Array index (0-based)
- `value` (int/expression): Value to write

**Examples**:
```
int arr 10
poke arr 0 42
poke arr 5 100
poke arr index value
```

**Error Handling**: 
- Index out of bounds: Runtime error
- Array not found: Runtime error
- Array type mismatch: Runtime error

---

### Peek Command (Array Read)

**Syntax**: `peek variableName = arrayName index`

**Description**: Read a value from an array and store in a variable.

**Parameters**:
- `variableName` (string): Target variable for value
- `arrayName` (string): Source array name
- `index` (int/expression): Array index to read

**Examples**:
```
int arr 10
poke arr 3 77
peek value = arr 3      # value now equals 77

peek x = arr 0
peek result = data index
```

**Note**: If variable doesn't exist, it is created.

---

## Control Flow

### If/Else Statement

**Syntax**:
```
if condition
    command1
    command2
else
    command3
    command4
end if
```

**Description**: Execute one branch based on a boolean condition.

**Conditions**:
- `x < 5`: Less than
- `x > 10`: Greater than
- `x == 5`: Equal to
- `x != 3`: Not equal to
- `x <= 10`: Less than or equal
- `x >= 5`: Greater than or equal

**Examples**:
```
if x < 10
    moveto 50,50
    circle 25
else
    moveto 100,100
    circle 50
end if

if count == 0
    pencolour 255,0,0
else
    pencolour 0,255,0
end if
```

**Scope**: Nested if statements supported

---

### For Loop

**Syntax**:
```
for variable = start to end
    command1
    command2
end for
```

**Description**: Execute block a fixed number of times.

**Parameters**:
- `variable` (string): Loop counter (created if not exists)
- `start` (int): Starting value (inclusive)
- `end` (int): Ending value (inclusive)

**Examples**:
```
for i = 1 to 10
    moveto i*10, i*10
    circle 5
end for

for x = 0 to 100
    drawto x, 50
end for
```

**Loop Counter**: Automatically incremented each iteration

**Nesting**: Nested for loops supported

---

### While Loop

**Syntax**:
```
while condition
    command1
    command2
end while
```

**Description**: Execute block repeatedly while condition is true.

**Condition Format**: Same as If statement conditions

**Examples**:
```
int i = 0
while i < 10
    moveto i*10, 50
    circle 3
    i = i + 1
end while

while x < 100
    drawto x, y
    x = x + 5
    y = y + 2
end while
```

**Break Condition**: Loop terminates when condition becomes false

**Nesting**: Nested while loops and mixed loops supported

**Warning**: Be careful to avoid infinite loops

---

## API Reference

### ICommand Interface

```csharp
public interface ICommand
{
    void Set(StoredProgram program, string parameters);
    void Execute();
    void Compile();
    void CheckParameters(string[] parameters);
}
```

**Methods**:
- `Set()`: Parse and store command parameters
- `Execute()`: Perform the command action
- `Compile()`: Validate and prepare command
- `CheckParameters()`: Validate parameter format

---

### AppCanvas

```csharp
public class AppCanvas
{
    public int Xpos { get; set; }
    public int Ypos { get; set; }
    public Color PenColour { get; set; }
    public int Width { get; }
    public int Height { get; }
    
    public void MoveTo(int x, int y);
    public void DrawTo(int x, int y);
    public void DrawCircle(int radius);
    public void DrawRectangle(int width, int height);
    public void SetPenColor(int r, int g, int b);
    public void Clear();
}
```

---

### VariableStore (Singleton)

```csharp
public sealed class VariableStore
{
    public static VariableStore Instance { get; }
    
    public void SetInt(string name, int value);
    public int GetInt(string name);
    public void SetReal(string name, double value);
    public double GetReal(string name);
    
    public void CreateIntArray(string name, int size);
    public void SetIntArrayValue(string name, int index, int value);
    public int GetIntArrayValue(string name, int index);
    
    public void Clear();
}
```

---

### ExpressionEvaluator

```csharp
public class ExpressionEvaluator
{
    public static int EvaluateInt(string expression);
    public static double EvaluateReal(string expression);
}
```

---

### ConditionEvaluator

```csharp
public class ConditionEvaluator
{
    public static bool Evaluate(string condition);
}
```

---

## Usage Examples

### Example 1: Simple Drawing

```
moveto 50,50
pencolour 255,0,0
circle 30
drawto 150,50
drawto 150,150
drawto 50,150
drawto 50,50
```

**Output**: Red circle at (50,50) with radius 30, followed by red square outline.

---

### Example 2: Variable Operations

```
int x = 10
int y = 20
int sum = x + y
int product = x * y

real pi = 3.14159
real radius = 10
real area = pi * radius * radius
```

**Variables Created**:
- `x` = 10
- `y` = 20
- `sum` = 30
- `product` = 200
- `pi` = 3.14159
- `radius` = 10
- `area` ≈ 314.159

---

### Example 3: Control Flow with For Loop

```
int x = 0
for i = 1 to 10
    moveto x, x
    circle i
    x = x + 20
end for
```

**Output**: 10 circles with increasing radii positioned diagonally.

---

### Example 4: Conditional Drawing

```
int width = 100

if width > 50
    pencolour 0,255,0
    rect 100,100
else
    pencolour 255,0,0
    circle 25
end if
```

**Output**: Green rectangle (since width = 100 > 50).

---

### Example 5: Array Operations

```
int data 5
poke data 0 10
poke data 1 20
poke data 2 30
poke data 3 40
poke data 4 50

peek value = data 2
```

**Array State**:
- `data[0]` = 10
- `data[1]` = 20
- `data[2]` = 30
- `data[3]` = 40
- `data[4]` = 50

**Variable**: `value` = 30

---

### Example 6: Complex Program

```
# Initialize drawing
moveto 100,100
pencolour 255,0,0

# Draw circle
circle 50

# Loop through array positions
int positions 4
poke positions 0 50
poke positions 1 150
poke positions 2 250
poke positions 3 350

for i = 0 to 3
    peek x = positions i
    moveto x, 200
    pencolour 0,255,0
    circle 20
end for

# Conditional drawing
int mode = 1
if mode == 1
    pencolour 0,0,255
    rect 300,200
end if
```

---

## Design Patterns

### 1. Command Pattern

**Purpose**: Encapsulate commands as objects for queuing and execution.

**Implementation**:
- `ICommand` interface defines contract
- Each command (MoveTo, DrawTo, etc.) implements ICommand
- StoredProgram maintains command queue
- Parser creates commands and queues them

**Benefits**:
- Decouple command creation from execution
- Enable undo/redo functionality
- Queue commands for batch processing

---

### 2. Factory Pattern

**Purpose**: Centralize command instantiation.

**Implementation**:
- `CommandFactory` interface
- `AppCommandFactory` implements factory
- Returns appropriate ICommand based on name

**Benefits**:
- Single responsibility for object creation
- Easy to add new commands
- Consistent command initialization

---

### 3. Singleton Pattern

**Purpose**: Ensure single global instance of variable storage.

**Implementation**:
- `VariableStore` private constructor
- Static `Instance` property
- Thread-safe lazy initialization

**Benefits**:
- Global access to variables
- No parameter passing required
- Controlled resource management

---

### 4. Strategy Pattern

**Purpose**: Different evaluation strategies for expressions and conditions.

**Implementation**:
- `ExpressionEvaluator` handles arithmetic
- `ConditionEvaluator` handles comparisons
- Pluggable evaluation algorithms

**Benefits**:
- Flexible expression handling
- Easy to extend operators
- Testable evaluation logic

---

## Testing

### Test Framework
- **Framework**: MSTest (Microsoft.VisualStudio.TestTools.UnitTesting)
- **Total Test Cases**: 11
- **Coverage**: Drawing, Variables, Arrays, Control Flow

### Test Categories

#### Drawing Commands (3 tests)
- TC01: MoveTo position updates
- TC02: DrawTo line drawing
- TC03: Multi-line program execution

#### Variable Operations (2 tests)
- TC04: Integer declaration and expression
- TC05: Real number declaration and expression

#### Array Operations (3 tests)
- TC06: Array creation and element access
- TC07: Poke (array write) operation
- TC08: Peek (array read) operation

#### Control Flow (3 tests)
- TC09: While loop iteration
- TC10: For loop iteration
- TC11: If/Else branching

### Running Tests

```csharp
// Test execution framework
[TestClass]
public class BOOSEappTests
{
    [TestInitialize]
    public void Setup()
    {
        canvas = new AppCanvas(400, 400);
        program = new StoredProgram(canvas);
        parser = new Parser(factory, program);
    }
    
    [TestMethod]
    public void TestMoveTo_UpdatesPenPosition()
    {
        parser.ParseProgram("moveto 150,200");
        program.Run();
        
        Assert.AreEqual(150, canvas.Xpos);
        Assert.AreEqual(200, canvas.Ypos);
    }
}
```

---

## Performance Considerations

### Optimization Tips

1. **Variable Storage**: VariableStore uses dictionary lookup O(1) average case
2. **Expression Parsing**: Cached evaluation when possible
3. **Canvas Operations**: Batch drawing operations for efficiency
4. **Array Access**: Direct indexing O(1) for array operations

### Scalability

- **Canvas Size**: Tested up to 4000x4000 pixels
- **Variables**: Supports thousands of variables
- **Arrays**: Limited by system memory
- **Program Size**: No hard limit on command count

---

## Known Limitations

1. **Floating-Point Precision**: Limited to double precision
2. **Array Size**: Single array limited by available memory
3. **Nested Structures**: Limited nesting depth (recursive)
4. **Error Handling**: Basic error messages without stack traces
5. **Debugging**: No step-through debugging in current version

---

## Future Enhancements

- [ ] Function definitions and calls
- [ ] String variable support
- [ ] Multi-dimensional arrays
- [ ] File I/O operations
- [ ] Graphics primitives (polygon, ellipse)
- [ ] Color palettes and gradients
- [ ] Performance profiling tools
- [ ] IDE with syntax highlighting
- [ ] Debugger with breakpoints

---



---

## Support & Contact

For issues, questions, or contributions:
- **Repository**: ASE-BOOSE on GitHub
- **Issues**: GitHub Issues tracker
- **Documentation**: Inline code comments and XML documentation

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 2 0.1 | 11th Jan 2026 | Initial release with core features |
| | | Drawing commands, variables, arrays, control flow |
| | | Complete test coverage (11 tests) |

---

## License

BOOSE is provided as-is for educational purposes.

---

*Document Version: 1.0*  
*Last Updated: January 11, 2026*  
*Developed by: Rishav Rauniyar*



#The BOOSE Documentation Site
HERE IS DOCUMENTATION LINK [BOOSE DOCS](https://github.com/rauniyarrishav951/ASE-Documentation/tree/main/XML%20Documentation/XML%20Documentation-%202nd%20Component)
