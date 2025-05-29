## ¿Qué es Redux Toolkit?

Redux Toolkit es la forma moderna de escribir lógica Redux. Simplifica la configuración y reduce el boilerplate que tradicionalmente requería Redux. Soluciona el problema del prop drilling.

## Conceptos Fundamentales

### **Slice**
Un slice es una función que encapsula la lógica de Redux de una característica específica de la aplicación.

**Parámetros requeridos:**
- **name**: String que identifica al slice (ej: "counter", "users")
- **initialState**: Estado inicial de esa porción del store
- **reducers**: Objeto con funciones que definen cómo actualizar el estado

> **Importante**: Los slices generan automáticamente las actions correspondientes a cada reducer.

### **Immer**
Biblioteca integrada que permite escribir código que "parece" mutar el estado directamente, pero mantiene la inmutabilidad internamente.

```javascript
// Con Immer puedes escribir:
state.count += 1

// En lugar de:
return { ...state, count: state.count + 1 }
```

### **Payload**
Información adicional que una action puede llevar. No todas las actions necesitan payload.

```javascript
// Action con payload
dispatch(addUser({ name: "Luis", email: "luis@email.com" })) 

// Action sin payload
dispatch(clearUsers())
```

## Funciones Principales

### **configureStore()**
Crea el store de Redux con configuración optimizada, incluyendo middleware para desarrollo y debugging.

### **createSlice()**
Función principal para crear slices. Combina actions y reducers en una sola definición.

### **createAsyncThunk()**
Maneja operaciones asíncronas (APIs) generando automáticamente actions para: pending, fulfilled y rejected.

## Hooks de Redux

### **useSelector()**
Lee datos del store:
```javascript
const count = useSelector(state => state.counter.value)
```

### **useDispatch()**
Dispara actions:
```javascript
const dispatch = useDispatch()
dispatch(increment())
```

## Configuración Básica

### **Provider**
Envuelve la aplicación para dar acceso al store:
```javascript
<Provider store={store}>
  <App />
</Provider>
```

### **Estructura de Carpetas Típica**
```
src/
├── store/
│   ├── store.js (configureStore)
│   └── slices/
│       ├── counterSlice.js
│       └── userSlice.js
```

## Flujo de Datos Unidireccional

1. **Componentes** leen el estado con `useSelector`
2. **Componentes** disparan actions con `useDispatch`
3. **Actions** llegan a los reducers
4. **Reducers** actualizan el estado
5. **Componentes** se re-renderizan con el nuevo estado

## Herramientas de Desarrollo

### **Redux DevTools**
Extensión para debugging que permite:
- Ver todas las actions
- Inspeccionar el estado
- "Viajar en el tiempo" entre estados
- Reproducir bugs

## Ejemplo Básico

```javascript
// counterSlice.js
import { createSlice } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions
export default counterSlice.reducer
```

```javascript
// store/index.js
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from './slices/counterSlice'

export const store = configureStore({
  reducer: {
    counter: counterReducer
  }
})
```

```javascript
// Component.jsx
import { useSelector, useDispatch } from 'react-redux'
import { increment, decrement } from './store/slices/counterSlice'

function Counter() {
  const count = useSelector(state => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>

      <span>{count}</span>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  )
}
```

## Puntos Clave

- Redux Toolkit simplifica Redux
- Immer maneja la inmutabilidad
- Los slices generan actions
- No todas las aplicaciones necesitan Redux
- DevTools para debugging
- El flujo de datos es unidireccional