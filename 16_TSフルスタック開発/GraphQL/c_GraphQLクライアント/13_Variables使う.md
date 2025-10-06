# graphql-requestでVariablesを使う

作成日 2025/10/06

## うまくいったサンプルコード

```javascript
import { gql, GraphQLClient } from 'graphql-request';

const document = gql`
query GetPokemonInfo ($pokeName: String!) {
    pokemon(name: $pokeName) {
        id
        name
        image
        evolutions {
            id
            name
            image
        }
    }
}
`;

const variables = {
  pokeName: "eevee"
};

const endpoint = 'https://graphql-pokemon2.vercel.app/';
const client = new GraphQLClient(endpoint);

async function fetchData() {
    try {
    const data = await client.request(document, variables);
    console.log(data);
    } catch (error) {
    console.error("Error fetching data:", error);
    }
}

fetchData();
```

## 戻り値

```text
{
  pokemon: {
    id: 'UG9rZW1vbjoxMzM=',
    name: 'Eevee',
    image: 'https://img.pokemondb.net/artwork/eevee.jpg',
    evolutions: [ [Object], [Object], [Object] ]
  }
}```
