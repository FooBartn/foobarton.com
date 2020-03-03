---
title: "Cisco Devnet Associate Study Guide"
description: "Looking to pass the Cisco DevNet Associate Exam? I've compiled a few notes! -- Incomplete --"
date: 2020-02-28T16:42:37-06:00
tags: [cisco, dev, python]
---

## 1.0 Software Development and Design (15%)

### 1.1 Compare data formats (XML, JSON, YAML)

- YAML (YAML Aint Markup Language)
  - Great for configuration files
  - Uses indentation to define data structure, similar to how Python structures code.
  - Purposefully made to be easy for humans to read/write
  - Allows commenting
  - YAML Example:

  ``` yaml
    --- # Authors And Books
    authors:
      -
        first_name: Madeleine
        last_name: L'Engle
        books:
            -
              title: A Wrinkle In Time
              publisher: Ariel Books
              genres:
                -  Young Adult
                -  Science Fantasy
              pages: 218
              publish_date: 1962-01-01
              description: A Wrinkle in Time is the story of Meg Murry, a high-school-aged girl who is transported on an adventure through time and space with her younger brother Charles Wallace and her friend Calvin O'Keefe to rescue her father, a gifted scientist, from the evil forces that hold him prisoner on another planet.
  ```

- JSON (JavaScript Object Notation)
  - Great for serialized data -- communication between APIs
  - Fairly easy to read. A little more difficult to write correctly.
  - Uses {} to denote objects, [] to denote arrays, etc. More "programmatical" in nature.
  - JSON Example:

  ``` json
  {
    "authors": [
      {
        "first_name": "Madeleine",
        "last_name": "L'Engle",
        "books": [
            {
                "title": "A Wrinkle In Time",
                "publisher": "Ariel Books",
                "genres": [
                    "Young Adult",
                    "Science Fantasy"
                ],
                "pages": 218,
                "publish_date": "1962-01-01",
                "description": "A Wrinkle in Time is the story of Meg Murry, a high-school-aged girl who is transported on an adventure through time and space with her younger brother Charles Wallace and her friend Calvin O'Keefe to rescue her father, a gifted scientist, from the evil forces that hold him prisoner on another planet."
            }
        ]
      }
    ]
  }
  ```

- XML (eXtensible Markup Language)
  - Uses tags, angle brackets, etc
  - Not as common with modern APIs
  - Extremely verbose. More difficult to read and write.
  - XML Example:

  ``` xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <root>
      <authors>
        <first_name>Madeleine</first_name>
        <last_name>L&#x27;Engle</last_name>
        <books>
        <title>A Wrinkle In Time</title>
        <publisher>Ariel Books</publisher>
        <genres>Young Adult</genres>
        <genres>Science Fantasy</genres>
        <pages>218</pages>
        <publish_date>1962-01-01T00:00:00.000Z</publish_date>
        <description>A Wrinkle in Time is the story of Meg Murry, a high-school-aged girl who is transported on an adventure through time and space with her younger brother Charles Wallace and her friend Calvin O&#x27;Keefe to rescue her father, a gifted scientist, from the evil forces that hold him prisoner on another planet.</description>
        </books>
      </authors>
    </root>
  ```

### 1.2 Describe parsing of common data format to Python data structures

#### Parse YAML in Python

``` python

import yaml # Need to install pyyaml

with open('authors.json') as authors_file:
    data=authors_file.read()

authors_json = yaml.safe_load(data)
authors = authors_json['authors']

print(f'Number of Authors: {len(authors)}')
for author in authors:
    print(f'Author Name: {author["first_name"]} {author["last_name"]}')
    books = author["books"]
    for book in books:
        print(f'Title: {book["title"]}')
        print(f'Pages: {book["pages"]}')
        print(f'Description: {book["description"]}')

```

#### Parse JSON in Python

``` python

import json

with open('authors.json') as authors_file:
    data=authors_file.read()

authors_json = json.loads(data)
authors = authors_json['authors']

print(f'Number of Authors: {len(authors)}')
for author in authors:
    print(f'Author Name: {author["first_name"]} {author["last_name"]}')
    books = author["books"]
    for book in books:
        print(f'Title: {book["title"]}')
        print(f'Pages: {book["pages"]}')
        print(f'Description: {book["description"]}')

```

#### Parse XML in Python

``` python

from xml.dom.minidom import parse

domTree = parse('authors.xml')
root = domTree.documentElement

authors = root.getElementsByTagName('authors')
print(f'Number of Authors: {len(authors)}')
for author in authors:
    firstName = author.getElementsByTagName('first_name')[0]
    lastName = author.getElementsByTagName('last_name')[0]
    print(f'Author Name: {firstName.firstChild.data} {lastName.firstChild.data}')
    books = author.getElementsByTagName('books')
    for book in books:
        title = book.getElementsByTagName("title")[0]
        pages = book.getElementsByTagName("pages")[0]
        descr = book.getElementsByTagName("description")[0]
        print(f'Title: {title.firstChild.data}')
        print(f'Pages: {pages.firstChild.data}')
        print(f'Description: {descr.firstChild.data}')

```

### 1.3 Describe the concepts of test-drive development

> Test-driven development (TDD) is a software development process that relies on the repetition of a very short development cycle: requirements are turned into very specific test cases, then the software is improved so that the tests pass. This is opposed to software development that allows software to be added that is not proven to meet requirements. - [Wikipedia Link](https://en.wikipedia.org/wiki/Test-driven_development)

The ideology of Test-driven development is that you write tests before you write code. The primary goal is to ensure that the vast majority of your code works the way that it should -- and that any changes to one part of your code do not have consequences to other parts of your code you don't catch immediately. Inevitably something you didn't account for will end up failing. When it does, you write a test that covers that case so it doesn't happen again. And the cycle continues. 

For more information on TDD check out [Agile Alliance: TDD](https://www.agilealliance.org/glossary/tdd/)

### 1.4 Compare software development methods (agile, lean, waterfall)

- Agile:
  - Flexibility focused.
  - Allows for evolution of the product as it is being worked on
  - Work is performed in short planned phases (i.e. every 2-3 weeks)
    - The goal of each iteration to provide a working product.
    - Tests must pass at the end of each cycle
    - Stakeholders get updated at the end of each cycle and may provide feedback
      - Changes may be incorporated from feedback in the next phase
  - Downside: The openness to change may result in fluxuating timelines
- Waterfall:
  - Development doesn't start until all planning is done
  - Sequential and Linear development cycle
  - Longer development cycles between producing updates
  - Inflexible -- Very hard to make changes in the middle of a dev cycle
  - Lacks visibility. I.e. fewer check-ins / updates
  - Upside: Since scope is known ahead of time:
    - Timelines tend to be more stable
    - Progress is more easily measured against an end goal
- Lean
  - Focused on producing an MVP as quickly as possible.
  - Can "fail fast" from a business perspective.
    - i.e. Are you building the right product? Allows you to abandon early if you aren't.
  - Reduce project delivery time and cost while delivering to a specific set of minimum requirements
  - Downside: Little effort and time is spent considering the future needs of the application.

### 1.5 Explain the benefits of organizing code into methods/ functions, classes, and modules

The primary purposes for organizing code are reusability, testability, and refactoring.

Consider this piece of code:

``` python
people = [
    {
        "firstName": "John",
        "lastName": "Carpenter",
        "occupation": "Director"
    },
    {
        "firstName": "Madeleine",
        "lastName": "L'Engle",
        "occupation": "Author"
    }
]

print(f'{people[0]["firstName"]} {people[0]["lastName"]}')
print(f'{people[1]["firstName"]} {people[1]["lastName"]}')
```

So we have an array of "anonymous" objects. i.e. the structure is unclear, you won't get intellisense when working on it.

We'll improve this a little at a time, starting with methods/functions:

``` python
def getFullName(person):
    return f'{person["firstName"]} {person["lastName"]}'

people = [
    {
        "firstName": "John",
        "lastName": "Carpenter",
        "occupation": "Director"
    },
    {
        "firstName": "Madeleine",
        "lastName": "L'Engle",
        "occupation": "Author"
    }
]

print(getFullName(people[0]))
print(getFullName(people[1]))
```

This time instead of repeating the same structure of code to produce a full name, we extracted it into a function. A piece of code that we can reuse over and over again to produce the same result. And if something needs to be changed we only need to change it in one place. Not 2 or more.

But there is still some improvement to be had here. For instance, what happens if we sent in an object that didn't have a firstName and a lastName? It would break. That's an extremely fragile piece of code. Lets see what a class can do for us here:

``` python

class Person:
  def __init__(self, firstName, lastName, occupation):
    self.firstName = firstName
    self.lastName = lastName
    self.occupation = occupation

  def getFullName(self):
      return f'{self.firstName} {self.lastName}'

people = []
people.append(Person('John', 'Carpenter', 'Director'))
people.append(Person('Madeleine', "L'Engle", 'Author'))

print(people[0].getFullName())
print(people[1].getFullName())

```

Ahhh... Now we're getting somewhere. We have a class named person with a constructor that takes a firstName, a lastName, and an occupation. So if we were to do people[0] and put a period, we would get intellisense showing us options available on this type of object.

Then we moved our getFullName() function inside that class. So every person object now has a method to get the fullname of that person. MUCH cleaner! And reuseable! And testable! We only have to test whether the Person class and its methods work correctly. If they don't we can fix it all in one place.

As far as modules go, they're more for separating classes, functions, and values into groups that make sense. i.e. you might make one module that handles logging, so you can import it into any other python file you're using. Or you might create a module that has all the methods for talking to an API. And then you could have a main python file that imports the API module and the logging module. And you'd know exactly what each is for because the logic is separated based on responsibility.

### 1.6 Identify the advantages of common design patterns (MVC and Observer)

#### MVC or Model-View-Controller

This is a pattern used in many languages -- it is for the use of applications with a GUI.

- Model: Your data structures and potentially business logic. Though it may just be used to get data from a database.
- View: What your user sees
- Controller: The glue between your model and your view. It gets data from the model to be shown to the user, and sends data from the view to the model layer to be saved to databases etc.

One of the benefits of this pattern is the separation of responsibilities into logical layers. This makes it easier to organize, understand, and test your code.

#### Observer Pattern

The simplest way I can think of to describe the observer pattern is: What if you had an editable table of data on a screen, and to the left of that data you had a chart based on that data. Every time you update the data in the table you'd expect the graph to change right? That's exactly the kind of problem that the Observer pattern is a solution for.

It quite literally "observes" an object that other objects rely on. And when that object's state changes, the observer says "Hey, you over there, this object you rely on just changed." Then those other objects can be update based on the object's new state.

#### Why Patterns

People have been programming for a long time. And it's very common for programmers to run into the same problems that other people have run into. It's the reason things like StackOverflow exist. Patterns were created to help remedy the most common "big" problems. So it's beneficial to learn common design patterns so that you can use proven solutions instead of banging your head against the wall trying to solve design issues on your own.

### 1.8 Utilize common version control operations with Git

#### Git Clone

``` git
git clone [url]
```

- **Example:** ```git clone git@github.com:FooBartn/WxTeamsSharp.git```
- **Purpose:** retrieve an entire repository from a hosted location via URL

#### Git Add/Remove

``` git
git add [file]
```

- **Example**: ```git add parsejson.py```
- **Purpose**: Add a file as it looks now to your next commit. Also known as Staging. Using ```git add .``` will add/stage all new and modified files. ```git add -A``` will stage all new, modified, and deleted files

#### Git Commit

``` git
git commit -m “[descriptive message]”
```

- **Purpose**: Commit your staged content as a new commit snapshot

#### Git Push / Pull

``` git
git push [alias] [branch]
```

- **Example**: ```git push origin master``` <-- Push the local master branch to remote named origin
- **Purpose**: Transmit local branch commits to the remote repository branch

``` git
git pull
```

- **Purpose**: Fetch and merge any commits from the tracking remote branch

#### Git Branch

``` git
git branch
```

- **Purpose**: List your branches. a * will appear next to the currently active branch

``` git
git branch [branch-name]
```

- **Purpose**: Create a new branch at the current commit

``` git
git checkout [branch]
```

- **Purpose**: Switch to another branch and check it out into your working directory

``` git
git checkout -b [branch]
```

- **Purpose**: Create another branch, switch to it, and check it out into your working directory

### Git Merge and handling conflicts

``` git
git merge [branch]
```

- **Purpose**: Merge the specified branch’s history into the current one

``` git
git status
```

- **Purpose**: One of the primary purposes is to list out any any files affected by merge conflicts.

> For more detail on resolving merge conflicts see [Git-Tower: Merge Conflicts](https://www.git-tower.com/learn/git/ebook/en/command-line/advanced-topics/merge-conflicts)

#### Git Diff

``` git
git diff
```

- **Purpose**: Diff of what is changed but not staged

``` git
git diff --staged
```

- **Purpose**: Diff of what is staged but not yet commited

> Note: Check out the [Atlassian: Git-Diff Tutorial](https://www.atlassian.com/git/tutorials/saving-changes/git-diff) to see how to read and understand the output from git diff!

## 2.0 Software Development and Design (20%)

### Construct a REST API request to accomplish a task given API documentation

#### HTTP Verbs / Actions

- **POST**: Create
- **GET**: Read
- **PUT**: Update
- **PATCH**: Update
- **DELETE**: Delete

#### Reading API Documentation

Let's take a look at one of my favorite little APIs: [ICanHazDadJoke API](https://icanhazdadjoke.com/api)

The documentation states that I do not need Authentication, and that it will adjust its return output based on the Accept headers we send in. It will return text/html by default.

The API states that the main endpoint is this:

``` na
GET https://icanhazdadjoke.com/ fetch a random dad joke.
```

Notice that the first part of this says GET. API documentation will let you know the correct verb to use to interact with that endpoint.

So lets try to get a Dad joke using Python's requests library!

``` python
import requests

response = requests.get('https://icanhazdadjoke.com/')
joke = response.content
print(joke)
```

Oh... if we do that, we get back a whole HTML page! Remember, the default return is text/html. This is so you can browse to 'https://icanhazdadjoke.com' and you will see a nicely formatted joke in your browser.

Alright, so what to do to get JSON? We need to change our Accept header to tell the API what kind of response we want back.

``` python

import requests

url = 'https://icanhazdadjoke.com/'
headers = {
    'Accept': 'application/json'
}
response = requests.get(url, headers=headers)
joke = response.json()
print(joke)

```

Ah! Much better. Now we get json back that looks something like this:

``` json
{
    "id": "pzXvHl3EYg",
    "joke": "Why don’t seagulls fly over the bay? Because then they’d be bay-gulls!",
    "status": 200
}
```

And if you run it again you'll get a different joke. But what if I wanted a specific joke?

There is another endpoint that says:

``` na
GET https://icanhazdadjoke.com/j/<joke_id> fetch a specific dad joke.
```

See in the json above, each joke also comes with an ID. You can use this to refer to a specific joke.

``` python

import requests

url = 'https://icanhazdadjoke.com/j/ukOCYoWDQuc'
headers = {
    'Accept': 'application/json'
}
response = requests.get(url, headers=headers)
joke = response.json()
print(joke)

```

This particular ID should result in the joke: My sea sickness comes in waves. I love it.

There's even a search endpoint. And this one is a good one to check out, because it uses query parameters. Query parameters are those things you see after the ? in a URL.

The endpoint is this:

``` na
GET https://icanhazdadjoke.com/search - search for dad jokes.
```

And the API states that the endpoint accepts these query parameters:

``` na
page - which page of results to fetch (default: 1)
limit - number of results to return per page (default: 20) (max: 30)
term - search term to use (default: list all jokes)
```

Hmm.. Let's try searching for a joke about dogs. But we only want it to return 5 results per page.

``` python
import requests

url = 'https://icanhazdadjoke.com/search'
headers = {
    'Accept': 'application/json'
}
params = {
    'term': 'dog',
    'limit': '5'
}
response = requests.get(url, headers=headers, params=params)
joke = response.json()
print(joke)
```

And that returned:

``` json
{
    "current_page": 1,
    "limit": 5,
    "next_page": 2,
    "previous_page": 1,
    "results": [
        {
            "id": "EBQfiyXD5ob", 
            "joke": "what do you call a dog that can do magic tricks? a labracadabrador"
        },
        {
            "id": "DIeaUDlbUDd",
            "joke": "“My Dog has no nose.” “How does he smell?” “Awful”"
        }
        // and more jokes..
    ],
    "search_term": "dog",
    "status": 200,
    "total_jokes": 12,
    "total_pages": 2
}
```

Whoa. That's way more than just jokes. We've got current_page, limit, next_page, previous_page, search_term, total_jokes, total_pages... 

Remember there was a 3rd query parameter available for the endpoint called page? If we called this same endpoint but used the query paramter ```'page': 2``` we'd get the second page of results. Try it out on your own!

> Another common command for making HTTP requests is cURL. Its syntax looks like this: curl -X GET https://icanhazdadjoke.com/. For more information about cURL check out [Everything cURL](https://ec.haxx.se/). Or, if you just want a quick cheat sheet check out: [devhints: curl](https://devhints.io/curl)

### Describe common usage patterns related to webhooks

APIs are wonderful. You ask them for information, they give it to you. But what if that information changes all the time, and your application relies on up-to-date data?

Well, you just query it more often right? Maybe. But most APIs have a rate-limit. After a certain number of requests per second or minute they tell you "Whoa, slow down." They do this by denying your request and usually sending a header along to tell you how long your timeout is for.

"But I need the new data as soon as it changes!!", you say. In comes the webhook to save the day, IF the API you're working with has made them available.

Instead of relying on you asking for more information, you register a link to an API of your own that is capable of receiving a POST request. This tells the API you need information from -- "Hey, everytime this data changes **YOU** send **ME** an update!". When the data changes, the API sees your registered webhook and sends a POST request to your API with the new data. Voila! No more sending GET requests every 5 seconds to the API!

### Identify the constraints when consuming APIs

I'm not entirely sure what they're looking for here, but my guess is things like:

- How often you can make a request of the API before it gives you back a 429 TooManyRequests response
- What type of authentication is required to use the API
- How many requests you can make in any given time period (day,week,month) before you hit your limit

### Explain common HTTP response codes associated with REST APIs

- **200** OK: All looks good
- **201** Created: New resource created
- **400** Bad Request: Request was invalid
- **401** Unauthorized: Authentication missing or incorrect
- **403** Forbidden Request: was understood but not allowed
- **404** Not Found: Resource not found
- **500** Internal Server Error: Something wrong with the server
- **503** Service Unavailable: Server is unable to complete request

### Troubleshoot a problem given the HTTP response code, request and API documentation

We'll take a look at a slightly more "professional" API than ICanHazDadJoke this time around. Let's check out the [Webex Teams API: Basics](https://developer.webex.com/docs/api/basics)

Scroll down to the API Errors section. You should see a table like this:

| Code | Status       | Description                                                                                               |
|------|--------------|-----------------------------------------------------------------------------------------------------------|
| 200  | OK           | Successful Request                                                                                        |
| 204  | No Content   | Successful request without body content                                                                   |
| 400  | Bad Request  | The request was invalid or cannot be otherwise served. An accompanying error message will explain further |
| 401  | Unauthorized | Authentication credentials were missing or incorrect                                                      |

Most APIs will have information like this to let you know what went wrong and how to handle it.

> Note: If you see an error message in the 50x range, you're all outta luck. Those are server issues and the only thing you can do is send a message to the owners of that API and hope they fix it.

### Identify the parts of an HTTP response (response code, headers, body)

![Cisco REST Infographic](https://developer.cisco.com/learning/posts/files/what-are-rest-apis/assets/images/request-response.png) - From [Cisco DevNet: What are REST APIs?](https://developer.cisco.com/learning/lab/what-are-rest-apis/step/1)

### Utilize common API authentication mechanisms: basic, custom token, and API keys

**Most** APIs have some sort of authentication, even if it's just to keep track of who is doing what so abusers can be caught. Not many are as open as the ICanHazDadJoke API, where its only request is that if you're using it from an application of your own (as opposed to just playing around with it) you put the application name in the User-Agent header so they can tell where it's coming from.

The most common form of this that I see is an API Key. You register for a service, agree to all the terms, and they give you an API key that is strictly for your account. You then put that in the header of every request you make. I.e. if ICanHazDadJoke did require an API key, the request might look like this:

``` python
import requests

url = 'https://icanhazdadjoke.com/search'
headers = {
    'Accept': 'application/json',
    'Authorization': 'Bearer 235235235xyz'
}
params = {
    'term': 'dog',
    'limit': '5'
}
response = requests.get(url, headers=headers, params=params)
joke = response.json()
print(joke)
```

You also have Basic Authentication, Digest Authentication, and OAuth (1 and 2) Authentication.

Basic authentication is the simplest. And the least secure. You send the username and password over plain text. So if it's done, it really should be done using Transport Layer Security (TLS) -- i.e. it should use https, not http. In Python it looks like this:

``` python
requests.get('https://api.github.com/user', auth=HTTPBasicAuth('user', 'pass'))
```

Digest Authentication is very similar to Basic, except that it encrypts the data with a hash instead of sending it in plain text. So even though TLS is always preferred, in this case it's not as risky.

``` python
requests.get('https://api.github.com/user', auth=HTTPDigestAuth('user', 'pass'))
```

OAuth is the most common form of authentication if you're authenticating on behalf of other users. Think of an app that you've signed up for that was 3rd party for a large service. Or when you use a Facebook account to authenticate with a non-Facebook service. When you sign up, you've probably seen a page that says "This application would like to access your account with these privileges: Read your username, Read your email, etc". That's the Oauth workflow at work!

It basically goes like this:
Your app has a Client Id and probably a Client Secret, along with a redirect URI (where to go back to once a user has authenticated). You also specify a scope of rights your app should have.

Whenever a user wants to authenticate, you send them to the API services Oauth url with the parameters we talked about in the paragraph above. They will get a message like "FooBarton's app would like to access your profile". If they click accept, their service uses the redirect URI to send them back to your page along with a special code. Your app then uses that code to make a **second** request to a different endpoint to get a token with that code. That token is then good for a set amount of time before it must be renewed or the user must re-authenticate.

To see more about how to use Oauth with Python, check out [requests-oauthlib](https://requests-oauthlib.readthedocs.io/en/latest/oauth2_workflow.html)

### Compare common API styles (REST, RPC, synchronous, and asynchronous)


#### REST and RPC

REST, which has become the most popular form of communication with APIs has rules and standards that help making talking to any API a near-ubiquitous experience. If you've done a GET with one REST API that returns JSON, you've done them all. This allows an API to last for a very long time without needing to be changed.

Some of the restrictions REST API developers are supposed to follow:

- REST must be stateless: not persisting sessions between requests.
- Responses should declare cacheablility: helps your API scale if clients respect the rules.
- REST focuses on uniformity: if you’re using HTTP you should utilize HTTP features whenever possible, instead of inventing conventions.

> See all 6 architectural constraints described [here](https://restfulapi.net/rest-architectural-constraints/)

REST APIs will commonly use all of the available HTTP verbs for a given endpoint to allow you to do CRUD-based (Create, Read, Update, Delete) operations. GET, POST, PUT, DELETE, PATCH

And these can take data payloads in the form of JSON, or query parameters like we used previously to modify the request

RPC stands for "Remote Procedure Call". Think of it like calling a python function. An RPC API is a list of functions available to call where you give it the name of the function and the argument/parameter for it. And these APIs typically only serve GET or POST requests.

One of the big differences is that, with RPC, *everything* is a separate function.

Say you have a person directory. If you want to get people by first name you might do:

```
POST /getPersonByFirstName
Content-Type: application/json

{"firstName": "Elliot"}
```

But if you wanted to get people by last name:

```
POST /getPersonByLastName
Content-Type: application/json

{"lastName": "Baker"}
```

Two different endpoints to get the same type of object, just by a different value. In comparison, you'd probably see this for a REST API

```
GET /getPerson?lastName=Baker
GET /getPerson?firstName=Elliot
```

Same API endpoint, just with a different query parameter.

In this regard, REST is usually seen as the best way to deal with CRUD, while RPC is more action based.

Slack uses an RPC api because things like kick, ban, etc don't really fit well in the GET, POST, PUT, DELETE, PATCH semantics. In REST you expect that GET is for reading, POST is for adding new things, etc. Where do kick and ban fit in? You could make them, for sure. But when you start shoehorning REST with RPC-style actions, it gets a little confusing. Not just for developers, but for users who expect your REST API to act just like a REST API.

#### Synchronous and Asynchronous

The easiest way to think of synchronous and asynchronous is that synchronous is synonomous with the wait symbol (hourlgass, spinner, etc). You know when you go to a website and it sits there with a spinner in the browser tab? That's a synchronous operation. You have to wait for it, and while you're waiting the app (or webpage) is stalled.

An asynchronous operation is one that you do not have to wait for directly. Let's say, for instance, that you make an RPC call to configure and deploy a new server. And that operation will take 10 minutes. Do you want your app to make that call and then sit there locked up for 10 minutes waiting for it to finish? (synch)

Or do you want to make that call and let a webhook notify you whenever it's done. And in the meantime your user is still able to use the app to look at other things. (async)

Or maybe you want to GET a list of all your transactions for the last 20 years. And this will take 30 minutes to complete. You could send a request to the API with your email address and it could send you an email with a link to your download when it's ready. But you didn't have to sit there waiting 30 minutes. You can go do other things. (async)

As with REST and RPC, Synchronous and Asynchronous APIs have their uses!

### Construct a Python script that calls a REST API using the requests library

We've done a few of these ^___^

## 3.0 Cisco Platforms and Development (15%)

### 3.1 Construct a Python script that uses a Cisco SDK given SDK documentation

A quick python script for getting a list of devices from the Cisco Sandbox DNAC

``` python
from dnacentersdk import api # pip install dnacentersdk first!

dnac = api.DNACenterAPI(username="devnetuser",
                        password="Cisco123!",
                        base_url="https://sandboxdnac2.cisco.com:443",
                        version='1.3.0',
                        verify=True)

devices = dnac.devices.get_device_list()

for device in devices.response:
    print(device.managementIpAddress)

```

There's a bit of a hidden doc that shows all the commands available with this SDK. [Check it out](https://dnacentersdk.readthedocs.io/en/latest/api/api_structure_table_v1_3_0.html)

### 3.2 Describe the capabilities of Cisco network management platforms and APIs (Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, and NSO)

List of general dev documents available [here](https://developer.cisco.com/docs/)

#### Meraki

The Meraki Dashboard API is an interface for software to interact directly with the Meraki cloud platform and Meraki managed devices. The API contains a set of tools known as endpoints for building software and applications that communicate with the Meraki Dashboard for use cases such as provisioning, bulk configuration changes, monitoring, and role-based access controls. The Dashboard API is a modern, RESTful API using HTTPS requests to a URL and JSON as a human-readable format. The Dashboard API is an open-ended tool can be used for many purposes, and here are some examples of how it is used today by Meraki customers:
- Add new organizations, admins, networks, devices, VLANs, SSIDs
- Provision thousands of new sites in minutes with an automation script
- Automatically onboard and off-board new employees' teleworker device
- Build your own dashboard for store managers, field techs, or unique use cases

Also leverage the Location Service APIs to discover real-time location statistics to improve customer engagement and loyalty across sites.

\- Read more @ [Meraki Dashboard API Documentation](https://documentation.meraki.com/zGeneral_Administration/Other_Topics/The_Cisco_Meraki_Dashboard_API)

#### Cisco DNA Center

Intent API (Northbound)
The Intent API is a Northbound REST API that exposes specific capabilities of the Cisco DNA Center platform. The Intent API provides policy-based abstraction of business intent, allowing focus on an outcome rather than struggling with individual mechanisms steps. The RESTful Cisco DNA Center Intent API uses HTTPS verbs (GET, POST, PUT, and DELETE) with JSON structures to discover and control the network. For more information, see Intent API. The Intent API is grouped, hierarchically into functional 'domains' and 'subdomains' of service.

\- Read more @ [Cisco DNA Center Platform](https://developer.cisco.com/docs/dna-center/#!cisco-dna-center-platform-overview/intent-api-northbound)

#### ACI

Cisco ACI Programmability with Object-Oriented Data Model and REST APIs
Cisco has taken a foundational approach to building a programmable network infrastructure with Cisco ACI. The ACI infrastructure operates as a single system at the fabric level, controlled by the centralized Cisco Application Policy Infrastructure Controller (APIC). With this approach, the data center network as a whole is tied together cohesively and treated as an intelligent transport system for the applications that support business. On network devices that are part of this fabric, the operating systems have been written to support this system view and provide an architecture for programmability at the foundation.

Instead of opening up a subset of the network functionality through programmatic interfaces, like previous Software Defined Networking (SDN) solutions, the entire ACI infrastructure is opened up for programmatic access. This is achieved by providing access to Cisco ACI object model. The ACI object model represents the complete configuration and runtime state of every single software and hardware component in the entire infrastructure. The object model is made available through standard REST API interfaces, making it easy to access and manipulate the configuration and runtime state of the system.

\- Read more @ [ACI Programmability](https://developer.cisco.com/docs/aci/#!introduction/aci-programmability)

#### Cisco SD-WAN

Legacy networking technology has become increasingly expensive and complex, and it cannot scale to meet the needs of today's multisite enterprises. The Cisco SD-WAN Secure Extensible Network (SEN), which is based on time-tested and proven elements of networking, offers an elegant, software-based solution that reduces the costs of running enterprise networks and provides straightforward tools to simplify the provisioning and management of large and complex networks that are distributed across multiple locations and geographies. Built in to the Cisco SD-WAN SEN are inherent authentication and security processes that ensure the safety and privacy of the network and its data traffic.

The Cisco SD-WAN SEN represents an evolution of networking from an older, hardware-based model to a secure, software-based, virtual IP fabric. The Cisco SD-WAN fabric, also called an overlay network, forms a software overlay that runs over standard network transport services, including the public Internet, MPLS, and broadband. The overlay network also supports next-generation software services, thereby accelerating your shift to cloud networking.

\- Read more @ [Cisco SD-WAN Guide](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/sdwan-xe-gs-book/system-overview.html)

#### NSO

Cisco® Network Services Orchestrator (NSO) enabled by Tail-f® is an industry-leading orchestration platform for hybrid networks. It provides comprehensive lifecycle service automation to enable you to design and deliver high-quality services faster and more easily.

The network is a foundation for revenue generation. Therefore, service providers must implement network orchestration to simplify the entire lifecycle management for services. For today’s virtualized networks, this means transparent orchestration that spans multiple domains in your network and includes network functions virtualization (NFV) and software-defined networking (SDN) as well as your traditional physical network and all its components

NSO is a model driven (YANG) platform for automating your network orchestration. It supports multi-vendor networks through a rich variety of Network Element Drivers (NEDs).

Read more @ [NSO Fundamentals](https://developer.cisco.com/docs/nso/)

### 3.3 Describe the capabilities of Cisco compute management platforms and APIs (UCS Manager, UCS Director, and Intersight)

#### UCS Manager

The Cisco Unified Compute System is the realization of Programmable Infrastructure. By providing an open full-coverage API, UCS enables infrastructure developers to manage compute, network, and storage resources with simplicity. The UCS Unified APIs are available in UCS Manager, UCS Central, and UCS IMC. Because the Unified APIs are, well ... unified, learning one API means you've learned the others. The systems or endpoints you develop for are represented as hierarchical Object Models referred to as the Management Information Tree.

Read more @ [Cisco UCS Programmability](https://developer.cisco.com/docs/ucs-dev-center/#!cicso-ucs-programmabilty/cisco-ucs-programmability)

#### UCS Director

Cisco UCS Director automates, orchestrates, and manages Cisco and third-party hardware. You can manage network components and services. And allow data center staff to focus on services that set you apart from the competition.

Cisco UCS Director can:

- Increase efficiency by automating multiple automated tasks
- Deliver error-free resources within minutes
- Simplify data center operations with centralized management
- Increase productivity with on-demand ordering of IT resources and services

Read more @ [Cisco UCS Director](https://developer.cisco.com/docs/ucs-dev-center-ucs-director/#!choosing-an-api)

#### Intersight

Cisco Intersight™ provides intelligent cloud-based infrastructure management for the Cisco Unified Computing System™ (Cisco UCS®) and Cisco HyperFlex® platforms. Intersight offers an intelligent level of management that enables IT organizations to analyze, simplify, and automate their environments.

Read more @ [Cisco Intersight](https://developer.cisco.com/site/intersight/) and [Cisco Intersight API](https://intersight.com/apidocs/introduction/overview/)

### 3.4 Describe the capabilities of Cisco collaboration platforms and APIs (Webex Teams, Webex devices, Cisco Unified Communication Manager including AXL and UDS interfaces, and Finesse)

#### Webex

- Webex Meetings is a powerful conferencing solution that lets you connect with anyone, anywhere, in real time. By combining video, audio and content sharing, Webex Meetings creates an effective conferencing environment, leading to more satisfying meetings and increased productivity.

- Webex Teams makes staying in sync with your teammates and customers easy. Conversations in Webex Teams take place in virtual meeting rooms called spaces. Some spaces live for a few hours while others become permanent fixtures of your team's workflow with titles like Daily Standup or Build Status. Webex Teams allows conversations to flow seamlessly between messages, video calls, and real-time whiteboarding sessions. No other solution brings together so many facets of collaboration into a single, unified platform.

- Webex Devices help you build the best workplace for your team. Connect your work to the meeting room and beyond. With Webex Devices you can join Webex Meetings or participate in interactive whiteboarding sessions with Webex Teams users. The possibilities are endless.

Read more @ [Cisco Webex for Developers](https://developer.webex.com/docs/platform-introduction)

#### Cisco Unified Communication Manager

Cisco® Unified Communications Manager is the heart of Cisco collaboration services, enabling session and call control for video, voice, messaging, mobility, instant messaging, and presence. Read more @ [Cisco UCM Data Sheet](https://www.cisco.com/c/en/us/products/collateral/unified-communications/unified-communications-manager-callmanager/datasheet-c78-741428.html)

- AXL: The Administrative XML Web Service (AXL) is a XML/SOAP based interface that provides a mechanism for inserting, retrieving, updating and removing data from the Unified Communication configuration database. Developers can use AXL and the provided WSDL to Create, Read, Update, and Delete objects such as gateways, users, devices, route-patterns and much more. Read more @ [Administrative AXL](https://developer.cisco.com/docs/axl/)

- UDS: The User Data Services API is a REST-based set of operations that provide authenticated access to user resources and entities, such as user's devices, subscribed services, and speed dials, from the Unified Communications configuration database. Read more @ [User Data Services API](https://developer.cisco.com/docs/user-data-services-api-reference/#!user-data-services-uds-developer-guide)

#### Finesse

Cisco Finesse is a next-generation agent and supervisor desktop designed to provide a collaborative experience for the various communities that interact with your customer service organization. It also helps improve the customer experience while offering a user-centric design to enhance customer care representative satisfaction.

For IT professionals, Cisco Finesse offers transparent integration with the Cisco Collaboration portfolio. It is standards compliant and offers low cost customization of the agent and supervisor desktops.

Cisco Finesse provides REST APIs for performing agent and supervisor actions programmatically. These REST APIs are easy to use, modeled after HTTP, and works in thick and thin client integrations. The Finesse Developer Guide explains the details for each of the API's, but here is a high-level description of the API functionality:

Read more @ [Finesse](https://developer.cisco.com/docs/finesse/)

### 3.5 Describe the capabilities of Cisco security platforms and APIs (Firepower, Umbrella, AMP, ISE, and ThreatGrid)

#### Firepower

Firepower offers powerful, documented integration points in context-rich APIs which allow the exchange of network and endpoint security events, data and host information.

- Firepower Management Center (FMC): Context-rich APIs for exchange of network and endpoint security event data and host information.

- Firepower Threat Defense (FTD): A Rest API that automates configuration management and execution of operational tasks on Cisco Firepower Threat Defense (FTD) devices.

Read more @ [Cisco Firepower](https://developer.cisco.com/firepower/)

#### Umbrella

Cisco Umbrella offers flexible cloud-delivered security when and how you need it. It combines multiple security functions into one solution, so you can enrich your incident response data and easily extend protection to devices and locations anywhere. Because Umbrella is delivered from the cloud, it is the easiest way to protect your users everywhere in minutes.

Blocks access to malicious domains, URLs, IPs, and files using the Enforcement API or pull threat intelligence programmatically with the Investigate API.

The Cisco Umbrella Enforcement API is designed to give technology partners the ability to send security events from their platform/service/appliance within a mutual customer’s environment to the Umbrella cloud for enforcement. You may also list the domains and delete individual domains from the list. All received events will be segmented by the mutual customer and used for future enforcement.

Read more @ [Umbrella Enforcement API](https://docs.umbrella.com/enforcement-api/reference/), [Umbrella Investigate API](https://docs.umbrella.com/investigate-api/docs), and [Umbrella API](https://docs.umbrella.com/umbrella-api/docs)

#### AMP

Cisco Advanced Malware Protection (AMP) for Endpoints offers cloud-delivered next-generation antivirus, endpoint protection platform (EPP), and advanced endpoint detection and response (EDR). Protect your Windows, Mac, Linux, Android, and iOS devices through a public or private cloud deployment with API access.

Read more @ [Cisco AMP for Endpoints API](https://api-docs.amp.cisco.com/)

#### ISE

Cisco Identity Services Engine (ISE) allows you to provide highly secure network access to users and devices. It helps you gain visibility into what is happening in your network, such as who is connected, which applications are installed and running, and much more. It also shares vital contextual data, such as user and device identities, threats, and vulnerabilities with integrated solutions from Cisco technology partners, so you can identify, contain, and remediate threats faster.

Enforce compliance, heighten infrastructure security, and streamline user network access operations.

Read more @ [Cisco ISE](https://www.cisco.com/c/en/us/products/security/identity-services-engine/index.html) and [Cisco ISE API](https://developer.cisco.com/docs/identity-services-engine/)

#### ThreatGrid

Threat Grid combines advanced sandboxing with threat intelligence into one unified solution to protect organizations from malware. With a robust, context-rich malware knowledge base, you will understand what malware is doing, or attempting to do, how large a threat it poses, and how to defend against it. 

Review and analyze potential threats or behavior indicators of malware activity.

Threat Grid’s REST APIs allow users to submit samples for analysis as part of an investigation or research. The indicators and data from the analysis are indexed and searchable making it easy to use for triage, hunting, or threat intelligence. This allows analysts to query the data and find, for example, additional pieces of infrastructure that an attacker is using in a campaign, methods of persistence a family of malware uses, host or network indicators that follow similar naming conventions, host indicators that families of malware leave behind, etc.

Read more @ [Threat Grid](https://www.cisco.com/c/en/us/products/security/threat-grid/index.html) and [Threat Grid API](https://developer.cisco.com/threat-grid/)


### 3.6 Describe the device level APIs and dynamic interfaces for IOS XE and NX-OS

#### IOS XE

**NETCONF** provides a means to programmatically interact with a device – in a model-based, machine-consumable, easy to understand and standards-based way. NETCONF is defined by >RFC-6241 and is based on the YANG modeling language.
- NETCONF listens on any IP address assigned to the system. When enabled, NETCONF runs on port 830 and uses SSH for transport. SSH is enabled automatically when the NETCONF feature is enabled.
- NETCONF connections should be authenticated using AAA credentials. RADIUS, TACACS+ or local users defined with privilege level 15 access are allowed.

**RESTCONF** is a YANG-based, REST-like interface for configuring and operating network devices. RESTCONF is defined by an RFC 8040. The structure of data exchanged using the RESTCONF interface is defined (in advance) using YANG models. Management systems using YANG can directly access managed resources in a single operation. Familiar REST operations are included with RESTCONF, like GET, POST, PUT, PATCH, etc.
- RESTCONF listens on any IP address assigned to the system. RESTCONF operates on port 443 when enabled, and uses HTTPS for transport.
- RESTCONF connections should be authenticated using AAA credentials. RADIUS, TACACS+ or local users defined with privilege level 15 access are allowed.

#### NX-OS

NETCONF and RESTCONF can both be used with NX-OS, but it brings an even better option to the table: NX-API

**NX-API REST** brings Model Driven Programmability (MDP) to standalone (non-APIC-based fabric) Nexus family switches. In model-driven architectures, software maintains a complete, explicit representation of the administrative and operational state of the system (the model) and performs actions only as side-effects of mutations of model entities.

This model is a formal specification understood by network planners and developers that is rendered into a running system. Furthermore, this object model is made available through standard Representational State Transfer (REST) interfaces, making it easier to access and manipulate the object model, and hence, the configuration and runtime state of the system.

For 9000 Series network devices, there is also a Python SDK available.

Read more @ [NETCONF for IOS XE](https://developer.cisco.com/docs/ios-xe/#!enabling-netconf-on-ios-xe), [RESTCONF for IOS XE](https://developer.cisco.com/docs/ios-xe/#!enabling-restconf-on-ios-xe/http-https), [NX-API REST](https://developer.cisco.com/site/cisco-nexus-nx-api-references/), and [Python SDK for 9000 Series](https://developer.cisco.com/docs/nx-os/#!cisco-nexus-9000-series-python-sdk-user-guide-and-api-reference) 


### 3.7 Identify the appropriate DevNet resource for a given scenario (Sandbox, Code Exchange, support, forums, Learning Labs, and API documentation)

[Sandbox](https://developer.cisco.com/site/sandbox/): Engineers, developers, network admins, architects, or anyone else who desires to develop/test against Cisco’s APIs, controllers, network gear, collaboration suite and more for free!
- Run your code against live infrastructure 24x7
- Free access to a variety of labs; choose from virtualized environments, simulators, and real hardware
- Select always on labs or reservation-based labs based on your need

[Code Exchange](https://developer.cisco.com/exchange/): A single, curated, online catalog for Cisco customers to find code, products, and services offered from across the Cisco ecosystem.
- Online, curated set of source code repositories related to all Cisco technology areas on public GitHub.
- Hundreds of code repositories, created and maintained by Cisco engineering teams, community contributors, ecosystem partners, technology and open source communities, and individual developers.

[Support](https://developer.cisco.com/site/support/): Need help on DevNet? Have a technology-specific question? We provide different ways of support to get your questions answered!
- Knowledge Base
- Community Forum
- Chat with DevNet
- Case-Based Ticket

[Forums](https://community.cisco.com/t5/for-developers/ct-p/4409j-developer-home): Get connected with other Cisco technology experts in DevNet.

[Learning Labs](https://developer.cisco.com/learning/): Dive deeper into Cisco and Cisco Partner technologies with DevNet Learning Labs, including Enterprise Networks, Data Center, Collaboration, Cloud, SDN, and IoT. Whether you're getting started or need a programming refresher, the Learning Labs get you started with tutorials covering REST APIs, Python, JavaScript, and other engineering technologies and concepts.

### 3.8 Apply concepts of model driven programmability (YANG, RESTCONF, and NETCONF) in a Cisco environment

Check out these learning labs:
- [Intro NETCONF](https://developer.cisco.com/learning/lab/intro-netconf/step/1)
- [Intro RESTCONF](https://developer.cisco.com/learning/labs/intro-restconf/step/1)

### 3.9 Construct code to perform a specific operation based on a set of requirements and given API reference documentation such as these:

I'm going to leave these for you to try out on your own!

#### 3.9.a Obtain a list of network devices by using Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, or NSO

#### 3.9.b Manage spaces, participants, and messages in Webex Teams

#### 3.9.c Obtain a list of clients / hosts seen on a network using Meraki or Cisco DNA Center

## 4.0 Application Deployment and Security (15%)

### 4.1 Describe benefits of edge computing

> Edge computing is a distributed computing paradigm which brings computation and data storage closer to the location where it is needed, to improve response times and save bandwidth. - [Wikipedia: Edge Computing](https://en.wikipedia.org/wiki/Edge_computing)

If a company website were hosted in Japan, for instance, people in the U.S. might experience delays when using it. But in an edge computing model, the company would have a server in the U.S. that it would direct U.S. people to instead. This is most commonly used in Content Delivery Networks where data is cached at the edge so users can download it faster.

### 4.2 Identify attributes of different application deployment models (private cloud, public cloud, hybrid cloud, and edge)

**Private Cloud**: Private Clouds typically use virtualization on physical hardware under direct control of a customer. They get a similar experience to Public Clouds like Amazon Web Services or Microsoft Azure, but keep all of their data on-premises. This can be for a number of reasons such as security, budgets, compliance, etc.

**Public Cloud**: A Public Cloud is a third-party resource for purchasing compute, database, and other services that is publically available over the internet. One of the primary reasons made for using public clouds is the on-demand nature. Allowing a company or individual to pay for only what they need -- or get rid of anything they don't -- whenever they need to.

**Hybrid Cloud**: A mix of both on-premises, private and public cloud infrastructure. A common use for this is cloud bursting, where a customer is able to get resources from the public cloud on demand when their on-premises resources have reached their limit. i.e. during times of high stress such as Black Friday sales. When the load returns to normal the public cloud resources are removed.

**Edge**: Used to bring computing power and storage close to the source of the request

### 4.3 Identify the attributes of these application deployment types

#### 4.3.a Virtual machines

A virtualized operating system sitting on top of a hypervisor. Deployed in public and private clouds such as Microsoft Azure, AWS, and VMware. Many VMs are able to share the same hardware, making more efficient use of physical resources.

#### 4.3.b Bare metal

Installing an operating system directly on the hardware instead of using virtualization; the previously-traditional way that servers were provisioned. Nowadays this is primarily for legacy applications that cannot be virtualized, or for installing a hypervisor (such as VMware) on.

#### 4.3.c Containers

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. - [What is a Container](https://www.docker.com/resources/what-container)

One benefit of a container is the ability to separate applications into their own "contained" environments. Which means you could update / redeploy one container without changing or affecting any other containers running on the same host.

### 4.4 Describe components for a CI/CD pipeline in application deployments

### 4.5 Construct a Python unit test

### 4.6 Interpret contents of a Dockerfile

### 4.7 Utilize Docker images in local developer environment

### 4.8 Identify application security issues related to secret protection, encryption (storage and transport), and data handling

### 4.9 Explain how firewall, DNS, load balancers, and reverse proxy in application deployment

### 4.10 Describe top OWASP threats (such as XSS, SQL injections, and CSRF)

### 4.11 Utilize Bash commands (file management, directory navigation, and environmental variables)

### 4.12 Identify the principles of DevOps practices

## 5.0 Infrastructure and Automation (20%)

### 5.1 Describe the value of model driven programmability for infrastructure automation

### 5.2 Compare controller-level to device-level management

### 5.3 Describe the use and roles of network simulation and test tools (such as VIRL and pyATS)

### 5.4 Describe the components and benefits of CI/CD pipeline in infrastructure automation

### 5.5 Describe principles of infrastructure as code

### 5.6 Describe the capabilities of automation tools such as Ansible, Puppet, Chef, and Cisco NSO

### 5.7 Identify the workflow being automated by a Python script that uses Cisco APIs including ACI, Meraki, Cisco DNA Center, or RESTCONF

### 5.8 Identify the workflow being automated by an Ansible playbook (management packages, user management related to services, basic service configuration, and start/stop)

### 5.9 Identify the workflow being automated by a bash script (such as file management, app install, user management, directory navigation)

### 5.10 Interpret the results of a RESTCONF or NETCONF query

### 5.11 Interpret basic YANG models

### 5.12 Interpret a unified diff

### 5.13 Describe the principles and benefits of a code review process

### 5.14 Interpret sequence diagram that includes API calls

## 6.0 Network Fundamentals (15%)

### 6.1 Describe the purpose and usage of MAC addresses and VLANs

### 6.2 Describe the purpose and usage of IP addresses, routes, subnet mask / prefix, and gateways

### 6.3 Describe the function of common networking components (such as switches, routers, firewalls, and load balancers)

### 6.4 Interpret a basic network topology diagram with elements such as switches, routers, firewalls, load balancers, and port values

### 6.5 Describe the function of management, data, and control planes in a network device

### 6.6 Describe the functionality of these IP Services: DHCP, DNS, NAT, SNMP, NTP

### 6.7 Recognize common protocol port values (such as, SSH, Telnet, HTTP, HTTPS, and NETCONF)

### 6.8 Identify cause of application connectivity issues (NAT problem, Transport Port blocked, proxy, and VPN)

### 6.9 Explain the impacts of network constraints on applications