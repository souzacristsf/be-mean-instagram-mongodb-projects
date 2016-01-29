# MongoDb - Projeto Final  
autor - [Michel Ferreira Souza](https://github.com/souzacristsf)
Data - 24/01/2016


## Para qual sistema você usaria o MongoDB (diferente desse)?  

Rede social, BI, E-commerce, entre outros. Outros ganhos que poderiamos obter usando MongoDB seria, consulta simples, Sharding, GridFs.


## Qual a modelagem da sua coleção de `users`?  
```js
users{
        "auth": [{
           "username": String,  
           "password": String, 
           "email": String, 
           "last_access": Date,  
           "online": boolean,  
           "disable": boolean,  
           "hash_token": String 
        }],
        "name": String,
        "bio": String,
        "date_register": Date,
        "avatar_path": String
}
```


## Qual a modelagem da sua coleção de `projects`?  

```
projects{
         "name": String,
         "description": String,
         "date_begin": Date,
         "date_dream": Date,
         "date_end": Date,
         "visible": boolean,
         "realocate": boolean,
         "expired": Date,
         "visualizable_mod": String,
         "members": [{
            "user_id" : [{ObjectId}],
            "type_member" : String,
            "notify" : boolean
         }],
         "goals": [{
            "name": String,
            "description": String,
            "date_begin": Date,
            "date_dream": Date,
            "date_end": Date,
            "realocate": boolean,
            "expired": Date,
            "tags": [],
            "historic": [{
               "data_realocate" : Date
            }],
            "activities": [
                { 
                  "activity_id": ObjectId 
                }
            ]
         }],
        "tags": [],
        "comments": [{
               "user_id: [{ObjectId}],
               "text": String,
               "date_comment": Date,
               "notify" : boolean,
               "files": [{
                   "name": String,
                   "size": String>,
                   "data": Date,
                   "type": String
        }]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?  
```js
activities{
           "name": String,
           "description": String,
           "date_begin": Date,
           "date_dream": Date,
           "date_end": Date,
           "realocate": boolean,
           "expired": Date,
           "tags": [],
           "historic": [{
               "data_realocate" : Date
           }],
           "user_id": [{ObjectId}],
           "comments": [{
               "user_id": ObjectId,
               "text": String,
               "date_comment": Date,
               "notify" : boolean,
               "files": [{
                   "name": String,
                   "size": String,
                   "data": Date,
                   "type": String
               }]
           }]
}
```
> olhando para o modelo relacional, minha ideia era fazer 4 coleção, onde teriamos;

 - Comments -> "Nesse caso, olhando para o modelo teriamos **comment** e **file** na coleção `projects` e `activities`, "Redundância". Para os dois modelos dado aqui, usariamos o **GridFS** para armazenar e recuperar arquivos que excedessem o BSON" e deixando apenas o **comment_id** de referencia na coleção `projects` e `activities`.
 - Activities 
 - Projects
 - Users

Então conversando com o Suissa que disse apenas 3 coleções `Activities`, `Projects`, `Users`, optei por esse tipo de estrutura pois foi o melhor caminho que achei. Onde o projeto poderá ter varias atividades e nessas atividades podem existir um ou muitos usuarios envolvidos da mesma maneira os comentarios e arquivos.


## Create - cadastro  

**Usuários**

Segue os usuario cadastrados no banco:

```js
Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "ffamoso",  
...            "password": "ff102030", 
...            "email": "f@famoso.com.br", 
...            "last_access": Date.now(),  
...            "online": false,  
...            "disable": false,  
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ1" 
...         }],
...         "name": "Fulano Famoso",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 967ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "bbeleza",  
...            "password": "bb102030", 
...            "email": "b@beleza.com.br", 
...            "last_access": Date.now(),  
...            "online": false,  
...            "disable": false,  
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ2" 
...         }],
...         "name": "Beltrano Beleza",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "spano",
...            "password": "sp102030", 
...            "email": "s@pano.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false,
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ3" 
...         }],
...         "name": "Sicrano Pe de Pano",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "mtranquilidade",
...            "password": "mt102030", 
...            "email": "m@tranquilidade.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false,
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ4" 
...         }],
...         "name": "Mariazinha Tranquilidade",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "jgrande",
...            "password": "jg102030", 
...            "email": "j@grande.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false,
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ5" 
...         }],
...         "name": "juliana do Pe Grande",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "msouza",
...            "password": "ms102030", 
...            "email": "m@souza.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false, 
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ6" 
...         }],
...         "name": "Michel Ferreira Souza",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "rrua",
...            "password": "rr102030", 
...            "email": "r@rua.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false, 
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ7" 
...         }],
...         "name": "Roberto do Outro Lado da Rua",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "jesquina",
...            "password": "je102030", 
...            "email": "j@esquina.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false,
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ8" 
...         }],
...         "name": "Juliao da Esquina",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "rgugu",
...            "password": "rg102030", 
...            "email": "r@gugu.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false, 
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9" 
...         }],
...         "name": "Rodolfo do Domingo do Gugu",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 5ms
WriteResult({
  "nInserted": 1
})

Souza(mongod-3.0.8) be-mean-project> db.users.save({
...         "auth": [{
...            "username": "erocha",
...            "password": "er102030", 
...            "email": "e@rocha.com.br", 
...            "last_access": Date.now(),
...            "online": false,
...            "disable": false,
...            "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJA" 
...         }],
...         "name": "Ezequiel Antero Rocha",
...         "bio": null,
...         "date_register": Date.now(),
...         "avatar_path": null
... })
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})

```

### Projetos  

1 - Cadastre 10 usuários diferentes.
2 - Cadastre 5 projetos diferentes.
      - cada um com 5 membros, sempre diferentes dentro dos projetos;
      - cada um com pelo menos 3 tags diferentes;
         - escolha 1 tag onde deva ficar em 2 projetos;
         - escolha 1 tag onde deva ficar em 3 projetos;
      - cada projeto com pelo menos 1 goal;
         - cada goal com pelo menos 3 tags;
         - cada goal com pelo menos 2 atividades, deixe 1 projeto sem.
```js
Souza(mongod-3.0.8) be-mean-project> db.users.find({},{_id: 1})
{
  "_id": ObjectId("56a4f94e06ee6e088e75c90b")
}
{
  "_id": ObjectId("56a4fade06ee6e088e75c90c")
}
{
  "_id": ObjectId("56a4fae606ee6e088e75c90d")
}
{
  "_id": ObjectId("56a4faf106ee6e088e75c90e")
}
{
  "_id": ObjectId("56a4fafe06ee6e088e75c90f")
}
{
  "_id": ObjectId("56a4fb0806ee6e088e75c910")
}
{
  "_id": ObjectId("56a4fb1306ee6e088e75c911")
}
{
  "_id": ObjectId("56a4fb1d06ee6e088e75c912")
}
{
  "_id": ObjectId("56a4fba106ee6e088e75c913")
}
{
  "_id": ObjectId("56a4fba406ee6e088e75c914")
}

Souza(mongod-3.0.8) be-mean-project> var idUsers = db.users.find({}, {_id: 1}).toArray()

Souza(mongod-3.0.8) be-mean-project> db.projects.insert({
         "name": "Lagoa",
         "description": "Sistema para limpar a lagoa",	
         "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
         "date_dream": ISODate("2016-03-01T09:00:00Z"),
         "date_end": null,
         "visible": true,
         "realocate": null,
         "expired": ISODate("2016-03-01T09:00:00Z"),
         "visualizable_mod": null,
         "members": [
         {
            "user_id" : idUsers[0]._id,
            "type_member": "owner", 
            "notify" : true
         },
         {
            "user_id" : idUsers[1]._id,
            "type_member": "member", 
            "notify" : false
         },
         {
            "user_id" : idUsers[2]._id,
            "type_member": "leader", 
            "notify" : true
         },
         { 
            "user_id" : idUsers[3]._id,
            "type_member": "voluntary", 
            "notify" : false
         },
         {
            "user_id" : idUsers[4]._id,
            "type_member": "member", 
            "notify" : false
         }
         ],
         "goals": [{
            "name": "Iniciar o start",
            "description": "començando a porra do objetivo",
            "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
            "date_dream": ISODate("2016-03-01T09:00:00Z"),
            "date_end": null,
            "realocate": null,
            "expired": ISODate("2016-03-01T09:00:00Z"),
            "tags": ["start","restart","reeestart"],
            "historic": [{
               "data_realocate" : null
            }],
            "activities": [
                { "activity_id": ObjectId("56a6bcc37c93e0301539990d") },
		{ "activity_id": ObjectId("56a6bcdc7c93e0301539990e") }
            ]
         }],
        "tags": ["NodeJs","Redis","AngularJS 2"],
        "comments": [{
               "user_id": null,
               "text": null,
               "date_comment": null,
               "notify" : null,
               "files": [{
                   "name": null,
                   "size": null,
                   "data": null,
                   "type": null
               }]
        }]
})

Souza(mongod-3.0.8) be-mean-project> db.projects.insert({
         "name": "Recuperação Florestal",
         "description": "Sistema para colorir a fazenda zica do pantano, muita viagem",	
         "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
         "date_dream": ISODate("2016-03-01T09:00:00Z"),
         "date_end": null,
         "visible": true,
         "realocate": null,
         "expired": ISODate("2016-03-01T09:00:00Z"),
         "visualizable_mod": null,
         "members": [{
            "user_id" : idUsers[5]._id,
            "type_member": "owner", 
            "notify" : true
         },
         {
            "user_id" : idUsers[6]._id,
            "type_member": "member", 
            "notify" : false
         },
         {
            "user_id" : idUsers[7]._id,
            "type_member": "leader", 
            "notify" : true
         },
         { 
            "user_id" : idUsers[8]._id,
            "type_member": "voluntary", 
            "notify" : false
         },
         {
            "user_id" : idUsers[9]._id,
            "type_member": "member", 
            "notify" : false
         }],
         "goals": [{
            "name": "Vamos Brilhar",
            "description": "Meu jardim do jeito que eu queria",
            "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
            "date_dream": ISODate("2016-03-01T09:00:00Z"),
            "date_end": null,
            "realocate": null,
            "expired": ISODate("2016-03-01T09:00:00Z"),
            "tags": ["Azul","Verde","Vermelho"],
            "historic": [{
               "data_realocate" : null
            }],
            "activities": [
                { "activity_id": ObjectId("56a6be2a6aa8571b1c114acd") },
		{ "activity_id": ObjectId("56a6be2a6aa8571b1c114ace") }
            ]
         }],
        "tags": ["HTML","Jquery","Mongodb"],
        "comments": [{
               "user_id": null,
               "text": null,
               "date_comment": null,
               "notify" : null,
               "files": [{
                   "name": null,
                   "size": null,
                   "data": null,
                   "type": null
               }]
        }]
})

Souza(mongod-3.0.8) be-mean-project> db.projects.insert({
         "name": "Dengue Mata",
         "description": "Sistema para acabar com o mosquito aedes aegypti",	
         "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
         "date_dream": ISODate("2016-03-01T09:00:00Z"),
         "date_end": null,
         "visible": true,
         "realocate": null,
         "expired": ISODate("2016-03-01T09:00:00Z"),
         "visualizable_mod": null,
         "members": [{
            "user_id" : idUsers[1]._id,
            "type_member": "owner", 
            "notify" : true
         },
         {
            "user_id" : idUsers[9]._id,
            "type_member": "member", 
            "notify" : false
         },
         {
            "user_id" : idUsers[2]._id,
            "type_member": "leader", 
            "notify" : true
         },
         { 
            "user_id" : idUsers[8]._id,
            "type_member": "voluntary", 
            "notify" : false
         },
         {
            "user_id" : idUsers[3]._id,
            "type_member": "member", 
            "notify" : false
         }],
         "goals": [{
            "name": "Matar",
            "description": "Matar para não levar picada",
            "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
            "date_dream": ISODate("2016-03-01T09:00:00Z"),
            "date_end": null,
            "realocate": null,
            "expired": ISODate("2016-03-01T09:00:00Z"),
            "tags": ["Voa","Picada","Tem Asa"],
            "historic": [{
               "data_realocate" : null
            }],
            "activities": [
                { "activity_id": ObjectId("56a6be2a6aa8571b1c114acf") },
		{ "activity_id": ObjectId("56a6be2a6aa8571b1c114ad0") }
            ]
         }],
        "tags": ["Mongodb","Redis","Neo4j"],
        "comments": [{
               "user_id": null,
               "text": null,
               "date_comment": null,
               "notify" : null,
               "files": [{
                   "name": null,
                   "size": null,
                   "data": null,
                   "type": null
               }]
        }]
})

Souza(mongod-3.0.8) be-mean-project> db.projects.insert({
         "name": "Matagal",
         "description": "Sistema para limpar a fazenda zica do pantano",	
         "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
         "date_dream": ISODate("2016-03-01T09:00:00Z"),
         "date_end": null,
         "visible": true,
         "realocate": null,
         "expired": ISODate("2016-03-01T09:00:00Z"),
         "visualizable_mod": null,
         "members": [{
            "user_id" : idUsers[7]._id,
            "type_member": "owner", 
            "notify" : true
         },
         {
            "user_id" : idUsers[4]._id,
            "type_member": "member", 
            "notify" : false
         },
         {
            "user_id" : idUsers[6]._id,
            "type_member": "leader", 
            "notify" : true
         },
         { 
            "user_id" : idUsers[5]._id,
            "type_member": "voluntary", 
            "notify" : false
         },
         {
            "user_id" : idUsers[0]._id,
            "type_member": "member", 
            "notify" : false
         }],
         "goals": [{
            "name": "Limpeza Total",
            "description": "Só paramos quando terminar",
            "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
            "date_dream": ISODate("2016-03-01T09:00:00Z"),
            "date_end": null,
            "realocate": null,
            "expired": ISODate("2016-03-01T09:00:00Z"),
            "tags": ["Machado","Enxada","Seh Pah"],
            "historic": [{
               "data_realocate" : null
            }],
            "activities": [
                { "activity_id": ObjectId("56a6be2a6aa8571b1c114ad1") },
		{ "activity_id": ObjectId("56a6be2c6aa8571b1c114ad2") }
            ]
         }],
        "tags": ["Mongodb","Redis","Neo4j"],
        "comments": [{
               "user_id": null,
               "text": null,
               "date_comment": null,
               "notify" : null,
               "files": [{
                   "name": null,
                   "size": null,
                   "data": null,
                   "type": null
               }]
        }]
})

Souza(mongod-3.0.8) be-mean-project> db.projects.insert({
         "name": "Horario de Verão",
         "description": "Sistema para controlar o horario de verao e reduzir custo",	
         "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
         "date_dream": ISODate("2016-03-01T09:00:00Z"),
         "date_end": null,
         "visible": true,
         "realocate": null,
         "expired": ISODate("2016-03-01T09:00:00Z"),
         "visualizable_mod": null,
         "members": [{
            "user_id" : idUsers[1]._id,
            "type_member": "owner", 
            "notify" : true
         },
         {
            "user_id" : idUsers[5]._id,
            "type_member": "member", 
            "notify" : false
         },
         {
            "user_id" : idUsers[6]._id,
            "type_member": "leader", 
            "notify" : true
         },
         { 
            "user_id" : idUsers[0]._id,
            "type_member": "voluntary", 
            "notify" : false
         },
         {
            "user_id" : idUsers[7]._id,
            "type_member": "member", 
            "notify" : false
         }],
         "goals": [{
            "name": "Força para começar",
            "description": "començando a porra do objetivo",
            "date_begin": ISODate("2013-01-05T04:14:20.480Z"),
            "date_dream": ISODate("2016-03-01T09:00:00Z"),
            "date_end": null,
            "realocate": null,
            "expired": ISODate("2016-03-01T09:00:00Z"),
            "tags": ["Nos que tá","Nos que voa","Nos somos loko"],
            "historic": [{
               "data_realocate" : null
            }],
            "activities": []
         }],
        "tags": ["Express","Ionic","Cordova", "NodeJs"],
        "comments": [{
               "user_id": null,
               "text": null,
               "date_comment": null,
               "notify" : null,
               "files": [{
                   "name": null,
                   "size": null,
                   "data": null,
                   "type": null
               }]
        }]
})
```
## Retrieve - busca

#### 1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
Souza(mongod-3.0.8) be-mean-project> var membersId = db.projects.find({name: /lagoa/i}, {_id: 0, members.user_id: 1})
Souza(mongod-3.0.8) be-mean-project> membersId
[
  {
    "members": [
      {
        "user_id": ObjectId("56a4f94e06ee6e088e75c90b")
      },
      {
        "user_id": ObjectId("56a4fade06ee6e088e75c90c")
      },
      {
        "user_id": ObjectId("56a4fae606ee6e088e75c90d")
      },
      {
        "user_id": ObjectId("56a4faf106ee6e088e75c90e")
      },
      {
        "user_id": ObjectId("56a4fafe06ee6e088e75c90f")
      }
    ]
  }
]

Souza(mongod-3.0.8) be-mean-project> var members = [];
Souza(mongod-3.0.8) be-mean-project> var membersId = db.projects.find({name: /lagoa/i}, {_id: 0, "members.user_id": 1})
Souza(mongod-3.0.8) be-mean-project> var setMembersArray = function (member) {members.push(db.users.findOne({ _id: member.user_id }))};
Souza(mongod-3.0.8) be-mean-project> membersId.members.forEach(setMembersArray); 

{
  "_id": ObjectId("56a4f94e06ee6e088e75c90b"),
  "auth": [
    {
      "username": "ffamoso",
      "password": "ff102030",
      "email": "f@famoso.com.br",
      "last_access": 1453652302973,
      "online": false,
      "disable": false,
      "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ1"
    }
  ],
  "name": "Fulano Famoso",
  "bio": null,
  "date_register": 1453652302973,
  "avatar_path": null
},
{
  "_id": ObjectId("56a4fade06ee6e088e75c90c"),
  "auth": [
    {
      "username": "bbeleza",
      "password": "bb102030",
      "email": "b@beleza.com.br",
      "last_access": 1453652702854,
      "online": false,
      "disable": false,
      "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ2"
    }
  ],
  "name": "Beltrano Beleza",
  "bio": null,
  "date_register": 1453652702854,
  "avatar_path": null
},
{
  "_id": ObjectId("56a4fae606ee6e088e75c90d"),
  "auth": [
    {
      "username": "spano",
      "password": "sp102030",
      "email": "s@pano.com.br",
      "last_access": 1453652710878,
      "online": false,
      "disable": false,
      "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ3"
    }
  ],
  "name": "Sicrano Pe de Pano",
  "bio": null,
  "date_register": 1453652710878,
  "avatar_path": null
},
{
  "_id": ObjectId("56a4faf106ee6e088e75c90e"),
  "auth": [
    {
      "username": "mtranquilidade",
      "password": "mt102030",
      "email": "m@tranquilidade.com.br",
      "last_access": 1453652721950,
      "online": false,
      "disable": false,
      "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ4"
    }
  ],
  "name": "Mariazinha Tranquilidade",
  "bio": null,
  "date_register": 1453652721950,
  "avatar_path": null
},
{
  "_id": ObjectId("56a4fafe06ee6e088e75c90f"),
  "auth": [
    {
      "username": "jgrande",
      "password": "jg102030",
      "email": "j@grande.com.br",
      "last_access": 1453652734630,
      "online": false,
      "disable": false,
      "hash_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ5"
    }
  ],
  "name": "juliana do Pe Grande",
  "bio": null,
  "date_register": 1453652734630,
  "avatar_path": null
}

```

#### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```js
Souza(mongod-3.0.8) be-mean-project> db.projects.find({tags: {$eq: "Mongodb"}},{name: 1})
{
  "_id": ObjectId("56a5147bef71215f519d076a"),
  "name": "Recuperação Florestal"
}
{
  "_id": ObjectId("56a5147bef71215f519d076b"),
  "name": "Dengue Mata"
}
{
  "_id": ObjectId("56a5147bef71215f519d076c"),
  "name": "Matagal"
}

```
#### 3. Liste apenas os nomes de todas as atividades para todos os projetos.
```js

var nameActivitiesId = db.activities.find({},{_id: 1, name: 1});

var nameProjectActivitie = function(activities) {
    nomeProjetoAtividade = { projeto: "", atividades: "" };

    activities.forEach(function(a){ 
        var projects = db.projects.find({"goals.activities.activity_id": {$exists: true}, "goals.activities.activity_id": a._id});
        projects.forEach(function(p){ 
            nomeProjetoAtividade.projeto = p.name;
            nomeProjetoAtividade.atividades = a.name;
        })
    });

    return nomeProjetoAtividade;
};

nameProjectActivitie(nameActivitiesId);


```
 

#### 4. Liste todos os projetos que não possuam uma tag.


#### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
```js
var neUser = [];

var membersProjects = db.projects.findOne({},{members: 1, _id: 0});

var getUser = function(data){neUser.push(db.users.find(data.user_id, {name: 1}))._id}

membersProjects.members.forEach(getUser)

db.users.find(neUser);

## Update - alteração

#### 1. Adicione para todos os projetos o campo `views: 0`.
#### 2. Adicione 1 tag diferente para cada projeto.
#### 3. Adicione 2 membros diferentes para cada projeto.
#### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
#### 5. Adicione 1 projeto inteiro com UPSERT.

## Delete - remoção

#### 1. Apague todos os projetos que não possuam tags.
#### 2. Apague todos os projetos que não possuam comentários nas atividades.
#### 3. Apague todos os projetos que não possuam atividades.
#### 4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
#### 5. Apague todos os projetos que possuam uma determinada tag em goal.

## Gerenciamento de usuários

#### 1. Crie um usuário com permissões **APENAS** de Leitura.
#### 2. Crie um usuário com permissões de Escrita e Leitura.
#### 3. Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
#### 4. Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
#### 5. Listar todos os usuários com seus papéis e ações.

## Cluster

Depois de criada toda sua base você deverá criar um cluster utilizando:

 - 1 Router
 - 1 Config Server
 - 3 Shardings
 - 3 Replicas















































```
