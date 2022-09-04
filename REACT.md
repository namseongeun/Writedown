# REACT

## 1. 버튼을 클릭하면 숫자가 늘어나는 거

```react
<body>
  <div id="root"></div>
</body>

<script type="text/babel">
  function App () {
      const [counter, setCounter] = React.useState(0)
      const onClick = () => {
        // setCounter(counter + 1)
        // 밑의 방법이 좀더 안전함(현재값으로 다음결과를 내는 것이 확실하게 보장되기 때문!)
        setCounter((current) => current + 1)
      }
    return (
      <div>
        <h3>Total clicks: {counter}</h3>
        <button onClick={onClick}>Click me</button>
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
```



## 2. 단위 변환기

```react
<body>
  <div id="root"></div>
</body>

<script type="text/babel">
  function App() {
    const [amount, setAmount] = React.useState()
    const [flipped, setFlipped] = React.useState(false)
    const onChange = (event) => {
      setAmount(event.target.value)
    }
    const reset = () => setAmount(0)
    const onFlip = () =>  {
      reset()
      setFlipped((current) => !current)
    }
    return (
      <div>
        // class가 자바스크립트에서 예약어이므로 className으로 변경해야함
        <h1 className="hi">SuperConverter</h1>

        // 분을 시간으로 변환
        <div>
          // class와 마찬가지로 for 역시 htmlFor로 변경해야함
          <label htmlFor="minutes">Minutes</label>
          <input 
            // 삼항연산자: 변환된 상태라면 amount가 시간인 상태이므로 분으로 변환
            value={flipped? amount*60 : amount}
            id="minutes" 
            placeholder="Minutes" 
            type="number"
            onChange={onChange}
            // 만약 변환되었다면 사용불가
            disabled={flipped === true}
            // disabled={flipped}
          />
          <h4>You want to convert {minutes}</h4>
        </div>

        // 시간을 분으로 변환
        <div>
          <label htmlFor="hours">Hours</label>
          <input 
            // 삼항연산자: 변환되지 않은 상태라면 amount가 분인 상태이므로 시간으로 변환
            value={flipped ? amount : Math.round(amount / 60)}
            id="hours" 
            placeholder="Hours" 
            type="number"
            onChange={onChange}
            // 만약 변환되지 않았다면 사용불가
            disabled={flipped === false}
            // disabled={!flipped}          
          />
        </div>

        // 리셋버튼과 클립버튼
        <button onClick={reset}>Reset</button>        
        <button onClick={onFlip}>Flip</button>        
      </div>
    )
  }
  
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)  
</script>
```



## 3. 두 가지 단위 변환기 전환

```react
<body>
  <div id="root"></div>
</body>

<script type="text/babel">
    function MinutesToHours() {
    const [amount, setAmount] = React.useState()
    const [flipped, setFlipped] = React.useState(false)
    const onChange = (event) => {
      setAmount(event.target.value)
    }
    const reset = () => setAmount(0)
    const onFlip = () =>  {
      reset()
      setFlipped((current) => !current)
    }
    return (
      <div>
        // 분을 시간으로 변환
        <div>
          // class와 마찬가지로 for 역시 htmlFor로 변경해야함
          <label htmlFor="minutes">Minutes</label>
          <input 
            // 삼항연산자: 변환된 상태라면 amount가 시간인 상태이므로 분으로 변환
            value={flipped? amount*60 : amount}
            id="minutes" 
            placeholder="Minutes" 
            type="number"
            onChange={onChange}
            // 만약 변환되었다면 사용불가
            disabled={flipped === true}
            // disabled={flipped}
          />
          <h4>You want to convert {minutes}</h4>
        </div>

        // 시간을 분으로 변환
        <div>
          <label htmlFor="hours">Hours</label>
          <input 
            // 삼항연산자: 변환되지 않은 상태라면 amount가 분인 상태이므로 시간으로 변환
            value={flipped ? amount : Math.round(amount / 60)}
            id="hours" 
            placeholder="Hours" 
            type="number"
            onChange={onChange}
            // 만약 변환되지 않았다면 사용불가
            disabled={flipped === false}
            // disabled={!flipped}          
          />
        </div>

        // 리셋버튼과 클립버튼
        <button onClick={reset}>Reset</button>        
        <button onClick={onFlip}>Flip</button>        
      </div>
    )
  }
    function KmToMiles() {
      return <h3>KM to M</h3>
    }
    function App() {
      const [index, setIndex] = React.useState("xx")
      const onSelect = (event) => {
        setIndex(event.target.value)
      }
      retrurn (
        <div>
          <h1>Super Converter</h1>
          <select value={index} onChange={onSelect}>
            <option value="0">Minutes & Hours</option>
            <option value="1">Km & Miles</option>
          </select>
          <hr />
          {index === "xx" ? 'Please select your units' : null}
          {index === "0" ? <MinutesToHours /> : null}
          {index === "1" ? <KmToMiles /> : null}        
        </div>
      )
    }

  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)  
</script>
```



## 4. Props - Btn 속성 상속하기

```react
<html>
  <body>
    <div id="root"></div>
  </body>

  <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    function Btn({ text, big }) {
      return (
        <button
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px",
            border: 0,
            borderRadius: 10,
            fontSize: big ? 18 : 16,
          }}>
          {text}
        </button>
      )
    }

    function App() {
      return (
        <div>
          <Btn text="Save Changes" />
          <Btn text="Continue" />
        </div>
      )
    }
    const root = document.getElementById("root")
    ReactDOM.render(<App />, root)
  </script>
</html>
```



## 5. Props - Btn 상속 이벤트 리스너 등록

```react
<html>
  <body>
    <div id="root"></div>
  </body>

  <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    function Btn({ text, onClick }) {
      return (
        <button
          onClick={onClick}
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px",
            border: 0,
            borderRadius: 10,
          }}>
          {text}
        </button>
      )
    }

    // 부모를 상속 받는 자식 요소들이 전부 리랜더링 되지 않도록 해주는 리액트의 메모 기능
    const MemorizedBtn = React.memo(Btn)
    function App() {
      const [value, setValue] = React.usestate("Save Changes")
      const changeValue = () => setValue("Revert Changes")
      return (
        <div>
          // 여기에 prop을 넣는다고 등록되는 것이 아니라 위의 function과 return에서 직접 등록해줘야함
          <MemorizedBtn text={value} onClick={changeValue} />
          <MemorizedBtn text="Continue" />
        </div>
      )
    }
    const root = document.getElementById("root")
    ReactDOM.render(<App />, root)
  </script>
</html>
```



## 6. Props - 데이터 타입 정하기

```react
<html>
  <body>
    <div id="root"></div>
  </body>

  <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    // 기본값 정하는 것도 가능: fontSize = 16
    function Btn({ text, fontSize = 16 }) {
      return (
        <button
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px",
            border: 0,
            borderRadius: 10,
            fontSize,
          }}>
          {text}
        </button>
      )
    }

    // 값의 형식 정하기
    // Typechecking With PropTypes
    Btn.propTypes = {
      // text는 string 타입의 값만 받는다.
      // 필수로 요구되는 사항: isRequired
      text: PropTypes.string.isRequired,
      // fontSize는 number 타입의 값만 받는다.
      fontSize: propTypes.number,
    }
    function App() {
      return (
        <div>
          <Btn text="Save Changes" fontSize={18} />
          <Btn text="Continue" fontSize={14} />
        </div>
      )
    }
    const root = document.getElementById("root")
    ReactDOM.render(<App />, root)
  </script>
</html>
```



## 7. Create React App

### Quick Overview

```bash
npx create-react-app [내가 만드려는 애플리케이션 이름]
npm start
```

```react
// index.js

import React from "react"
import ReactDOM from "react-dom"
import APP from "./App"

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
  document.getElementById("root")
)
```

```react
// App.js
import Button from "./Button" 

function App() {
  return (
    <div>
      <h1>Welcome back!!!</h1>
      <Button text={"Continue"}/>
    </div>
  )
}

export default App
```

```react
// Button.js

// npm i prop-types
import PropTypes from "prop-types"
improt styles from "./Button.module.css"

function Button({ text }) {
  return <button className={styles.btn}>{text}</button>
}
          
Button.propTypes = {
  text: PropTypes.string.isRequired,
}         
         
export default Button
```

```css
/* Button.module.css */
/* CSS작업도 모듈화 하는 것이 가능!! */

.btn {
  color: white;
  background-color: tomato;
}
```





## 8. Effects

```react
// App.js
import { useState, useEffect } from "react"

function App() {
  const [counter, setValue] = useState(0)
  const [keyword, setKeyword] = useState("")
  const onCilck = () => setValue((prev) => prev + 1)
  const onChange = (event) => setKeyword(event.target.value)

  // 랜더링 될 때마다 실행 
  console.log("I run all the time")
    
  // 단 한 번만 실행되는 코드
  useEffect(() => {
    console.log("only one")
  }, [])
  
  // keyword가 변할 때에만 실행되는 코드
  useEffect(() => {
   	// 조건을 거는 것도 가능하다.
    if (keyword !== "" && keyword.length > 5) {
      console.log("SEARCH FOR", keyword)
    }
    console.log("SEARCH FOR", keyword)
  }, [keyword])

  // counter가 변할 때에만 실행되는 코드
  useEffect(() => {
    console.log("when counter changed")
  }, [counter])
  
  // keyword와 counter가 모두 변할 때에만 실행되는 코드
  useEffect(() => {
    console.log("when keyword & counter changed")
  }, [counter, keyword])
    
  return (
    <div>
      <input value={keyword} onChange={onChange} type="text" placeholder="Search here...">
      <h1>{counter}</h1>
      <button onClick={onClick}>click me</button>
    </div>
  )
}

export default App
```



## 9. Cleanup Function

```react
// App.js
import { useState, useEffect } from "react"

function Hello() {
  function byFn() {
    console.log("bye :(")
  }
  function hiFn() {
    console.log("created :)")
    return byFn
  }
  useEffect(hiFn, [])
  return <h1>Hello</h1>
}
//function Hello() {
//  useEffect(() => {
//    console.log("hi :)")
//    return () => console.log("bye :(")
//  }, [])
// return <h1>Hello</h1>
//}

function App() {
  const [showing, setShowing] = useState(false)
  const onClick = () => setShowing((prev) => !prev)
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Showw"}</button>
    </div>          
  )

export default App
