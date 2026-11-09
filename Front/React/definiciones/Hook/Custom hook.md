Los custom hooks son funciones utilitarias, para la encapsulación de lo lógica con el fin de que se peuda reusar el codigo y no repetirse asi mismo.

## Sin custom hook

```tsx

import React, { useState } from "react";
import "./style.css"

export default function App() {
  const [counter, setCounter] = useState(0);
  
  return (
  <div>
  <div>counter</div>
  <button>onClick={() => setCounter(counter + 1)} + </button>
  </div>
  )

}

```


## Usando el custom hook

```tsx

import React, { useState } from "react";
import "./style.css"

const useCounter = () => {
  const [counter, setCounter] = useState(0);
  
  const increase = () => setCounter(counter + 1);
  const decrement = () => setCounter(counter - 1);
  const reset = () => setCounter(0);
  
  return {
    counter,
    increase,
    decrement,
    reset
  };
};

export default function App() {
  const {counter, increase, decrement, reset } = useCounter();
  return (
  <div>
  <div>counter</div>
  <button>onClick={increase} + </button>
  <button>onClick={decrement} - </button>
  <button>onClick={reset} zero </button>    
  </div>
  )
}

```
