# Получение списка нфт пользователя

Зная адрес пользователя мы можем получить список нфт на его блокчейн аккаунте:

1. Получение списка нфтей по адресу кошелька пользователя

```graphql
query nftBalances(
    $search: String
    $order: [NftBalanceSortInput!]
    $first: Int
    $after: String
    $where: NftBalanceFilterInput
) {
    nftBalances(
        search: $search - полнотекстовый поиск по названию|описанию
        first: $first
        after: $after
        order: $order
        where: $where
    ) {
        nodes {
            tokenId - ид токена
            value - количество инстансов данного токена
            chainId - блокчейн
            contractAddress - адрес нфт контракта
            owner - адрес владельца
            nftToken {
                tokenId - ид токена
                chainId - блокчейн
                chain {
                  nativeCurrency - нативная валюта блокчейна(в чем платится комиссия транзакций)
                  displayName - человекочитабельное название блокчейна
                  blockExplorerUrl - url адрес explorer
                }
                nftContract {
                    address - адрес контракта
                    logo - лого нфт контракта
                    name - название контракта
                    symbol - краткое название контракта
                    type - тип контракта ERC721 | ERC1155
                }
                contractAddress - адрес контракта
                createdAt - дата создания токена
                type - тип токена (basic, lootbox)
                total - общее количесто токена
                nftMetadata { // метадата
                    id - ид метадаты
                    tokenId - ид токена
                    name - имя нфт
                    description - описание нфт
                    image - картинка
                    animationUrl - url анимации
                    backgroundColor
                    externalUrl - url страница нфт
                    attributes { - массив аттрибутов
                        traitType - название аттрибута
                        value - значение
                    }
                    metadataUrl - url где лежит метадата, или вся метадата в json формате
                }
            }
        }
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
    }
}

```

Пример простого базового запроса: возвращает до 1000 элементов

```graphql
query nftBalances($where: NftBalanceFilterInput) {
  nftBalances(where: $where) {
    nodes {
      value
      nftToken {
        tokenId
        chainId
        contractAddress
        total
        nftMetadata {
          name
          description
          image
          animationUrl
          backgroundColor
          externalUrl
          attributes {
            traitType
            value
          }
          metadataUrl
        }
      }
    }
  }
}
```

Пример фильтра

```json
{
  "where": {
    "owner": {
      "eq": "0xc8fd80f4119dbb1e59a0ca8667447e0e36c81ea2"
    }
  }
}
```
