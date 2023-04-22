# Answers for the queries

1. Explain what the simple `List` component does.

ANS : The `List` component is displays a list of items. Each item is displayed in a separate `li` element. The `SingleListItem` component represents a single item and its properties are passed down as props to the component. The List component maps over the array of items passed as props and renders a `SingleListItem` for each item in the array and `onclick` of the items background color of that particular items turn `green`;

2. What problems / warnings are there with code?

ANS:
a. )The "setSelectedIndex" default value in [usestate] hook is not initialized with an initial value, which may cause issues. It should be initialized with a     default value or a value that is passed as a prop.

b.) The "isSelected" prop in the "SingleListItem" component is expected to be a boolean, but it is being passed the "selectedIndex" value [null] which is a number. This may be a problem.

3. Please fix, optimize, and/or modify the component as much as you think is necessary.

## Code

```javascript
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [],
};

const List = memo(WrappedListComponent);

export default List;
```
