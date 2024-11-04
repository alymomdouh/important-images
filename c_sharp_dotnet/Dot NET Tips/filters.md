###   JWT is common but have you ever felt the need for API Key Authentication?

In .NET, action filters are a part of the ASP .NET MVC and ASP .NET Core frameworks, 
allowing you to execute code before or after an action method is called. 

Various types of filters serve different purposes. Here are some common types:

1. Result Filter
2. Action Filters
3. Exception Filter
4. Authorization Filter

1) Result filters are executed before and after the execution of the action result. 
Useful for implementing global changes to the response.

2) Action filters allow you to run code before and after executing an action method. 
They are useful for tasks such as logging, caching, and modifying the result of an action.

3) Exception filters are executed when an unhandled exception occurs during the execution of an action method. 
They are used to handle and log exceptions.

4) Authorization filters are used to perform authorization checks before an action method is executed Helps in managing user authentication and authorization logic in a modular way.

API Key authorization is a common method to authenticate API requests by including a unique API key in the request headers. 

An Authorization Filter can be employed to check the presence and validity of the API key before allowing access to the API.

![image](https://github.com/user-attachments/assets/68e0c2e6-1dbb-4ad3-a906-e93480001efc)
