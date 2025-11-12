## Exercicios de UseState

**Objetivo:** alternar entre mostrar e esconder um conteúdo.

**Descrição:**

Crie um componente `ToggleText` que:

- Mostra um botão “Mostrar / Ocultar texto”
- Quando clicado, alterna entre exibir ou esconder um `<p>` com uma frase.

**Dica:** use `useState` para armazenar `true` ou `false`.

```tsx
import React, { useRef, useState } from 'react'
import Confetti from 'js-confetti'
import './style.css'

const App = () => {
  const [isVisible, setIsVisible] = useState(false);

  const handleClick = () => {
    setIsVisible(!isVisible)
  }
  
  return (
     <div>
       <p style={{ display: isVisible ? "block" : "none" }}>Show test now</p>
       
       <button onClick={handleClick}>
        {isVisible ? "Ocultar texto" : "Mostrar texto"}
      </button>
     </div>
    
  )
}

export default App
```

## 🧩 **Exercício 3 – Formulário controlado**

**Objetivo:** capturar o valor de um input em tempo real.

**Descrição:**

Crie um componente `InputName` que:

- Tem um `<input>` de texto
- Mostra abaixo “Olá, [nome digitado]”
- Atualiza o texto conforme o usuário digita.

```tsx
import React, { useRef, useState } from 'react'

const App = () => {
  const [name, setName] = useState('');
  
  const handleKey = (event) => {
    if(event.key != 'Enter' && event.key != 'Backspace')
      setName(name + event.key)
  }
  
  return (
    <div>
      <p>Helo {name}</p>

      <label for="meuInput">Nome:</label>
      <input type="text" id="meuInput" name="nomeUsuario" onKeyDown={handleKey}></input>
    </div>
  )
}

export default App
```

## 🧩 **Exercício 4 – Lista dinâmica**

**Objetivo:** adicionar e remover itens de uma lista.

**Descrição:**

Crie um componente `TodoList` que:

- Tem um input e um botão “Adicionar”
- Ao clicar, o item é adicionado a uma lista (array)
- Cada item tem um botão “Remover”

**Dica:** `useState` para o array de tarefas.

```tsx
import React, { useRef, useState } from 'react'

const App = () => {
  const [toDo, setToDo] = useState([])
  const [count, setCount] = useState(0)
  const [text, setText] = useState('')
  
  const handleInsertToDO = (task) => {
    setCount(count + 1)
   
     setToDo([...toDo, {key: count, value: text}])
    console.log(toDo)
    setText('')
  }

  const handleRemoveToDo = (index) => {
    setToDo(toDo.filter((_, i) => i !== index));
  }

  
  return (
    <div>
    <div>
      <input 
        type="text"
        placeholder="insert item in list to do"
        value={text}
        onChange={(e) => setText(e.target.value)}
      />
    
      <button onClick={handleInsertToDO}>+</button>
    </div>

      <ul>
        <div>
        {
          toDo.map((item, index) => (
            <div style={{display: 'flex'}}>
              <li key={index}>{item.key} -- {item.value}</li> 
              <button onClick={() => handleRemoveToDo(index)}>X</button>
            </div>
          ))
        }
        </div>
      </ul>
    </div>
  )
}

export default App
```

## **Exercício 5 – Mudar cor de fundo**

**Objetivo:** usar `useState` para alterar estilos dinamicamente.

**Descrição:**

Crie um componente `ColorBox` que:

- Mostra uma `<div>` colorida (ex: azul)
- Tem 3 botões: “Vermelho”, “Verde”, “Azul”
- Cada clique muda a cor de fundo da div.

```tsx
import React, { useRef, useState } from 'react'

const App = () => {
  const [color, setColor] = useState('blue')

  const handleColor = (color) => {
    if(color == 'green') setColor('green')
    else if(color == 'yellow') setColor('yellow')
    else setColor('blue')

    console.log(color)
  }

  
  return (
    <div style={{
      width: '16rem',
      height: '16rem',
      background: color == 'green' ? 'green' : color == 'yellow' ? 'yellow' : 'blue'
    }}>
       <button onClick={() => handleColor('green')}>Verde</button>
       <button onClick={() => handleColor('blue')}>Azul</button>
       <button onClick={() => handleColor('yellow')}>Amarelo</button>
    </div>
  )
}

export default App
```

## 🧩 **Exercício 7 – Simulador de login**

**Objetivo:** gerenciar múltiplos estados.

**Descrição:**

Crie um componente `LoginSimulator` que:

- Mostra um input de nome de usuário
- Um botão “Entrar”
- Quando clicado, muda o estado para “logado” e mostra “Bem-vindo, [nome]”
- Um botão “Sair” volta ao estado inicial.

```tsx
import React, { useRef, useState } from 'react'

const App = () => {
  const [buttonLogin, setButtonLogin] = useState('Login')
  const [message, setMessage] = useState('Sign in Platform')
  const [name, setName] = useState('')
  
  const handleLogin = (option, name) => {
    console.log(option, name)
    if(option == 'Login') {
      console.log('enrou')
      setButtonLogin('Logout')
      setMessage(`Welcome to platform ${name}`)
      
    } else {
      setButtonLogin('login')
      setMessage('Sign in Platform')
    }
  }
  
  return (
    <div>
      <p>{message}</p>  
      <div>
        <input 
          type='text'
          placeholder='Insert Your Name'
          value={name} 
          onChange={(e) => setName(e.target.value)}
        />
         <button onClick={() => handleLogin(buttonLogin, name)}>{buttonLogin}</button>
      </div>
    </div>
  )
}

export default App
```