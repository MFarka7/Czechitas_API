
### POST /user/create - happy path
endpoint: https://praha.czechibank.ostrava.digital/api/v1/user
```json
{
  "email": "papousekPepa@czechitas.cz",
  "name": "Papagaj Pepa",
  "password": "password123456"
}
```


### POST /user/create - random functions
endpoint: https://praha.czechibank.ostrava.digital/api/v1/user
```json
{
  "email": "{{$randomEmail}}",
  "name": "{{$randomFullName}}",
  "password": "{{$randomPassword}}"
}
```

Links: [Postman documentation - variable list](https://learning.postman.com/docs/tests-and-scripts/write-scripts/variables-list/)

## TESTS (Validations)
### Links
1. https://learning.postman.com/docs/tests-and-scripts/write-scripts/test-scripts/

Example for checking status code - I am expecting, that `response has status code 200`
```js
pm.test("should have correct status", function () {
  pm.response.to.have.status(200);
});
```

Example - checking payload - checking property `success` if it is true
```js
pm.test("should return success response", function () {
   var jsonData = pm.response.json();
   pm.expect(jsonData.success).to.eql(true);
});
```

Example - checking payload - if the schema is correct
```js
pm.test("should return user data", function(){
   var jsonData = pm.response.json();
   pm.expect(jsonData.data.user).to.have.property("name"); // if data.user has property `name`
   pm.expect(jsonData.data.user).to.have.property("email");  // if data.user has property `email`
   pm.expect(jsonData.data.user).to.not.have.property("password"); // if data.user has property password

   pm.expect(jsonData.data.user.name).to.eql("Test88"); // if property `data.user.name` has value "Test88"
   pm.expect(jsonData.data.user.email).to.eql("test88@czechibank.ostrava.digital"); // if property `data.user.email` has value "test88@czechibank.ostrava.digital" 
})

```

### Set Collection Variables

```js
var response = pm.response.json();
pm.collectionVariables.set("apiKeyFromResponse", response.apiKey);
pm.collectionVariables.set("bankAccountFromResponse", response.data.bankAccounts[0].number); 
```

### Your Flow
1) Create new user
2) Verify that balance on user's account is 100 000 Czechitokens
3) Create new account for that user
4) Rename that account
5) Send 100 Czechitokens from default account to newly created one
6) Verify that new balance on default account is 99 900
7) Verify that new balance on newly created account is 100




