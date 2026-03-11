# Event Propagation (JavaScript)

Event propagation means **how an event travels through nested HTML elements when something happens (like a click).**

Example HTML structure:

```html
<div id="grandparent">
  Grandparent
  <div id="parent">
    Parent
    <div id="child">
      Child
    </div>
  </div>
</div>
```

Structure:

Grandparent
└── Parent
  └── Child

---

# 1. Event Bubbling (Default Behavior)

By default, JavaScript events **bubble up**.

That means the event starts from the **clicked element** and moves **up to its parent elements**.

### Code Example

```html
<!DOCTYPE html>
<html>
<body>

<div id="grandparent">
  Grandparent
  <div id="parent">
    Parent
    <div id="child">
      Child
    </div>
  </div>
</div>

<script>

const grandparent = document.querySelector("#grandparent");
const parent = document.querySelector("#parent");
const child = document.querySelector("#child");

grandparent.addEventListener("click", () => {
  console.log("Grandparent clicked");
});

parent.addEventListener("click", () => {
  console.log("Parent clicked");
});

child.addEventListener("click", () => {
  console.log("Child clicked");
});

</script>

</body>
</html>
```

### What happens when you click **Child**

Console output:

```
Child clicked
Parent clicked
Grandparent clicked
```

### Why?

Because the event travels like this:

Child → Parent → Grandparent

This is called **Event Bubbling**.

---

# 2. stopPropagation()

Sometimes you want the event to **stop at the clicked element**.

For that we use:

`event.stopPropagation()`

### Code Example

```javascript
child.addEventListener("click", (e) => {
  e.stopPropagation();
  console.log("Child clicked");
});
```

### Output when clicking child

```
Child clicked
```

Parent and grandparent **will not run** because the event is stopped.

---

# 3. Event Capturing

Capturing is the **opposite of bubbling**.

The event moves **from the top element down to the target element**.

To enable capturing, pass **true** as the third parameter in `addEventListener`.

### Code Example

```javascript
grandparent.addEventListener("click", () => {
  console.log("Grandparent capture");
}, true);

parent.addEventListener("click", () => {
  console.log("Parent capture");
}, true);

child.addEventListener("click", () => {
  console.log("Child capture");
}, true);
```

### Output when clicking child

```
Grandparent capture
Parent capture
Child capture
```

Event flow:

Grandparent → Parent → Child

---

# 4. Full Event Flow Example

This example shows **capturing and bubbling together**.

```javascript
grandparent.addEventListener("click", () => {
  console.log("Grandparent Capture");
}, true);

parent.addEventListener("click", () => {
  console.log("Parent Capture");
}, true);

child.addEventListener("click", () => {
  console.log("Child Target");
});

parent.addEventListener("click", () => {
  console.log("Parent Bubble");
});

grandparent.addEventListener("click", () => {
  console.log("Grandparent Bubble");
});
```

### Output

```
Grandparent Capture
Parent Capture
Child Target
Parent Bubble
Grandparent Bubble
```

---

# Simple Way to Remember

Bubbling
Child → Parent → Grandparent

Capturing
Grandparent → Parent → Child

---

✅ **Short summary**

Event propagation has 3 phases:

1. Capturing (top → down)
2. Target (clicked element)
3. Bubbling (bottom → up)

Default behavior in JavaScript is **bubbling**.

