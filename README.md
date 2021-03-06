# Contributions

## React Wheel Events

Prevented FixedDataTable2 from hijacking scroll events from React elements rendering through portals.

The order of event bubbling on mouse wheel activation is for wheel events to fire first on the active element, followed by wheel events on background elements, followed by scroll events on the active element, followed by scroll events on background elements.

React's mock event system sends events to elements based on component hierarchy, as opposed to DOM structure. These can differ when using portals, such that children of FixedDataTable2 can render outside of FixedDataTable2's DOM. When not using React, events on these elements would not bubble to FixedDataTable2, as they are outside of the tabular DOM.

Previously, FixedDataTable2 used wheel event listeners to mimic scrolling behavior, which prevented event bubbling. When active elements had _scroll_ event listeners, FixedDataTable2 would terminate the event chain at the wheel listener, such that the foreground element never received its scroll event, despite being the active element.

With an activation prop (`stopReactWheelPropagation`), the wheel listener for FixedDataTable2 now checks that the active element is a _DOM child_ of the table before reacting to events.

## Touch Events on Scrollbar

Previously, the Table component as a whole used a touch event listener to allow scrolling. However, since the scrollbar was a child of the table, touch events on the scrollbar caused scrolling in the same direction as touch events on the table. This is counter intuitive. Touch events on the table itself imply dragging the table in a material fashion, pulling data in the direction of your cursor. Touch events on the scrollbar imply the opposite -- pulling the _scrollbar_ in the direction of your cursor, meaning the data should flow the other direction.

Example of fixed behavior due to this change:

![scrollbar example](https://user-images.githubusercontent.com/343837/43801363-8b3289a4-9a58-11e8-90ad-2691d2770dca.gif)
