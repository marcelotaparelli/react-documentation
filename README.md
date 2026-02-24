# React + TypeScript + Vite

---

## Setup do Ambiente

O **Vite** substituiu o antigo *Create React App*. Ele possui **HMR (Hot Module Replacement)** nativo, então você **não precisa de nodemon**. O Vite recarrega apenas o módulo alterado instantaneamente ao salvar.

```bash
# Criar projeto com template TypeScript
npm create vite@latest meu-app -- --template react-ts

# Instalar dependências e rodar
cd meu-app
npm install
npm run dev

```

---

## Componentes e Props (TS)

Componentes são funções. Use `interfaces` para tipar as propriedades.

```tsx
interface CardProps {
  title: string;
  description?: string; // Opcional
  children: React.ReactNode; // Para passar componentes dentro de componentes
}

export function Card({ title, description, children }: CardProps) {
  return (
    <div className="card">
      <h1>{title}</h1>
      {description && <p>{description}</p>}
      {children}
    </div>
  );
}

```

---

## Estilização e Fontes

### Importando Fontes

* **Google Fonts:** Cole o `<link>` no `index.html`.
* **Local:** Coloque na pasta `assets` e importe no `App.css`:
`@font-face { font-family: 'MinhaFonte'; src: url('./assets/font.ttf'); }`

### Styled Components

`npm install styled-components` e `npm install -D @types/styled-components`

```tsx
import styled from 'styled-components';

const Button = styled.button<{ primary?: boolean }>`
  background: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  padding: 10px 20px;
  border: none;
  font-family: 'Inter', sans-serif;
`;

// Uso: <Button primary>Clique aqui</Button>

```

---

## Roteamento (React Router v6)

`npm install react-router-dom`

### Configuração e Rotas Privadas

As rotas privadas verificam uma condição (ex: `isAuth`) antes de renderizar o conteúdo.

```tsx
import { BrowserRouter, Routes, Route, Navigate, Link } from 'react-router-dom';

// Componente de Rota Privada
const PrivateRoute = ({ children }: { children: JSX.Element }) => {
  const isAuth = !!localStorage.getItem('@token'); 
  return isAuth ? children : <Navigate to="/login" />;
};

export function AppRoutes() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/login" element={<Login />} />
        
        {/* Rota Protegida */}
        <Route path="/dashboard" element={
          <PrivateRoute>
            <Dashboard />
          </PrivateRoute>
        } />
      </Routes>
    </BrowserRouter>
  );
}

```

---

## Hooks Essenciais

### `useState`

```tsx
const [user, setUser] = useState<{name: string} | null>(null);

```

### `useEffect`

```tsx
useEffect(() => {
  // Código de montagem/atualização
  return () => { /* Cleanup - desmontagem */ };
}, [dependencia]);

```

### `useRef` (Acesso ao DOM)

```tsx
const inputRef = useRef<HTMLInputElement>(null);
const focar = () => inputRef.current?.focus();

```

---

## Renderização e Listas

### Condicional

```tsx
{isLogged ? <UserMenu /> : <LoginBtn />}
{data.length === 0 && <p>Nenhum item encontrado.</p>}

```

### Listas (Keys são obrigatórias)

```tsx
{items.map((item) => (
  <li key={item.id}>{item.text}</li>
))}

```

---

## Consumo de API

### Fetch (Nativo)

```tsx
const getData = async () => {
  const response = await fetch('https://api.site.com/users');
  const data = await response.json();
  setUsers(data);
};

```

### Axios (Recomendado)

`npm install axios`

```tsx
import axios from 'axios';

const api = axios.create({ baseURL: 'https://api.site.com' });

const createUser = async (userData: object) => {
  try {
    const res = await api.post('/users', userData);
    return res.data;
  } catch (err) {
    console.error("Erro na chamada", err);
  }
};

```

---

## Formulários (Controlados)

```tsx
export function MyForm() {
  const [email, setEmail] = useState('');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log("Enviando para o servidor:", email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="email" 
        value={email} 
        onChange={(e) => setEmail(e.target.value)} 
      />
      <button type="submit">Salvar</button>
    </form>
  );
}

```

---

## Eventos Comuns (TS)

* `onClick={(e: React.MouseEvent) => ...}`
* `onChange={(e: React.ChangeEvent<HTMLInputElement>) => ...}`
* `onSubmit={(e: React.FormEvent) => ...}`

---

### Pro-tip: Estrutura de Pastas

```text
src/
 ├── @types/      # Definições de tipos globais
 ├── assets/      # Imagens e fontes locais
 ├── components/  # Componentes pequenos (Button, Input)
 ├── pages/       # Páginas da aplicação (Home, Login)
 ├── services/    # Configuração do Axios/API
 ├── styles/      # Estilos globais (GlobalStyles.ts)
 └── routes.tsx   # Configuração das rotas

```

---

**Deseja que eu crie um exemplo de "Custom Hook" para gerenciar requisições de API de forma automática?**
