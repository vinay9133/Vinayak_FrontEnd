A simple user interface element called Simple List shows a group of connected objects in a vertical list format. By clicking or tapping on the list items, users can do various actions or travel to different screens. The Simple List component is a practical and adaptable UI element that offers a simple approach to present a group of related things to users in an understandable and structured fashion.
1. The "onClick" handler in the "WrappedSingleListItem" component is set to "onClick=onClickHandler(index)". Instead of registering it as an event handler, this will immediately call the 'onClickHandler' method with the 'index' attribute when the component is rendered. You can update it to "onClick=() => onClickHandler(index)" to correct this.                                                                                                                                                         2.  The 'useState' hook is wrongly called in the 'WrappedListComponent' component. The setter function is supplied as an input to 'useState' rather than the original state value. Change that to "const [selectedIndex, setSelectedIndex] = useState(null);" to fix the problem.                                                           3. The 'items' prop in the 'WrappedListComponent' component is set to 'null' by default, but the 'propTypes' validator anticipates that it should be an array of objects. You may correct this by setting the default value to an empty array using the following syntax: "WrappedListComponent.defaultProps = items: [] ;".              4. The 'propTypes' validator for the 'items' prop is incorrectly defined in the 'WrappedListComponent' component. You should call the array and shapeOf validators as functions, as in "PropTypes.arrayOf(PropTypes.shape(... ))". Change it to "WrappedListComponent.propTypes = items: PropTypes.arrayOf(PropTypes.shape( text: PropTypes.string.isRequired ))" to address the issue.
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
    index,
    isSelected,
    onClickHandler,
    text
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'purple' : 'blue' }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ 
    items
}) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
};

WrappedListComponent.defaultProps = {
  items: [
    {
        text: "SteelEye"
    },
    {
        text: "Hey world!"
    },
    {
        text:"Vinayak"
    },
    {
      text: "welcome to steeleye"
    },
  ]
};

const List = memo(WrappedListComponent);

export default List;
