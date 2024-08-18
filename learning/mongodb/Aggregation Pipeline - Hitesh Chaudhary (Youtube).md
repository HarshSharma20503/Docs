**Reference Link** : [Youtube Link](https://www.youtube.com/playlist?list=PLRAV69dS1uWQ6CZCehxKy0rjkqhQ2Z88t)

Here we are doing question based learning so only questions and answers are provided.

**Q1.** **How many active users are there?**
```jsx
[
    {
        $match : { isActive : true }        
    },
    {
        $count : "activeUsersCount"
    }
]
```

- **Explanation:**
    - **$match**: Filters users with `isActive: true`.
    - **$count**: Counts the resulting users and outputs the count as `activeUsersCount`.
- **Note:** Each stage processes the output of the previous stage.

---

**Q2. What is the average age of all users?**

```jsx
[
    {
        $group : {
            _id : null,
            averageAge : { $avg : "$age" }
        }      
    }
]
```

- **Explanation:**
    - **$group**: Groups all users into a single group with `_id: null`.
    - **$avg**: Calculates the average value of the `age` field and stores it as `averageAge`.
- **Note:** Setting `_id: null` groups all documents together.

---

**Q3. List the top 5 most common favorite fruits among the users?**

```jsx
[
    {
        $group : {
            _id : "$favoriteFruit",
            count : {
                $sum : 1
            }
        }      
    },
    {
        $sort : {
            count : -1 // descending order for ascending write 1
        }
    },
    {
        $limit : 5
    }
]
```

- **Explanation:**
    - **\$group**: Groups users by `favoriteFruit` and counts occurrences with `$sum: 1`.
    - **$sort**: Sorts the fruits by `count` in descending order.
    - **$limit**: Limits the output to the top 5 fruits.
- **Note:** Sorting and limiting are key to finding the top results.

---

**Q4. Find the total number of males and the females.**

```jsx
[
    {
        $group : {
            _id : "$gender",
            genderCount : {
                $sum : 1
            }
        }      
    },
]
```

- **Explanation:**
    - **$group**: Groups users by `gender`.
    - **$sum**: Counts the number of documents for each gender (e.g., male and female) and stores the count as `genderCount`.
- **Note:** The result will show the total count for each gender.

---

**Q5. Which country has the highest number of the registered users?**

```jsx
[
    {
        $group : {
            _id : "$company.location.country",
            count : {
                $sum : 1
            }
        }      
    },
    {
        $sort : {
            count : -1
        }
    },
    {
        $limit : 1
    }
]
```

- **Explanation:**
    - **$group**: Groups users by `company.location.country` and counts the number of users per country.
    - **$sort**: Sorts the countries by user count in descending order.
    - **$limit**: Limits the result to the top country with the highest user count.
- **Note:** This gives the country with the most registered users.
---

**Q6. List all the unique eye colors present in the collection.**

```jsx
[
    {
        $group : {
            _id : "$eyeColor",
        }   
    },
]
```

- **Explanation:**
    - **$group**: Groups documents by `eyeColor`, ensuring each color is listed only once as `_id`.
- **Note:** This returns all unique eye colors present in the collection.
---

**Q7. What is the average number of tags per user?**

Solution 1: Using `$unwind`
```jsx
[
    {
        $unwind : "$tags"
    },
    {
        $group : {
            _id : "$_id",
            numberOfTags : {
                $sum : 1
            }
        }
    },
    {
        $group : {
            _id : null,
            averageNumberOfTags : {
                $avg : "$numberOfTags"
            }
        }
    }
]
```

**Explanation:**
- **$unwind**: Deconstructs the `tags` array into multiple documents.
- **$group**: Groups by user `_id` to count `numberOfTags`.
- **$group** (again): Calculates the average `numberOfTags` across all users.

Solution 2: Using `$addFields`
```jsx
[
    {
        $addFields : {
            numberofTags : {
                $size : {
                    $ifNull : ["$tags", []]
                }
            }
        }
    },
    {
        $groups : {
            _id : "$numberofTags"
            averageNumberOfTags : {
                $avg : "$numberofTags"
            }
        }
    }
]
```

- **Explanation:**
    - **\$addFields**: Adds `numberOfTags` by calculating the size of the `tags` array. Handles null/empty arrays with `$ifNull`.
    - **$group**: Calculates the average `numberOfTags` across all users.
- **Note:** Both approaches achieve the same goal but use different methods to calculate the average number of tags per user.

---

**Q8. How many users have the tag ‘enim’ in the tags array?**

```jsx
[
    {
        $match : {
            tags : 'enim'
        }
    },
    {
        $count : "userWithEnimTag"
    }
]
```

- **Explanation:**
    - **$match**: Filters users who have the tag `'enim'` in their `tags` array.
    - **$count**: Counts the number of users with the `'enim'` tag and outputs it as `userWithEnimTag`.
- **Note:** This finds all users whose `tags` array contains the specific tag `'enim'`.

---

**Q9. What are the name and age of the users who are inactive and have ‘velit’ as a tag?**

```jsx
[
    {
        $match : {
            isActive : false,
            tags : "velit"
        }
    },
    {
        $project : {
            name : 1,
            age : 1
        }
    }
]
```

- **Explanation:**
    
    - **$match**: Filters users who are inactive (`isActive: false`) and have `'velit'` in their `tags` array.
    - **$project**: Includes only the `name` and `age` fields in the output.
- **Note:** The `$project` stage ensures that only the required fields are returned.

---

**Q10. How many users have phone number starting with ‘+1 (940)’ ?**

```jsx
[
    {
        $match : {
            "company.phone" : "/^\\+1 \\(940\\)/",
            
        }
    },
    {
        $count : "usersWithSpecialPhoneNumber"
    }
]
```

- **Explanation:**
    - **\$match**: Filters users whose `company.phone` starts with `+1 (940)` using a regular expression (`$regex`).
    - **$count**: Counts the number of users with matching phone numbers and outputs as `usersWithSpecialPhoneNumber`.
- **Note:** The `$regex` operator is used for pattern matching in MongoDB queries.

---

**Q11. Who has registered the most recently?**

```jsx
[
    {
        $sort : {
            registered : -1
        }
    },
    {
        $limit : 1
    },
    {
        $project : {
            name : 1,
            registered : 1
        }
    }
]
```

- **Explanation:**
    - **$sort**: Sorts users by the `registered` field in descending order (most recent first).
    - **$limit**: Limits the result to the top 1 user.
    - **$project**: Includes only the `name` and `registered` fields in the output.
- **Note:** This pipeline identifies the user who registered most recently by sorting and limiting the results.

---

**Q12. Categories user by there favorite fruit?**

```jsx
[
    {
        $group : {
            _id : "$favoriteFruit",
            users : {
                $push : "$name"
            }
        }
    },
]
```

- **Explanation:**
    - **$group**: Groups users by `favoriteFruit`.
    - **$push**: Collects names of users with the same `favoriteFruit` into an array called `users`.
- **Note:** This pipeline categorizes users based on their favorite fruit, with each fruit as a group containing an array of user names.

---

**Q13. How many users have ‘ad’ as the second tag in their list of tags?**

```jsx
[
    {
        $match : {
            "tags.1" : "ads"
        }
    },
    {
        $count : 'secondTagAd'
    }
]
```

- **Explanation:**
    - **$match**: Filters users where the second tag in the `tags` array (`tags[1]`) is `"ad"`.
    - **$count**: Counts the number of users matching the condition and outputs the count as `secondTagAd`.
- **Note:** Array indexing starts from 0, so `tags.1` refers to the second element in the `tags` array.
---

**Q14. Find users who have both ‘enim’ and ‘ad’ as tags.**

```jsx
[
    {
        $match : {
            tags : {
                $all : ["enim", "ad"]
            }
        }
    },
]
```

- **Explanation:**
    - **$match**: Filters users who have both `'enim'` and `'ad'` in their `tags` array.
    - **$all**: Ensures that all specified tags are present in the array.
- **Note:** This query retrieves users who have both tags in their `tags` array.
---

**Q15.** List all companies located in the USA with their corresponding user count.

```jsx
[
    {
        $match : {
            "company.location.country" : "USA"
        }
    },
    {
        $group : {
            _id: "$company.title",
            userCount : {
                $sum : 1
            }
        }
    }
]
```

- **Explanation:**
    - **$match**: Filters users whose company is located in the USA.
    - **$group**: Groups users by their company title (`$company.title`) and counts the number of users per company with `$sum: 1`.
- **Note:** This pipeline provides a list of companies in the USA along with the count of users for each company.
---

**Q16.** **Find the author details of the books.**

```jsx
[
    {
        $lookup : {
            from : "authors",
            localField : "author_id",
            foreignField : "_id",
            as : "author_details"
        }
    },
    {
        $addFields : {
            author_details : {
                $first : "$author_details"
            }
        }
    }
]
```

or

```jsx
[
    {
        $lookup : {
            from : "authors",
            localField : "author_id",
            foreignField : "_id",
            as : "author_details"
        }
    },
    {
        $addFields : {
            author_details : {
                $arrayElemAt : ["$author_details", 0]
            }
        }
    }
]
```

- **Explanation:**
    - **$lookup**: Joins the `authors` collection with the `books` collection on `author_id` and `_id`, storing the result in `author_details`.
    - **\$addFields**: Adjusts the `author_details` field to only include the first element of the array (since `$lookup` returns an array).
- **Note:** Both solutions achieve the same result, converting the array of `author_details` into a single document by extracting the first element.

---