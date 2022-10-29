# Midterm 2 Question 2

## 1. Duplicated code 

There are a lot of code blocks that resemble each other, thus we need to extract the duplicated code to a new method.

### `drawDomainMarker()` and `drawRangeMarker()`

The code in `drawDomainMarker()` and `drawRangeMarker()` are overtly similar with only a few variables differences. 
Thus, I have extracted the different variables and made a common method `drawAxisMarker()` for these two methods.


## 2. Feature envy

After `drawAxisMarker()` is made (*it no longer exists due to refactoring*), I realized that the method heavily depends on the `Marker` class.
So I have moved this method into `Marker.java`. In `AbstractXYItemRenderer.java`, `drawDomainMarker()` and `drawRangeMarker()` simply generate the params and call `marker.drawAxis()`.

## 3. Instance of

Note that in the method, there is the following piece of code.

```java
public class Marker {
    public void drawAxis() {
        if (marker instanceof ValueMarker) {
            // draw ValueMarker
        }
        else if (marker instanceof IntervalMarker) {
            // draw IntervalMarker
        }
    }
} 
```
In order to get rid of `instanceof`, we made its subclass to handle the draw function like the following code snippet:
```java
public class Marker {
    public void drawAxis() {
        // Do nothing
        // Cannot make this method abstract due to CategoryMarker
    }
} 

public class ValueMarker extends Marker{
    @Override
    public void drawAxis() {
        // draw ValueMarker
    }
}

public class IntervalMarker extends Marker{
    @Override
    public void drawAxis() {
        // draw IntervalMarker
    }
}
```

Therefore, when `marker.drawAxis()` is called in `AbstractXYItemRenderer.java`, the marker should use its own `drawAxis()`.

## 4. Long method

The following code snippet is originally from `AbstractXYItemRenderer` class (*with original line numbers*):
![img01](./img/img01.jpg)

Since draw outline has its own logic, I decide to make a separate method called `drawMarkerOutline()` in `IntervalMarker` class.
Following images are the refactored code:
![img02](./img/img02.jpg)
![img03](./img/img03.jpg)
