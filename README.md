# Color Squares Game

A SwiftUI-based interactive game where users can place colored squares on a canvas and track their counts.


https://github.com/user-attachments/assets/5bcddffc-e978-4ffc-98f7-2f834d250f04




## Interview Questions

1. **Stack View and Clicking Functionality:**
   How would you implement a touch-responsive canvas in SwiftUI that allows placing custom views (like colored squares) at the touch location?

3. **Number Counting:**
   Explain how you would maintain a count of items by category (like color) and ensure this count is updated correctly when items are added or removed.

4. **Undo Tab Item:**
   Describe the data structures and logic you would use to implement an undo feature in a drawing or game app. How would you ensure that the app state is correctly reverted?

5. **Redo Tab Item:**
   After implementing an undo feature, how would you add a redo functionality? What additional considerations are there when implementing redo alongside undo?

## Features

- Interactive canvas for placing colored squares
- Color selection toolbar with visual feedback
- Dynamic counting of squares for each color
- Undo and redo functionality
- Clean and intuitive user interface

## Implementation Highlights

### 1. Stack View and Clicking Functionality

The game uses a ZStack to layer the white canvas, placed squares, and the color selection toolbar. The main canvas is responsive to touch, allowing users to add squares wherever they tap.

```swift
ZStack {
    Color.white.edgesIgnoringSafeArea(.all)
        .onTapGesture { location in
            addSquare(point: location)
        }
    
    // Placed squares
    ForEach(squares) { square in
        // ... square view ...
    }
    
    // Color selection toolbar
    VStack {
        Spacer()
        HStack {
            // ... color buttons ...
        }
    }
}
```

### 2. Number Counting

Each square displays a number representing its count for that specific color. This is achieved by maintaining a dictionary of color counts and updating it as squares are added or removed.

```swift
@State private var colorCounts: [Color: Int] = [:]

private func addSquare(point: CGPoint) {
    colorCounts[selectedColor, default: 0] += 1
    let newSquare = Square(color: selectedColor, position: point, count: colorCounts[selectedColor]!)
    squares.append(newSquare)
}
```

### 3. Undo Functionality

The undo feature allows users to remove the last placed square. This is implemented using an array to store the squares and a separate array for removed squares.

```swift
@State private var squares: [Square] = []
@State private var removedSquaresStack: [Square] = []

private func undo() {
    guard let lastSquare = squares.popLast() else { return }
    removedSquaresStack.append(lastSquare)
    colorCounts[lastSquare.color, default: 0] -= 1
    if colorCounts[lastSquare.color] == 0 {
        colorCounts.removeValue(forKey: lastSquare.color)
    }
}
```

### 4. Redo Functionality

The redo feature allows users to restore previously undone actions. This is implemented by managing the `removedSquaresStack`.

```swift
private func redo() {
    guard let lastRemoved = removedSquaresStack.popLast() else { return }
    squares.append(lastRemoved)
    colorCounts[lastRemoved.color, default: 0] += 1
}
```

## Conclusion

This Color Squares Game demonstrates key SwiftUI concepts including state management, custom drawing, and user interaction. The implementation of undo and redo functionality showcases more advanced state handling techniques.
