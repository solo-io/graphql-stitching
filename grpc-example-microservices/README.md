## Stitching gRPC microservices

Run 
```bash
kubectl apply -f kubernetes
```
to apply all the example gRPC microservices. 

2. The services are labeled with the function discovery label
```yaml
discovery.solo.io/function_discovery: enabled
```

3. Once the gRPC microservices are deployed, gRPC reflection GraphQLApi discovery will automatically create GraphQLApis.
3. Check out the various endpoints in the `kubernetes/graphql-vs.yaml`.
4. Run the following query against the stitched-gateway endpoint:
```graphql
query GetUser {
  GetUser(UserSearch: { username: "wpatel" }) {
    last_name
    first_name
  }
  GetUserByCountry(UserByCountry: { country_code: "US" }) {
    users {
      first_name
      last_name
      username
    }
  }
  GetReviewsForProduct(ReviewsForProduct: { product_id: 2 }) {
    reviews {
      author {
        username
        last_name
        first_name
      }
      content
      rating
    }
  }
  GetProducts(ProductsSearch: { name: "cotton" }) {
    products {
      price
      id
      name
      seller {
        username
        first_name
        last_name
      }
    }
  }
}
```

or you can use this curl against the endpoint: 
```bash
curl --request POST \
    --header 'content-type: application/json' \
    --url http://localhost:8080/graphql-stitched \
    --data '{"query":"query GetUser {\n  GetUser(UserSearch: { username: \"wpatel\" }) {\n    last_name\n    first_name\n  }\n  GetUserByCountry(UserByCountry: { country_code: \"US\" }) {\n    users {\n      first_name\n      last_name\n      username\n    }\n  }\n  GetReviewsForProduct(ReviewsForProduct: { product_id: 2 }) {\n    reviews {\n      author {\n        username\n        last_name\n        first_name\n      }\n      content\n      rating\n    }\n  }\n  GetProducts(ProductsSearch: { name: \"cotton\" }) {\n    products {\n      price\n      id\n      name\n      seller {\n        username\n        first_name\n        last_name\n      }\n    }\n  }\n}","variables":{}}'
```