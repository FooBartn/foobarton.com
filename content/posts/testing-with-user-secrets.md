---
title: "C# Testing With User Secrets"
date: 2020-02-15T23:12:53-06:00
description: "A quick guide on how to get User Secrets into your tests -- and your sensitive data out of your code!"
tags: [c#, .net, microsoft, testing]
---

# TLDR

Use ConfigurationBuilder, AddUserSecrets, and IConfiguration.GetValue("key") to get access to User Secrets from any .Net Core project. No complex procedure required! And minimal scarring!*

> *These claims have not been evaluated by any legitimate agency. Read at your own risk.

# Problem

"Don't push sensitive material to the source code repository."

Most developers are pretty familiar with this. Obviously, we don't want our precious keys and passwords escaping into The Wild Wild Web and falling prey to some nefarious basement dwelling evil-doer named DarkCLIde. *Obviously*.

With Unit Testing this isn't a problem. Unit Tests are done in "a vacuum" of sorts. The entire purpose of a Unit Test is to rely on as few dependencies as possible and test one single "Unit" or function at a time.

But what about Integration Testing?

You know, the practice of making sure things that work separately also work *together*. That way you don't end up with something like this:

![Two Unit Tests No Integration](/twounitnointegration.gif)

But when interacting with different live systems you usually need credentials of some sort: an API key, a username and password, etc. So where do you put them? 

You could use a file that you add to your .gitignore settings. But then your teammates have to make sure they do that too -- and that no one accidentally forgets. Because rewriting Git history sucks.

# User Secrets

With .Net Core you can use what are known as User Secrets. Since introduction this has been fairly easy to implement if you want to build an ASP.NET Core web application. ASP.NET Core does all the work for you.

But what if you built a library? Maybe one that interacts with an API? How do you get the User Secrets working? Luckily, with one of the more recent updates, they added "Manage User Secrets" to the context menu of more than just ASP.NET Core projects. This makes initial set up much easier!

> You can also use ```dotnet user-secrets init``` from within the project directory if you're not using Visual Studio.

Now you have all the necessary bits in place. What's next? This can be a little confusing since the documentation for User Secrets only refers to ASP.NET Core -- and for that you don't need to do anything else to get it working at this point except add data to your secrets file. 

But what about console applications and test projects?

# .Net Core Library

I'm going to focus on what an Xunit test project for a .Net Core Library would look like. Because it's the use case I came across that had very little documentation. Most of the examples I found had you build a whole ServiceCollection and set up Dependency Injection etc; and you could! But if you just want to use User Secrets, I can show you a much simpler way.

Let's assume you've written a library for interacting with an API that looks something like this:

```c#
public class MyApi
{
    private readonly string apiKey;

    public MyApi(string apiKey) {
        _apiKey = apiKey;
    } 

    public void AddUser(User user) => // Code to add user here
    public IEnumerable<User> GetUsers() => // Code to get users here
}

public class User 
{
    public User(string name) 
    {
        Name = name;
    }

    public string Name { get; set; }
}
```

Alright, so now how do we prove that, given an apiKey, AddUser actually adds a user? and GetUsers actually gets the user you added?

Integration Tests.

Keep in mind that at this level I'm not looking for efficiency. I don't care so much *how* it works, only that it *works*. i.e. The result is what I expect. So do your best to ignore how simplified and lacking in content the example code above is. 

Let's see the not-so-secure way first:

```c#
public class UserTests 
{
    [Fact]
    public void ShouldAddUser() {
        using var myApi = new MyApi("1580gskg023t83t0ig0s");
        var user = new User("Bob");
        myApi.AddUser(user);

        var users = myApi.GetUsers();
        users.Any(x => x.Name == "Bob").Should().BeTrue();
    }
}
```

Assuming this library works the way we expect it to, this should result in a passing test. We added the user "Bob" and then checked to see that the user "Bob" was added. But now our API key is sitting there. Out in the open. For anyone with access to the code to see.

![Danger, Will Robinson! Danger!](/dangerwillrobinson.gif)

# Using User Secrets

How do we fix it? With a little re-**Configuration**, of course. Let's assume this is the contents of our User Secrets file:

```json
{
    "ApiKey": "1580gskg023t83t0ig0s"
}
```


And now we'll modify our UserTests class to this:

```c#
public class UserTests 
{
    private string _apiKey;

    public UserTests() 
    {
        var configuration = new ConfigurationBuilder()
            .AddUserSecrets<Settings>()
            .Build();

        _apiKey = configuration.GetValue<string>("ApiKey");
    }

    [Fact]
    public void ShouldAddUser() {
        using var myApi = new MyApi(_apiKey);
        var user = new User("Bob");
        myApi.Add(user);

        var users = myApi.GetUsers();
        users.Any(x => x.Name == "Bob").Should().BeTrue();
    }
}
```

That's it!

So what have we done here? 
- We've created a configuration so we could use the AddUserSecrets extension method.
- We used the configuration method for getting the value of the property "ApiKey", which was added from the User Secrets file.

Now when the ShouldAddUser() test runs, it will pull the _apiKey from the configuration a.k.a. your User Secrets file; a file which is outside the project path and so is in no danger of being checked in via source control. 

Result? No sensitive data uploaded! Groot Happy Dance Time!

![Dancing Groot](/dancinggroot.gif)