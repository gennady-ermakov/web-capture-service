# Atalasoft Web Capture Service
[![npm version](https://badge.fury.io/js/web-capture-service.svg)](https://badge.fury.io/js/web-capture-service)
[![NuGet version](https://badge.fury.io/nu/Atalasoft.Web.Capture.Service.svg)](https://badge.fury.io/nu/Atalasoft.Web.Capture.Service)
[![Bower version](https://badge.fury.io/bo/web-capture-service.svg)](https://badge.fury.io/bo/web-capture-service)

Web Capture Service includes a set of integrated components that can be used to easily capture-enable a website.
It uses Javascript, supported by an local scanning service on the client machine which 
could be deployed either as windows service or regular windows application. 

Also Web Capture Service supports scanning in multiuser environments - MS Terminal Server and Citrix. On these environments, multiple users work with Web Capture Service at the same time from different Windows logon sessions with the same user experience as on single-user machine.

### Installation

#### Bower
```bash
bower install web-capture-service
```

#### Nuget
```bash
PM> Install-Package Atalasoft.Web.Capture.Service
```

#### NPM
```bash
npm install web-capture-service
```

### How to use
You can find a tutorial about how to create a simple capture application in the [Atalasoft team blog](http://atalasoft.github.io/2016/08/03/nuget-tutorial-wcs/)

The other way is to read a [tutorial](https://atalasoft.github.io/web-capture-service/tutorial-2-2-demo-application.html) in the Web Capture documentation.
There is also a version of [sample for ASP.NET Core](https://atalasoft.github.io/web-capture-service/tutorial-2-5-demo-application-aspnet-core.html)

### Licensing 

To run the projects locally, you need to have a DotImage license. There are various way to acquire the license:

 - Use [DotImage Activation Wizard Visual Studio extension](https://visualstudiogallery.msdn.microsoft.com/88ff07c9-fe68-48bd-bfdc-3fbc8a0ec1db)
 - Download a complete DotImage installation package from the [Atalasoft web site](https://atalasoft.com). You will be prompted to activate the product during installation

This source code is property of Atalasoft, Inc. (http://www.atalasoft.com/)  
Permission for usage and modification of this code is only permitted 
with the purchase of a source code license.

### Related Articles

 - [Web Capture Service documentation](https://atalasoft.github.io/web-capture-service/)
 - [Introducing NuGet Packages](http://atalasoft.github.io/2016/05/03/introducing-nuget/)
 - [Introducing Activation Wizard Extension](http://atalasoft.github.io/2016/05/14/introducing-activation-wizard-extension/) 
 - [NuGet Tutorial II - Web Capture Service](http://atalasoft.github.io/2016/08/03/nuget-tutorial-wcs/)
