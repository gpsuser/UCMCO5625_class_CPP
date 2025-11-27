
This version avoids using member initializer lists and instead assigns values within the constructor body, which was more common in pre-C++11 code. 

single parameter pre c++ 11

```c++
class Circle
{
private:
    double radius;

public:
    // Default constructor
    Circle()
    {
        radius = 0;
    }

    // Parameterized constructor
    Circle(double r)
    {
        radius = r;
    }

    // Getters
    double getRadius()
    {
        return radius;
    }

    double getArea()
    {
        return radius * radius * 3.14159;
    }

    double getCircumference()
    {
        return 2 * 3.14159 * radius;
    }

    double getDiameter()
    {
        return 2 * radius;
    }

    // Setters
    void setRadius(double rad)
    {
        if (rad >= 0)
            radius = rad;
    }

    // Modifiers
    void scale(double factor)
    {
        radius *= factor;
    }
};
```

The default constructor plays a key role in object initialization, especially in older C++ codebases or when working with containers and frameworks. 

Here's why it's useful:

🧱 Why You Need a Default Constructor

- Automatic Initialization
If you create an array or container of Circle objects (e.g., Circle circles[10]; or std::vector<Circle> vec(5);), the default constructor is used to initialize each element. Without it, the compiler will throw an error unless another constructor is explicitly called.

- Flexibility in Object Creation
Sometimes you want to create a Circle object first and set its radius later:
```c++
Circle c;
c.setRadius(5.0);
```
- Without a default constructor, this pattern wouldn't compile.
- Compatibility with STL and Legacy Code

Many parts of the Standard Template Library (STL) and older APIs expect types to be default-constructible. If your class lacks a default constructor, it may not work with certain containers, algorithms, or serialization frameworks.
- Placeholder or Sentinel Objects
You might use a default-constructed object as a placeholder in a data structure, like a "null" or "empty" circle.

If you know you'll always initialize with a specific radius and never need a default object, you could omit it — but in practice, it's often safer and more versatile to include it. Want to see examples of STL behavior that breaks without it?






two parameters pre c++ 11

```c++
#include <string>

class Circle
{
private:
    double radius;
    std::string color;

public:
    // Default constructor
    Circle()
    {
        radius = 0;
        color = "black";  // default color
    }

    // Parameterized constructor
    Circle(double r, std::string c)
    {
        radius = r;
        color = c;
    }

    // Getters
    double getRadius()
    {
        return radius;
    }

    std::string getColor()
    {
        return color;
    }

    double getArea()
    {
        return radius * radius * 3.14159;
    }

    double getCircumference()
    {
        return 2 * 3.14159 * radius;
    }

    double getDiameter()
    {
        return 2 * radius;
    }

    // Setters
    void setRadius(double rad)
    {
        if (rad >= 0)
            radius = rad;
    }

    void setColor(std::string c)
    {
        color = c;
    }

    // Modifiers
    void scale(double factor)
    {
        radius *= factor;
    }
};
```

two parameters c++ 11
```c++
#include <string>

class Circle
{
private:
    double radius;
    std::string color;

public:
    // Default constructor
    Circle() : radius(0), color("black") {}

    // Parameterized constructor
    Circle(double r, std::string c) : radius(r), color(c) {}

    // Getters
    double getRadius() { return radius; }
    std::string getColor() { return color; }

    double getArea() { return radius * radius * 3.14159; }
    double getCircumference() { return 2 * 3.14159 * radius; }
    double getDiameter() { return 2 * radius; }

    // Setters
    void setRadius(double rad)
    {
        if (rad >= 0)
            radius = rad;
    }

    void setColor(std::string c)
    {
        color = c;
    }

    // Modifiers
    void scale(double factor)
    {
        radius *= factor;
    }
};
```
This version uses initializer lists

This style is more efficient and preferred in modern C++ because it directly initializes members rather than assigning them after construction

## Inheritance and constructors with two member variables in base class

When dealing with inheritance, constructors in derived classes need to call the appropriate base class constructors to ensure that all member variables are properly initialized.

If your `Circle` class has two members — say `radius` and `color` — then your derived `Sphere` class should call the appropriate base constructor that initializes both. Here's how the updated `Sphere` class would look using the **newer constructor style** (i.e. member initializer lists):

---

### 🧭 Updated `Sphere` Class (Derived from `Circle` with Two Members)

```cpp
#include <string>
#include <cmath>  // for pow

class Circle
{
protected:
    double radius;
    std::string color;

public:
    Circle() : radius(0), color("black") {}
    Circle(double r, std::string c) : radius(r), color(c) {}

    double getRadius() { return radius; }
    std::string getColor() { return color; }

    double getArea() { return radius * radius * 3.14159; }
    double getCircumference() { return 2 * 3.14159 * radius; }
    double getDiameter() { return 2 * radius; }

    void setRadius(double rad) { if (rad >= 0) radius = rad; }
    void setColor(std::string c) { color = c; }

    void scale(double factor) { radius *= factor; }
};

class Sphere : public Circle
{
public:
    // Default constructor
    Sphere() : Circle() {}

    // Constructor with radius and color
    Sphere(double r, std::string c) : Circle(r, c) {}

    // Sphere-specific methods
    double getVolume()
    {
        return (4.0 / 3.0) * 3.14159 * std::pow(radius, 3);
    }

    double getSurfaceArea()
    {
        return 4 * 3.14159 * radius * radius;
    }
};
```

---

### 🔍 Notes:
- `Sphere(double r, std::string c)` calls the base constructor `Circle(r, c)` to initialize both `radius` and `color`.
- `std::pow(radius, 3)` is used for volume calculation.
- You could also expose `getColor()` from `Circle` if you want to display or use the sphere's color.





