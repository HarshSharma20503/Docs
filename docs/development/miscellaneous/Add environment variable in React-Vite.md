# Add Env variable in react

## React

Write every environment variable with prefix REACT_APP_....
e.g.

```bash
REACT_APP_URL = http://localhost:8080/
```

In the jsx file use the variable using

```bash
process.env.REACT_APP_URL
```

---

## Vite

Write every environment variable using prefix VITE_...

```bash
VITE_URL = http://localhost:8080/
```

In the jsx file use the variable using

```bash
import.meta.env.VITE_URL
```

---

## Reference

- [youtube](https://www.youtube.com/shorts/r92aHr752Bg)
