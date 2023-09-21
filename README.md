# User_Courses
API feita para gerir rotinas de faculdade, como cadastro de cursos, alunos, usuarios administradores, matricula de alunos em cursos. Feita em Node.js, PostgreSQL e Express.js
#
## Rotas - /users e /login

### POST /users

**Entrada:** 

```
{
    "name": string,
    "email": string,
    "password": string,
    "admin": boolean
}
```
- O campo admin é opcional, false por padrão.
- Email deve estar em formato válido.

**Retorno:**

```
{
    "id": number,
    "name": string,
    "email": string,
}
```

- Tentando cadastrar um email já existente:
```
{
  "message": "Email already registered"
}
```

### POST /login

**Entrada:**

```
{
  "email": string,
  "password": string
}
```

**Retorno:**
```
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

- Email/senha incorreto:
```
{
  "message": "Wrong email/password"
}
```

### GET /users

- Somente usuários administradores possuem acesso a rota.
- Necessário encaminhar o **Bearer token** no header da requisição.
  
**Retorno:**
```
[
  {
    "id": 1,
    "name": "Ugo",
    "email": "ugo@kenzie.com.br",
    "admin": true
  },
  {
    "id": 2,
    "name": "Lucas",
    "email": "lucas@kenzie.com.br",
    "admin": false
  }
]
```

### GET /users/:id/courses

- Lista todos os cursos de um usuario por id.
- Somente usuários administradores possuem acesso a rota.
- Necessário encaminhar o **Bearer token** no header da requisição.

**Retorno:**
```
[
  {
    "couseId": 1,
    "courseName": "Frontend",
    "courseDescription": "HTML, CSS e JavaScript",
    "userActiveInCourse": true,
    "userId": 1,
    "userName": "Ugo"
  },
  {
    "couseId": 2,
    "courseName": "React",
    "courseDescription": "Biblioteca React para construção de frontend",
    "userActiveInCourse": false,
    "userId": 1,
    "userName": "Ugo"
  }
]
```

- Caso o usuário não esteja matriculado em algum curso.

```
{
  "message": "No course found"
}
```
#
## Rotas /courses

### POST /courses

- Deve ser possivel a criação de um novo curso.
- Somente usuários administradores possuem acesso a rota.
- Necessário encaminhar o **Bearer token** no header da requisição.

**Entrada:**
```
  {
  "name": string,
  "description": string
}
```

**Retorno:**
```
{
  "id": number,
  "name": string,
  "description": string
}
```

### GET /courses

- Lista todos os cursos

**Retorno:**
```
[
  {
    "id": 1,
    "name": "Frontend",
    "description": "HTML, CSS e JavaScript"
  },
  {
    "id": 2,
    "name": "React",
    "description": "Frontend com a biblioteca React"
  }
]
```

### POST /courses/:courseId/users/:userId

- Deve ser possível matricular o usuário em um curso, enviando o id do curso e o id do user no parâmetro de rota
- Somente usuários administradores possuem acesso a rota.
- Necessário encaminhar o **Bearer token** no header da requisição.

**Retorno:**
```
{
  "message": "User successfully vinculed to course"
}
```

- Caso o course ou o user não existam:

```
{
  "message": "User/course not found"
}
```

### DELETE /courses/:courseId/users/:userId

- Deve ser possível inativar a matricula de um usuário em um curso, enviando o id do curso e o id do user no parâmetro de rota:
- Somente usuários administradores possuem acesso a rota.
- Necessário encaminhar o **Bearer token** no header da requisição.

**Retorno:** 204 NO BODY

- Caso o usuario/curso não existam:

```
{
  "message": "User/course not found"
}
```

### GET /courses/:id/users:

- Lista todos os usuários vinculados a um curso.
- Somente usuários administradores possuem acesso a rota.
- Necessário encaminhar o **Bearer token** no header da requisição.

**Retorno:**
```
[
  {
    "userId": 1,
    "userName": "Ugo",
    "couseId": 1,
    "courseName": "Frontend",
    "courseDescription": "HTML, CSS e JavaScript",
    "userActiveInCourse": true
  },
  {
    "userId": 2,
    "userName": "Lucas",
    "couseId": 1,
    "courseName": "Frontend",
    "courseDescription": "HTML, CSS e JavaScript",
    "userActiveInCourse": true
  }
]
```

  

