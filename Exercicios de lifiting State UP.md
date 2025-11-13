## Exercícios Com Lifting State Up

### 💪 **Exercício 1 – Comunicação básica**

**Objetivo:** entender a passagem de estado e callbacks.

**Descrição:**

- Crie dois componentes filhos: `<InputName />` e `<DisplayName />`.
- O componente pai (`App`) deve conter o estado `name`.
- `<InputName />` tem um input e, ao digitar, chama uma função recebida via props para atualizar o `name`.
- `<DisplayName />` exibe o nome atualizado.

**Dica:**

Use `props.onChangeName` e `props.name`.

```tsx
// appjs 
import React, {useState} from 'react';
import InputName from './InputName'
import DisplayName from './DisplayName'

export default function App() {
  const [name , setName] = useState('')
  
  return (
    <div className='App'>
      <InputName name={name} setName={setName} />
      <DisplayName displayName={name}/>
    </div>
  );
}

// InputName
export interface INameProps {
  name: string;
  setName: React.Dispatch<React.SetStateAction<string>>;
}

export default function InputName({name, setName}: INameProps) {
  return <div>
    <input 
      type='text'
      value={name}
      onChange={(e) => setName(e.target.value)}
      placeholder="Digite algo" 
    />
  </div>
}

//displayName
export interface IDysplayNameProps {
  displayName: string;
}

export default function DisplayName({displayName}: IDysplayNameProps) {
  
  return <div>
    <p> Bem Vindo: {displayName}
    </p>
  </div>
}

```

### ⚙️ **Exercício 2 – Comunicação entre irmãos**

**Objetivo:** um componente atualiza e outro reage.

**Descrição:**

- Crie dois componentes irmãos: `<CounterControl />` e `<CounterDisplay />`.
- O estado `count` fica no pai.
- `<CounterControl />` tem botões `+` e  que atualizam `count`.
- `<CounterDisplay />` apenas mostra o valor atual do contador.

```tsx
//App.tsx
export default function App() {
  const [number , setNumber] = useState(0)
  
  return (
    <div className='App'>
      <InputName number={number} setNumber={setNumber} />
      <CounterDisplay displayCounter={number}/>
    </div>
  );
}

//InputName
export interface INumberProps {
  number: number;
  setNumber: React.Dispatch<React.SetStateAction<number>>;
}

export default function InputName({number, setNumber}: INumberProps) {
  return <div>
    <button value={number} onClick={() => setNumber(number + 1)}>+</button>
    <button onClick={() => setNumber(number - 1)}>-</button>
  </div>
}

// CounterDisplay
	export interface ICounterDysplayProps {
	  displayCounter: number;
	}
	
	export default function CounterDisplay({displayCounter}: ICounterDysplayProps) {
	  
	  return <div>
	    <p> Contagem: {displayCounter}
	    </p>
	  </div>
	}
```

### 🧩 **Exercício 3 – Controle de formulário compartilhado**

**Descrição:**

- Crie dois componentes: `<FormInput />` e `<FormSummary />`.
- No pai, guarde `name`, `email` e `age`.
- `<FormInput />` atualiza os três estados via callbacks recebidos.
- `<FormSummary />` exibe um resumo atualizado em tempo real.

```tsx
import React, {useState} from 'react';
import FormInput from './DisplayName'
import { IInputForm } from './DisplayName'
import FormSummary from './InputName'

export default function App() {
  const [iInputForm, setIInputForm] = useState<IInputForm>({
    name: '',
    email: '',
    age: 0
  });

  return (
    <div className='App'>
      <h2>Formulário</h2>
      <FormInput iInputForm={iInputForm} setIInputForm={setIInputForm} />
      <FormSummary iInputForm={iInputForm} />
    </div>
  );
}

import { IInputForm } from './DisplayName'

export default function FormSummary({ iInputForm }: { iInputForm: IInputForm }) {
  return <div>
    <p>{iInputForm.name}</p>
    <p>{iInputForm.age}</p>
    <p>{iInputForm.email}</p>
  </div>
} 

export interface IInputForm {
  age: number;
  name: string;
  email: string;

}

export interface IInputFormProps {
  iInputForm : IInputForm;
  setIInputForm: React.Dispatch<React.SetStateAction<IInputForm>>
}

export default function FormInput({iInputForm, setIInputForm}: IInputFormProps) {
  
  return <div>
    <input
     type="text"
     value={iInputForm.name}
     placeholder='set your name'
     onChange={(e) => setIInputForm({...iInputForm, name: e.target.value})}
    />
    <input 
      type='email'
      placeholder='set your email'
      value={iInputForm.email}
      onChange={(e) => setIInputForm({...iInputForm, email: e.target.value})} 
    />
    <input
      type="number"
      value={iInputForm.age}
      placeholder="Set your age"
      onChange={(e) => setIInputForm({...iInputForm, age: Number(e.target.value)})}
    />
  </div>
}
```

### ⚙️ **Exercício 4 – Logando alterações do estado compartilhado**

**Descrição:**

- Use o mesmo exercício 2 (contador).
- No componente pai, adicione um `useEffect` que:
    - Rode toda vez que `count` mudar.
    - Faça um `console.log("O contador foi alterado:", count)`.

👉 **Aprendizado:** o `useEffect` no pai pode reagir a mudanças feitas em componentes filhos.

```tsx
import {useState, useEffect} from 'react'
import InputName from './DisplayName'
import CounterDisplay from './InputName'

export default function App() {
  const [number , setNumber] = useState(0)
  
  useEffect(() => {
    console.log('Contador mudou:', number);
  },[number])

  return (
    <div className='App'>
      <InputName number={number} setNumber={setNumber} />
      <CounterDisplay displayCounter={number}/>
    </div>
  );
}

export interface ICounterDysplayProps {
  displayCounter: number;
}

export default function CounterDisplay({displayCounter}: ICounterDysplayProps) {
  
  return <div>
    <p> Contagem: {displayCounter}
    </p>
  </div>
}

export interface INumberProps {
  number: number;
  setNumber: React.Dispatch<React.SetStateAction<number>>;
}

export default function InputName({number, setNumber}: INumberProps) {
  return <div>
    <button value={number} onClick={() => setNumber(number + 1)}>+</button>
    <button onClick={() => setNumber(number - 1)}>-</button>
  </div>
}
```

### ⚙️ **Exercício 5 – Requisição baseada em estado elevado**

**Descrição:**

- No pai, tenha um estado `userId`.
- Crie dois filhos:
    - `<UserSelector />` → escolhe o ID (1, 2 ou 3)
    - `<UserDetails />` → mostra os dados do usuário
- Use `useEffect` no **pai** para buscar `https://jsonplaceholder.typicode.com/users/${userId}` sempre que o ID mudar.
- Passe os dados retornados via props para `<UserDetails />`.

👉 Esse exercício combina:

- **Lifting** (estado `userId` e dados do usuário no pai)
- **useEffect** (reagir a mudança e buscar dados)

```tsx
import {useState, useEffect} from 'react'
import UserSelector from './DisplayName'
import UserData from './InputName';

export default function App() {
  const [userId, setUserId] = useState(0);
  const [data, setData] = useState({});
  
  useEffect(() => {
    fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
      .then(response => response.json())
      .then(data => setData(data))
      .catch(error => console.error('Erro:', error));
  },[userId])

  return (
    <div className='App'>
      <UserSelector userId={userId} setUserId={setUserId} />
      <UserData data={data} />
    </div>
  );
}

// 
export interface IUserIdProps {
  userId: number;
  setUserId: React.Dispatch<React.SetStateAction<number>>;
}

export default function UserSelector({userId, setUserId}: IUserIdProps) {
  return <div>
    <input type='number' value={userId} onChange={(e) => setUserId(e.target.value)} />
  </div>
}

export default function UserData({ data }) {
  return (
    <div>
      <h3>Dados do usuário</h3>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  )
}
```