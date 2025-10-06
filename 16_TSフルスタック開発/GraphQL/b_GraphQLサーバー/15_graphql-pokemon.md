# GraphQL Pokémon

作成日 2025/10/06

## 1. 公式サイトを読む

[GraphQL Pokemon Guide | Learn to Query Pokemon Evolution Chains](https://graphql-pokemon.vercel.app/)

ソースコード => [lucasbento/graphql-pokemon: Get information of a Pokémon with GraphQL!](https://github.com/lucasbento/graphql-pokemon)

稼働中のAPI => [GraphiQL](https://graphql-pokemon2.vercel.app/)

成功したオペレーション

```javascript
query {
    pokemon(name: "pikachu") {
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

// QUERY VARIABLES
{
  "pokeName": "eevee"
}
```
