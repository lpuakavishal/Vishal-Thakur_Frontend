Live link:-https://stellar-narwhal-13d84b.netlify.app

Please check this pdf:-https://drive.google.com/file/d/1fwtPupu3xKZVxdqL-RRBIfvfmPUYYjyf/view


Qno-:1. Explain what the simple List component does.


Ans:- The List component is a React component that displays a list of items with a single-item selection
feature. It is composed of two functional sub-components: SingleListItem and ListComponent.
SingleListItem represents a single item in the list and accepts four props: index, isSelected,
onClickHandler, and text. Index represents the position of the item in the list, isSelected is a boolean
that indicates whether the item is currently selected, onClickHandler is a function that is called when the
item is clicked, and text represents the item's text content. When an item is clicked, the onClickHandler
function is called with the item's index as an argument.
ListComponent represents the entire list and receives one prop, which is an array of objects called items.
Each item object has a text property representing the text content of the item. ListComponent utilizes
the useState hook to maintain the selected item's index state and the useEffect hook to reset the
selected index when there is a change in the items prop.
ListComponent maps over the items prop and renders a SingleListItem component for each item,
passing in the appropriate props, including isSelected, based on the current selectedIndex state.
Consequently, the List component provides a simple and reusable solution for displaying a list of items
with single-item selection functionality in a React application.

Qno-:2.What problems / warnings are there with code?

Ans:- 
    As per my understanding I have encountered five errors in the code.
a. The onClick Events should use function reference but here its a function call. Function Call would
have been fine if there was arrow function.

    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>

b. Error in useState() hooks. -:
Basically the useState() hooks return a array which has two element , the first element is the
variable_name and the second element is setVariable_name (a function which set the value of
variable_name ).

     const [setSelectedIndex, selectedIndex] = useState();

c. null as a default props:- We should not use null as default props there must be some valid value
allocated to default props.

    WrappedListComponent.defaultProps = {
         items: null,
    };

d. Unique key to props -: It is recommended to provide unique key to props while using map function
on array,
unique key to each props help the virtual DOM to render only the required segment of code. It helps the
diffing algorithm to work in a optimized way and brings smoothness in reconciliation process.

    return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
      )

e. Syntex error in array and shapeOf.

    WrappedListComponent.propTypes = {
    items: PropTypes.array(PropTypes.shapeOf(
    {
    text: PropTypes.string.isRequired,
    } )),
    };


Qno3:- Please fix, optimize, and/or modify the component as much as you think is necessary.  (PLEASE CHECK App.js)

    import React, { useState, useEffect, memo } from "react";
    import PropTypes from "prop-types";

    // Single List Item
    const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
     const handleClick = () => {
         onClickHandler(index);
    };

    return (
     <li
        style={{ backgroundColor: isSelected ? "green" : "red" }}
        onClick={handleClick}
        >
      {text}
    </li>
    );
    };

    WrappedSingleListItem.propTypes = {
        index: PropTypes.number,
         isSelected: PropTypes.bool,
           onClickHandler: PropTypes.func.isRequired,
            text: PropTypes.string.isRequired
         };

    const SingleListItem = memo(WrappedSingleListItem);

    // List Component
    const WrappedListComponent = ({ items }) => {
     const [selectedIndex, setSelectedIndex] = useState();

    useEffect(() => {
     setSelectedIndex(null);
    }, [items]);

    const handleClick = (index) => {
     selectedIndex === index ? setSelectedIndex(null) : setSelectedIndex(index);
    };
     // console.log(selectedIndex);
     return (
     <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
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
      text: PropTypes.string.isRequired
    })
     )
    };

    WrappedListComponent.defaultProps = {
     items: [
     { text: "Task1" },
     { text: "Task2" },
     { text: "Task3" },
     { text: "Task4" }
    ]
    };

    const List = memo(WrappedListComponent);

    export default List;


