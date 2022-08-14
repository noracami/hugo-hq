---
date: 2222-07-30T20:06:44+08:00
---

```javascript
const API = "whatever";
const FetchAPIExample = () => {
  const [data, setData] = React.useState({});

  React.useEffect(() => {
    fetch(API, {
      method: "POST",
      headers: headers,
      body: JSON.stringify({
        "queryString":
      })
    })
    .then((response) => response.json())
    .then((response) => {
      setData(response);
    });
  }, [name]);
};

const FormInput = () => {
  const [name, setName] = React.useState("");
  const atChange = (e) => {
    setName(e.target.value);
  };

  return (
    <input
      type="text"
      className="my-input"
      value={name}
      onChange={atChange}
    />
  );
};
```
