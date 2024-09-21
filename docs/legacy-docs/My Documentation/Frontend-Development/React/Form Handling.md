>[!SUMMARY]- Table of Contents
>
>- [[Form Handling#Handle Change Code|Handle Change Code]]
# Handle Change Code

- checkbox need to be treated differently
- rest types can be handles just by creating a formdata useState in react

```javascript
    const [formData, setFormData] = useState({
        year: "",
        batches: [],
        questions: [],
    });

    const handleChange = (e) => {
        const { name, value, type, checked } = e.target;
        console.log(name, value, type, checked);
        if (type === "checkbox") {
            if (checked) {
                setFormData((prevData) => ({
                    ...prevData,
                    batches: [...prevData.batches, name],
                }));
            } else {
                setFormData((prevData) => ({
                    ...prevData,
                    batches: prevData.batches.filter((batchName) => batchName !== name),
                }));
            }
        } else {
            // Handle other input types
            setFormData((prevData) => ({
                ...prevData,
                [name]: value,
            }));
        }
    };
```
