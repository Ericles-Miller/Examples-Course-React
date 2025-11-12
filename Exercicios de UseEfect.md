# Exercícios de UseEfect

## 🧩 **Exercício 1 – Console quando o componente monta**

**Objetivo:** entender o `useEffect` com dependência vazia `[]`.

**Descrição:**

- Crie um componente que exiba “Hello React!”
- Use `useEffect` para mostrar um `console.log('Componente montou!')` apenas **uma vez**, quando o componente for renderizado.

**Dica:**

```tsx
useEffect(() => {
  console.log('Componente montou!');
}, []); // <- executa uma vez
	
```

## 🧩 **Exercício 2 – Monitorar mudanças em um estado**

**Objetivo:** reagir a mudanças em um valor específico.

**Descrição:**

- Crie um contador (`useState`) com botões + e –.
- Use `useEffect` para exibir no console sempre que o contador mudar.

```tsx
import React, { useEffect, useRef, useState } from 'react'

const App = () => {
  const [count , setCount] = useState(0)
  
  const handleButton = (option) => {
    if(option == '+') setCount(count + 1)
    else setCount(count -1)
  }

  useEffect(() => {
    console.log('Contador mudou:', count);
  }, [count]);
  
  return (
    <div>
      <button onClick={() => handleButton('-')}>-</button>
      <button onClick={() => handleButton('+')}>+</button>
    </div>
  )
}

export default App
```

## 🧩 **Exercício 3 – Atualizar o título da aba**

**Objetivo:** usar `useEffect` para interagir com o `document`.

**Descrição:**

- Mostre o contador na tela.
- Atualize o **título da aba** do navegador (`document.title`) com o valor do contador sempre que ele mudar.

💡 Exemplo:

Se o contador for `3`, o título deve ser “Você clicou 3 vezes”.

```tsx
import React, { useEffect, useRef, useState } from 'react'

const App = () => {
  const [count , setCount] = useState(0)

  const handleCount = () => {
      setCount(count +1)
  }

  useEffect(() => {
    document.title = `Você clicou ${count} ${count === 1 ? 'vez' : 'vezes'}`;

    console.log(count)
  }, [count]); 

  
  return (
    <div>
      <button onClick={() => handleCount()}>+</button>
    </div>
  )
}

export default App
```

## 🧩 **Exercício 4 – Simular busca de dados**

**Objetivo:** usar `useEffect` com `setTimeout`.

**Descrição:**

- Mostre “Carregando dados...” quando o componente montar.
- Depois de 2 segundos (`setTimeout`), mude o texto para “Dados carregados com sucesso!”

```tsx
import React, { useEffect, useState } from 'react';

const App = () => {
  const [message, setMessage] = useState('Carregando dados...');

  useEffect(() => {
    // executa apenas uma vez quando o componente monta
    const timer = setTimeout(() => {
      setMessage('Dados carregados com sucesso!');
    }, 2000);

    // limpa o timer se o componente desmontar antes de 2s
    return () => clearTimeout(timer);
  }, []);

  return (
    <div style={{ textAlign: 'center', marginTop: '40px' }}>
      <p>{message}</p>
    </div>
  );
};

export default App;

```

## 🧩 **Exercício 5 – useEffect com cleanup**

**Objetivo:** limpar efeitos quando o componente desmonta.

**Descrição:**

- Crie um componente que use `setInterval` para aumentar o contador a cada segundo.
- Use o **retorno da função dentro do `useEffect`** para limpar o intervalo quando o componente for desmontado.

```tsx
const Counter = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('🟢 Componente montou — iniciando contador');

    // cria um intervalo que incrementa o contador a cada 1 segundo
    const interval = setInterval(() => {
      setCount(prev => prev + 1);
      console.log('⏱️ Contador atualizando...');
    }, 1000);

    // função de limpeza: será executada ao desmontar o componente
    return () => {
      clearInterval(interval);
      console.log('🔴 Componente desmontou — intervalo limpo');
    };
  }, []); // roda apenas na montagem e desmontagem

  return <p>Contador: {count}</p>;
};

```

## 🧩 **Exercício 6 – Buscar dados de uma API**

**Objetivo:** fazer uma requisição com `fetch` dentro do `useEffect`.

**Descrição:**

- Quando o componente montar, busque dados de uma API (ex: `https://jsonplaceholder.typicode.com/users`)
- Mostre na tela a lista de nomes.
- Exiba “Carregando...” enquanto os dados não chegam.

```tsx
import React, { useEffect, useState } from 'react';

const App = () => {
  const [message, setMessage] = useState('')
  const [shouldEfect, setShouldEfect] = useState(false)
  const [users, setUsers] = useState([])
  
  useEffect(() => {
    if(!shouldEfect) return

    setMessage('Dados Sendo Carregados')
    
    fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
    .then(data => {
      setUsers(data) 
    })
    .catch(error => console.error('Erro:', error))
    .finally(() => setShouldFetch(false)); 

   setMessage('Dados Carregados')

  }, [shouldEfect])

  const handleMessage = () => {
    setMessage('Load Data')
    setShouldEfect(true)
  }

  return (
    <div>
      <p>{message}</p>
      <button onClick={handleMessage}>Find Data</button>
      {users.length > 0 && (
        <ul style={{ listStyle: 'none', padding: 0 }}>
          {users.map(user => (
            <li key={user.id}>
              <strong>{user.name}</strong> — {user.email}
            </li>
          ))}
        </ul>
      )}
    </div>
  )
};

export default App;

```

## 🧩 **Exercício 7 – Dependências múltiplas**

**Objetivo:** entender como o `useEffect` reage a mais de um estado.

**Descrição:**

- Crie dois estados: `name` e `age`.
- Use `useEffect` que roda sempre que **qualquer um dos dois** mudar, e exiba no console:

```tsx
import React, { useEffect, useState } from 'react';

const App = () => {
  const [name, setName] = useState('')
  const [age, setAge] = useState(0)

  useEffect(() => {
    console.log('Name or age changed')
  }, [age, name])

  

  return (
    <div style={{ textAlign: 'center', marginTop: '40px' }}>
      <h2>Dependências múltiplas</h2>

      <div>
        <input
          type="text"
          placeholder="Digite seu nome"
          value={name}
          onChange={(e) => setName(e.target.value)}
          style={{ marginRight: '10px' }}
        />

        <input
          type="number"
          placeholder="Digite sua idade"
          value={age}
          onChange={(e) => setAge(e.target.value)}
        />
      </div>

      <p>Nome: {name}</p>
      <p>Idade: {age}</p>
    </div>
  );
}

export default App;

```

## 🧩 **Exercício 8 – useEffect condicional**

**Objetivo:** controlar quando um efeito roda.

**Descrição:**

- Só execute o efeito (`console.log`) **se o nome tiver mais de 3 letras**.
- Caso contrário, não execute nada.

```tsx
import React, { useEffect, useState } from 'react';

const App = () => {
  const [name, setName] = useState('')

  useEffect(() => {
      if(name.length >=3)
      console.log('Name is tallest to 3 caracters')
  }, [name])

  return (
    <div style={{ textAlign: 'center', marginTop: '40px' }}>
      <h2>Dependências múltiplas</h2>

      <div>
        <input
          type="text"
          placeholder="Digite seu nome"
          value={name}
          onChange={(e) => setName(e.target.value)}
          style={{ marginRight: '10px' }}
        />
      </div>

      <p>Nome: {name}</p>
    </div>
  );
}

export default App;

```

## 🧩 **Exercício bônus – Tempo de login**

**Objetivo:** combinar `useEffect` + `setTimeout`.

**Descrição:**

- Quando o usuário clicar em “Login”, mostre “Entrando...”.
- Após 3 segundos, mostre “Bem-vindo à plataforma!”

```tsx
import React, { useEffect, useState } from 'react';

const App = () => {
  const [message, setMessage] = useState('')
  const [isLoggingIn, setIsLoggingIn] = useState(false);

  useEffect(() => {
    if (!isLoggingIn) return;

    const timer = setTimeout(() => {
      setMessage('Welcome to Platform');
      setIsLoggingIn(false); // encerra o estado de login
    }, 3000);

    return () => clearTimeout(timer);
  }, [isLoggingIn])

  const handleLogin = () => {
    setMessage('Entrando...');
    setIsLoggingIn(true); // ativa o efeito
  };

  
  return (
    <div style={{ textAlign: 'center', marginTop: '40px' }}>
      <h2>{message}</h2>

      <div>
          <button value={message}
          onClick={() => handleLogin()}
          >
            Login
          </button>
      </div>
    </div>
  );
}

export default App;

```