
![](https://i.imgur.com/HpMehrs.png)
# Table of Contents
- [Problem](#problem)
- [Solution](#solution)
- [Technologies](#technologies)
- [Usage](#usage)
- [Examples](#examples)
  * [Find Student by ID](#find-student-by-id)
  * [Show all Classes](#show-all-classes)
  * [Create a Student](#create-a-student)
  * [Find Student](#find-student)
  * [Update Student](#update-student)
  * [Add Class to Student](#add-class-to-student)
  * [Remove Class from Student](#remove-class-from-student)

![Screen shot of final product](https://i.imgur.com/TbkWMaK.png)


# Problem
The [current](https://joess.mst.edu/) student service solution used by the Missouri University of Science & Technology is outdated. 

These issues include -
- Poor UX design where it is difficult to find the correct feature when needed
 - The UI colors do not agree with the university's branding color scheme
 - Cluttered, congested raw links
 - UI makes poor usage of the user viewport

![Screen Shot of current implementation of Joes'SS](https://i.imgur.com/CPQChfF.png)

# Solution
For my senior design project, my team solved this by implementing a new rendition of Joe'SS. Care was taken into improving the UX by surveying students on what they didn't like about the current solution. The UI was reworked by using university branding colors, creating a new side navigation bar & reworking the placement of the components.

# Technologies

 - [Node](https://nodejs.org/)
 - [GraphQL](https://graphql.org/)
 - [Apollo](https://www.apollographql.com/)
 - [Prisma](https://www.prisma.io/)
 - [Faker](https://github.com/marak/Faker.js/)

# Usage

1.  npm install
2.  If prisma/dev.db does not exist
    1.  npx prisma migrate save --experimental
    2.  npx prisma migrate up --experimental
    3.  npx prisma generate
    4.  node src/seed.js
    5.  npx prisma studio (Check if data seeded properly)
3.  node src/index.js
4.  Got to  [http://localhost:4000/](http://localhost:4000/)  to access the apollo backend

# Examples

## Find Student by ID

```
query {
  student(id: $id){
    id
    name
    email
    classes{
      title
    }
    advisor {
      name
    }
  }
}

```

## Show all Classes

```
query {
	classes{
    title
  }
}

```

## Create a Student

```
mutation {
  createStudent(email: "dcsbc8@mst.edu", name: "Devinda Senanayaka", advisorId: 2){
    name
    id
    email
  }
}

```

## Find Student

```
query{
  student(id: 2){
    name
    email
  }
}

```

## Update Student

```
mutation{
  updateStudent(id: 5, name: "Dev", advisorId: 3){
    name
    email
    advisor{
      name
    }
  }
}

```

## Add Class to Student

```
mutation {
  addClassToStudent(studentID: 2, classID: 1){
    name
    classes{
      title
      id
    }
  }
}

```

## Remove Class from Student

```
mutation {
  removeClassToStudent(studentID: 2, classID: 1){
    name
    classes{
      title
      id
    }
  }
}
```
