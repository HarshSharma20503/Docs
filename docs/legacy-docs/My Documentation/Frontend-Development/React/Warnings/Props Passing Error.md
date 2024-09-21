>[!SUMMARY]- Table of Contents
>        - [[Props Passing Error#Code Showing Warning|Code Showing Warning]]
>        - [[Props Passing Error#Code Not Showing Error|Code Not Showing Error]]

Invalid value for prop `changeindex` on <div></div> tag. Either remove it from the element, or pass a string or number value to keep it in the DOM

# Code Showing Warning

```javascript
function OffCanvasExample(props){
return (
...
<Offcanvas.Body>
	{props.questions.map((question, index) => (
		<div key={index} onClick={(event) => props.changeindex(index)} style={{ cursor: "pointer" }}>
			...
		</div>
	))}
</Offcanvas.Body>
)
}
```

# Code Not Showing Error

```javascript
function OffCanvasExample({changeindex, ...props}){
return (
...
<Offcanvas.Body>
	{props.questions.map((question, index) => (
		<div key={index} onClick={(event) => changeindex(index)} style={{ cursor: "pointer" }}>
			...
		</div>
	))}
</Offcanvas.Body>
)
}
```

just destructured the changeindex function while receiving the props