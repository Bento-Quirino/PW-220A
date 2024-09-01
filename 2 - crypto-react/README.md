
# Criando uma Mini Aplicação de Cotação de Criptomoedas com React

Este tutorial irá guiá-lo na criação de uma aplicação simples para exibir cotações de criptomoedas usando React, passo a passo.

## 1. Configurando o Ambiente

O ambiente de desenvolvimento é o mesmo do projeto anterior. Caso já tenha o Node.js e o npm instalados, você pode pular esta parte.

## 2. Criando o Projeto

1. **Crie um novo projeto React**:
   - Abra o terminal e execute:
     ```bash
     npm create vite@latest crypto-quote
     ```
   - Isso criará uma nova aplicação React chamada "crypto-quote".

2. **Navegue até a pasta do projeto**:
   ```bash
   cd crypto-quote
   ```

3. **Instale os pacotes e inicie o servidor de desenvolvimento**:
   - Execute:
     ```bash
     npm i
     npm run dev
     ```
   - Isso vai abrir o projeto em seu navegador.

## 3. Estrutura do Projeto

Organize a estrutura dentro da pasta `src`:

- **Crie uma pasta chamada `components`** para armazenar seus componentes.
- **Crie uma pasta chamada `services`** para armazenar as funções de API.

## 4. Configurando a API

Vamos utilizar a [CoinGecko API](https://www.coingecko.com/en/api/documentation) para buscar as cotações de criptomoedas. Essa API não requer autenticação e suporta CORS.

1. **Crie um serviço para fazer as requisições**:
   - Dentro de `src/services`, crie um arquivo `cryptoApi.js`:
     ```javascript
     export const getCryptoData = async (coinId) => {
       try {
         const response = await fetch(\`https://api.coingecko.com/api/v3/simple/price?ids=\${coinId}&vs_currencies=usd\`);
         const data = await response.json();
         return data;
       } catch (error) {
         console.error("Erro ao buscar a cotação:", error);
       }
     };
     ```

## 5. Criando o Componente Principal

Agora, vamos criar um componente para exibir a cotação da criptomoeda.

1. **Crie um componente chamado `CryptoQuote.js`**:
   - Dentro de `src/components`, crie o arquivo `CryptoQuote.js`:
     ```javascript
     import React, { useState } from 'react';
     import { getCryptoData } from '../services/cryptoApi';

     const CryptoQuote = () => {
       const [crypto, setCrypto] = useState(null);
       const [search, setSearch] = useState('');

       const handleSearch = async () => {
         const data = await getCryptoData(search.toLowerCase());
         setCrypto(data);
       };

       return (
         <div>
           <h1>Cotação de Criptomoedas</h1>
           <input 
             type="text" 
             placeholder="ID da Criptomoeda (e.g., bitcoin)" 
             value={search} 
             onChange={(e) => setSearch(e.target.value)} 
           />
           <button onClick={handleSearch}>Buscar</button>

           {crypto && crypto[search] && (
             <div>
               <h2>{search.toUpperCase()}</h2>
               <p>Preço: ${crypto[search].usd}</p>
             </div>
           )}
         </div>
       );
     };

     export default CryptoQuote;
     ```

2. **Importe o componente na App.js**:
   - Abra o arquivo `src/App.js` e substitua o conteúdo com:
     ```javascript
     import React from 'react';
     import CryptoQuote from './components/CryptoQuote';

     function App() {
       return (
         <div className="App">
           <CryptoQuote />
         </div>
       );
     }

     export default App;
     ```

## 6. Executando o Projeto

Salve todas as alterações e verifique se o servidor de desenvolvimento está rodando (`npm start` se necessário).

Você deve ver a sua aplicação no navegador, onde pode digitar o ID de uma criptomoeda (por exemplo, "bitcoin") e obter a cotação em USD.

## 7. Personalização e Estilização

- Personalize o estilo com CSS ou frameworks como Bootstrap.
- Adicione mais funcionalidades, como a exibição de múltiplas criptomoedas, gráficos de preços, etc.
