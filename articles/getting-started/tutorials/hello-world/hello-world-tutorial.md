---
uid: tutorials.hello-world-tutorial
---

# The "Hello World" tutorial

## Overview

This tutorial is designed to to give you a feel for what Intent Architect can do, and let you experience the common work flows of the tool. This tutorial covers the basics of:
* Creating an Intent Architect application
* Configuring your Architecture
* Designing your application through modeling
* Realizing your designs as code

By the end of the tutorial you will have built a complete ASP.NET Core web application, implemented using `Clean Architecture` design patterns, with some basic use cases.

This tutorial should take around 5 minutes.

## Create your first Intent Architect application

* Open Intent Architect. 
* Click `Create a new application`.

![Home Screen](images/create-application.png)

On the first screen of the wizard you can select your desired application architecture, go with the `Clean Architecture .NET` option.

* Select the `Clean Architecture .NET` template.
* Change the application name to `HelloWorldTutorial`.
* Check / change the `Location` field. (This is where your Intent Architect application will be created)
* Click `Next`.

![Select your architecture](images/choose-application-template.png)

This screen allows you to configure your selected Architecture, in the tutorial you are just going to use the default configuration.

* Click `Create`.

![Home Screen](images/choose-your-patterns.png)

You have successfully created an Intent Architect application.
* Intent Architect will now download and install modules based on the application setup wizard you just went through.
* You should see a `Ready` message in the bottom left, when this process is complete.

![Application created](images/application-created.png)

Next you will be designing the more functional aspects of your system, starting with Domain modeling.

## Model your domain

The domain model is where you model your business / problem domain. What are the concepts that exist in your problem space, what data do they have, how are they related, etc.

* Open the `Domain Designer`

![Open Domain Designer](images/open-domain-designer.png)

* Right-click anywhere on the grid, and select `New Class`.
* Change the name to `Planet`.
* Right-click on the Planet class, and select `Add Attribute`.
* Name the attribute `Name` of type `string`.
* Save the domain (`ctrl`+`s`).

![Model a Planet](images/model-planet.png)

> [!Note]
> This is the simplest domain model possible for the purposes of the "Hello  World" Tutorial. Real domain models are much more complex and interesting. If you want know more, check out this webinar, [A deep dive into Domain Modeling](https://www.youtube.com/watch?v=AFcOyF_TWAg).  TO DO Update link

Next you will be designing how your application can be interacted with.

## Model your services

In the `Service Designer` you can describe your application services, which dictates how you application can be interacted with. This designer also allows you to define which services are remotely accessible and how they are exposed.

* Open the `Services Designer`

![Open Service Designer](images/open-service-designer.png)

Lets create a service for your `Planet` entity.
* Right-click on `HelloWorldTutorial.Services` and select `Create CQRS CRUD Operations`.
* In the dialog window
    * select `Planet` from the dropdown.
    * Click `Done`.

![CRUD a service](images/crud-service.png)

> [!Note]
> The `Clean Architecture in .NET` template has installed an optional CRUD feature, which is what you used to quickly create service definitions and it will create default convention-based service implementations for you. This is a feature which can be used as is, embraced and extended  or skipped all together.    

Intent Architect has created a minimalist CQRS style service for us with basic CRUD operations for your `Planet` entity.
These service operations are currently only available with-in the application, your next step will be to expose these operations as a RESTful Api.  

* Click on `CreatePlanetCommand`.
* Hold down `Shift`
* Click on `GetPlanetsQuery`
* You should have all the `Command`s and `Query`s highlighted
* Right-click on any of the highlighted items and select `Expose as Http Endpoint`
* Save the Services (`ctrl`+`s`).

![Expose Services](images/expose-services.png)

All the `Command`s and `Query`s should now be decorated with REST verbs and routes.

That's designing done, and now you are going to have Intent Architect realize this design as source code.

## Integrate your design into your code base

In Intent Architect there is a process called the `Software Factory`, this process is responsible for:
* Materializing your design as code
* Integrating that code into you code base

So, lets see it in action.

* Click `Run Software Factory`. 

![Generate the code](images/run-software-factory.png)

This dialog is presenting you a view of what Intent Architect wants to change in code base, and gives you an opportunity to review it.

* Click `Apply Changes`.

![Navigate to source code](images/software-factory-post-apply.png)

What did Intent Architect just produce for you?

* Click on the blue hyperlink at the bottom left of the Software Factory dialog.
* This should open a folder containing all the generated source code.

> [!Note]
> Because there wasn't an existing code base Intent Architect has scaffolded the entire solution. Intent Architect is not merely a scaffolding tool, it is a continuous code integration tool, i.e. as you make incremental design changes in Intent Architect it is able to integrate these changes back into the existing code base on a continuous basis.     

Now you can open up the solution in your IDE.

* Open the `.sln` file.

![The generated solution](images/vs-solution.png)

> [!Note]
> If you are curious about the architecture of this solution, check out this webinar, [Intent Architect Webinar on Clean Architecture in .NET 7](https://www.youtube.com/watch?v=AFcOyF_TWAg).  


## Run the application

Run the application in your IDE and you should be presented with a Swagger based UI which you can use to interact with the application.

In the UI you can see the 5 service operations you modeled earlier, and are now able to interact with them

![Swagger of Service](images/swagger-ui.png)

Time to create your first `Planet`.
* Click on the `Post` operation.
* Click on `Try it out`, right hand side of screen.
* Update the `name` property in the Request body to `hello world`.
```
{
  "name": "hello world"
}
```
* Click the big blue `Execute` button.

![Create a Planet](images/create-planet.png)

If you scroll down a little you should see a `Server Response` with a code of `201` and in the response body there will be a `guid` which is the `Id` of your newly created `Planet` entity.

If you'd like to verify this:
* Copy to `Planet` id.
* Click on the `Get` operation, with route `/api/planets/{id}`.
* Click on `Try it out`, right hand side of screen.
* Paste the `Planet` id in the `id` text-box.
* Click the big blue `Execute` button.

You should get a response similar to this
```
{
  "id": "3ba435f3-89e8-4282-a12d-5a7522524e2c",
  "name": "hello world"
}
```

![Get the Planet](images/fetch-planet.png)

Congratulations, you have successfully built your first application using Intent Architect. 

At this point you should have some sense of what Intent Architect can do:
* Rapidly building applications
* Configure your application architecture
* Visually design your application and domain through modeling
* Achieve better architectural adherence and consistency through automation

## Next Steps

TO DO Link to Training Tutorial 

## Additional Related Resources
[Intent Architect Webinar on Clean Architecture in .NET 7](https://www.youtube.com/watch?v=AFcOyF_TWAg)
[A deep dive into Domain Modeling](https://www.youtube.com/watch?v=AFcOyF_TWAg).  TO DO Update link

