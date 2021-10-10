# Resumo: Redux Toolkit

Este projeto foi criado com [Create React App](https://github.com/facebook/create-react-app), usando o [Redux](https://redux.js.org/) e [Redux Toolkit](https://redux-toolkit.js.org/) como modelos.

Documentação: [https://redux-toolkit.js.org](https://redux-toolkit.js.org/)

Quick Start: [https://redux-toolkit.js.org/tutorials/quick-start](https://redux-toolkit.js.org/tutorials/quick-start)

Exemplo com formulário: [exercise-forms-redux](https://github.com/WeltonThomasFerreira/exercise-forms-redux)

# Observação

Esse método de implementação usa Hooks, ou seja, não funcionam em componentes de classe, apenas componentes funcionais.

Se tiver dúvidas sobre o funcionamento de Hooks em React consulte a [documentação.](https://pt-br.reactjs.org/docs/hooks-intro.html)

# Instalação

## Usando create react app

```bash
npx create-react-app my-app --template redux
```

## Adicionando em um app

```bash
npm install @reduxjs/toolkit react-redux
```

# API's

- **configureStore()**
- **createReducer()**
- **createAction()**
- **createSlice()**
- createAsyncThunk
- createEntityAdapter
- createSelector

# Implementado redux

## Criando uma redux store

```jsx
// src/app/store.js

import { configureStore } from '@reduxjs/toolkit'

export default configureStore({
  reducer: {},
})
```
Isso já configura o ReduxDevTools.

## Provendo a redux store para o react

```jsx
// index.js

import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import store from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

## Criando reducers e actions com createSlice()

### Exemplo: Contador

```jsx
// features/counter/counterSlice.js

import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    },
  },
})

// As actions são criadas para da cada Reducer
export const { 
increment, 
decrement, 
incrementByAmount } = counterSlice.actions

// .reducer é a combinação dos três reducers
export default counterSlice.reducer
```

### Importando slice/reducers para a store

```jsx
// src/app/store.js

import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export default configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```

## Usando states e actions redux nos componentes react

```jsx
// features/counter/Counter.js

import React from 'react'
// Importamos os hooks para conectar o componente ao redux
import { useSelector, useDispatch } from 'react-redux'
// Importamos as actions para evoluirmos o estado na store
import { decrement, increment } from './counterSlice'

export function Counter() {
// useSelector() permite acessar o estado na store
  const { count } = useSelector((state) => state.counter)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
```

# Explicando a estrutura de pastas e arquivos

A organização usa a lógica de "feature folders". Exemplo:

- **/src**
    - **index.jsx**
    - **/app**
        - **store.js**
        - **rootReducer.js** (opcional)
        - **App.js**
        - **/common** (utiliátios e componentes genéricos)
    - **/features**
        - **/counter**
            - **Counter.jsx** (componente)
            - **counterSlice.js** (reducer, action)

---

A pasta 'pages' ainda pode ser usada no nível de preferência.

A pasta 'components', no exemplo, iria dentro de /counter e agruparia os componentes pertinente ao 'Counter.jsx'. Counter se comportaria como um sub app.
