
# Criando uma Pokédex com React

Este tutorial irá guiá-lo na criação de uma Pokédex simples usando React, passo a passo.

## 1. Configurando o Ambiente

1. **Instale o Node.js e npm**:
   - Se você ainda não tem o Node.js e npm instalados, baixe-os [aqui](https://nodejs.org/).
   - Após a instalação, verifique com os comandos:
     ```bash
     node -v
     npm -v
     ```

2. **Crie um novo projeto React**:
   - Abra o terminal e execute:
     ```bash
     npm create vite@latest pokedex
     ```
   - Isso criará uma nova aplicação React chamada "pokedex" usando o vite, não esqueça de escolher react, para esse exemplo usaremos javascript.

3. **Navegue até a pasta do projeto**:
   ```bash
   cd pokedex
   ```

4. **Instale os pacotes e inicie o servidor de desenvolvimento**:
   - Execute:
     ```bash
     npm i
     npm run dev
     ```
   - Isso vai abrir o projeto em seu navegador.

## 2. Estrutura do Projeto

Dentro da pasta `src`, organize a estrutura do projeto:

- **Crie uma pasta chamada `components`** dentro de `src` para armazenar seus componentes.
- **Crie uma pasta chamada `services`** dentro de `src` para armazenar as funções de API.

## 3. Configurando a API

Vamos usar a [PokeAPI](https://pokeapi.co/) para buscar os dados dos Pokémon.

1. **Crie um serviço para fazer as requisições**:
   - Dentro de `src/services`, crie um arquivo `pokeapi.js`:
     ```javascript
     export const getPokemonData = async (name) => {
       try {
         const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${name}`);
         const data = await response.json();
         return data;
       } catch (error) {
         console.error("Erro ao buscar o Pokémon:", error);
       }
     };
     ```

## 4. Criando o Componente Principal

Agora, vamos criar um componente para exibir os dados do Pokémon.

1. **Crie um componente chamado `Pokedex.js`**:
   - Dentro de `src/components`, crie o arquivo `Pokedex.js`:
     ```javascript
     import React, { useState } from 'react';
     import { getPokemonData } from '../services/pokeapi';

     const Pokedex = () => {
       const [pokemon, setPokemon] = useState(null);
       const [search, setSearch] = useState('');

       const handleSearch = async () => {
         const data = await getPokemonData(search.toLowerCase());
         setPokemon(data);
       };

       return (
         <div>
           <h1>Pokédex</h1>
           <input 
             type="text" 
             placeholder="Nome do Pokémon" 
             value={search} 
             onChange={(e) => setSearch(e.target.value)} 
           />
           <button onClick={handleSearch}>Buscar</button>

           {pokemon && (
             <div>
               <h2>{pokemon.name}</h2>
               <img src={pokemon.sprites.front_default} alt={pokemon.name} />
               <p>Peso: {pokemon.weight}</p>
               <p>Altura: {pokemon.height}</p>
             </div>
           )}
         </div>
       );
     };

     export default Pokedex;
     ```

2. **Importe o componente na App.js**:
   - Abra o arquivo `src/App.js` e substitua o conteúdo com:
     ```javascript
     import React from 'react';
     import Pokedex from './components/Pokedex';

     function App() {
       return (
         <div className="App">
           <Pokedex />
         </div>
       );
     }

     export default App;
     ```

## 5. Executando o Projeto

Salve todas as alterações e verifique se o servidor de desenvolvimento está rodando (caso não esteja, execute `npm start` novamente).

Agora, você deve ver a sua Pokédex no navegador, onde você pode digitar o nome de um Pokémon e ver os detalhes exibidos!

## 6. Personalização e Estilização

- Você pode personalizar o estilo adicionando classes CSS ou usando frameworks como Bootstrap ou Material-UI.
- Adicione mais funcionalidades, como listagem de Pokémon, navegação por páginas, etc.
