---
title: "ASP.NET Core MVC 概述"
author: ardalis
description: "了解如何通过 ASP.NET Core MVC 框架并使用MVC模式生成 Web 应用和 Api 。"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 2492b6aa4602dbbf3b9cd3dca00d40690c640cab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="overview-of-aspnet-core-mvc"></a>ASP.NET Core MVC 概述

由[Steve Smith](https://ardalis.com/)编写

ASP.NET Core MVC 是一个丰富的框架，用于生成 Web 应用和 Api ， 并使用模型-视图-控制器设计模式。

## <a name="what-is-the-mvc-pattern"></a>什么是 MVC 模式？

模型-视图-控制器 (MVC) 体系结构模式将应用程序分成三个组件的主要组： 模型、 视图和控制器。 此模式有助于实现[关注点分离](http://deviq.com/separation-of-concerns/)。 使用此模式，用户请求会被路由到一个负责和模型交互来执行用户操作或检索查询结果的控制器。 控制器会选择要呈现给用户的视图，并为该视图提供它需要的模型数据。

下图显示了三个主要组件以及相互之间的引用关系：

![MVC 模式](overview/_static/mvc.png)

这样的职责划分能帮助你降低应用程序的复杂性，因为它可以让你更轻松地编写代码、 调试和测试拥有单一职责的各个部分 （模型、 视图或控制器）， 并且遵循了[单一职责原则](http://deviq.com/single-responsibility-principle/). 而如果要在依赖项涉及到以上三项中两项或以上时，更新、测试和调试代码将会变得困难起来。 例如，用户界面逻辑往往会比业务逻辑变化更多。 如果用于呈现的代码和业务逻辑耦合在同一个对象，那么你每次修改用户界面时，还不得不修改包含业务逻辑的对象的代码。 这可能会引入错误并且在每次对用户界面进行细微改动后都需要重新测试所有业务逻辑。

> [!NOTE]
> 视图和控制器依赖于模型。 但是模型并不会依赖其他二者。 这是分离的主要优点之一。 这种分离使模型可以独立于视觉呈现来创建和测试。

### <a name="model-responsibilities"></a>模型的职责

一个 MVC 应用程序中的模型表示应用程序状态以及任何由它执行的业务逻辑和操作。 模型应当封装业务逻辑和维持应用程序状态的实现逻辑。 强类型视图通常使用为其专门设计的视图模型类型，这些类型包含了要显示的数据， 而控制器则会从模型来创建和填充视图模型。

> [!NOTE]
> 有多种方法来组织使用 MVC 体系结构模式的应用程序的模型。 深入了解某些[不同种类的模型类型](http://deviq.com/kinds-of-models/)。

### <a name="view-responsibilities"></a>视图的职责

视图负责通过用户界面提供内容。 它们使用[Razor 视图引擎](#razor-view-engine)将.NET 代码嵌入 HTML 标记。 在视图内，应当只包含极少量的逻辑，并且这些逻辑应当都和呈现内容相关。 如果你发现为了显示一个复杂模型中的数据需要在视图文件中执行大量的逻辑，请考虑使用[视图组件](views/view-components.md)，视图模型，或视图模板，以简化视图。

### <a name="controller-responsibilities"></a>控制器的职责

控制器是处理用户交互、和模型交互、并最终选择将要渲染的视图的组件。 在 MVC 应用程序中，视图仅显示信息，控制器处理、并响应用户输入和交互。 在 MVC 模式中，控制器是初始入口点，并且负责选择要使用哪个模型类型，呈现哪个视图 (顾名思义——控制器控制应用程序如何响应给定的请求)。

> [!NOTE]
> 控制器不应当因为过多的职责而变得过于复杂。 若要避免控制器逻辑变得过于复杂，使用[单一职责原则](http://deviq.com/single-responsibility-principle/)来把业务逻辑从控制器转移到域模型中。

>[!TIP]
> 如果你发现你的控制器频繁地执行相同种类的操作，则可以按照[切勿重复原则](http://deviq.com/don-t-repeat-yourself/)将这些操作移动到[过滤器](#filters)中。

## <a name="what-is-aspnet-core-mvc"></a>ASP.NET Core MVC 是什么

ASP.NET Core MVC 框架是轻量级的、 开源的、 高度可测试的呈现框架，并为ASP.NET Core优化。

ASP.NET Core MVC 提供了一种基于模式的方法来构建实现完全分离关注点的动态网站。 它让你完全控制标记，支持 TDD 友好的开发，并且使用最新的 web 标准。

## <a name="features"></a>特性

ASP.NET Core MVC 包括以下部分：

* [路由](#routing)
* [模型绑定](#model-binding)
* [模型验证](#model-validation)
* [依赖关系注入](../fundamentals/dependency-injection.md)
* [筛选器](#filters)
* [区域](#areas)
* [Web Api](#web-apis)
* [可测试性](#testability)
* [Razor 视图引擎](#razor-view-engine)
* [强类型化的视图](#strongly-typed-views)
* [标记助手](#tag-helpers)
* [视图组件](#view-components)

### <a name="routing"></a>路由

ASP.NET Core MVC 建立在[ASP.NET Core 路由](../fundamentals/routing.md)之上，这是一个功能强大的 URL 映射组件，让你可以构建具有易于理解和搜索的 Url。 这使你可以定义适用于搜索引擎优化 (SEO) 和链接生成的应用程序URL命名模式，而不用考虑你的 web 服务器上的文件的组织方式。 你可使用方便的路由模板语法来定义路由，并且支持路由值约束、默认值和可选值。

*基于约定的路由*让你可以全局定义应用程序可接受的URL格式以及各个格式如何映射到给定的控制器中的特定的操作方法。 当收到传入的请求时，路由引擎将解析该 URL 并匹配到一种定义的 URL 格式，然后调用关联的控制器操作方法。

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*属性路由*使您能够通过使用定义应用程序路由的属性修饰控制器和操作来指定路由信息。 这意味着路由定义就放在相关控制器和操作的旁边。

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>模型绑定

ASP.NET Core MVC[模型绑定](models/model-binding.md)将客户端请求数据 （窗体值、 路由数据、 查询字符串参数、 HTTP 标头） 转换为控制器可以处理的对象。 因此，控制器逻辑无需解析传入的请求数据;它只需将数据作为参数传入操作方法。

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>模型验证

ASP.NET Core MVC 可通过使用数据批注验证属性修饰模型对象来支持[验证](models/validation.md)。 在客户端将值发布到服务器之前，以及在服务器调用控制器操作之前，会在客户端侧检查数据验证属性。

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

控制器操作：

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

在服务器和客户端上，框架都会验证请求数据。 模型类型上指定的验证逻辑以非介入式批注的形式添加到被渲染的视图中，并且浏览器中使用[jQuery 验证](https://jqueryvalidation.org/)强制执行。

### <a name="dependency-injection"></a>依赖注入

ASP.NET Core 内置对[依赖注入 (DI)](../fundamentals/dependency-injection.md)的支持。 ASP.NET Core mvc[控制器](controllers/dependency-injection.md)可通过其构造函数请求依赖，从而使它们遵循[显式依赖原则](http://deviq.com/explicit-dependencies-principle/)。

你的应用程序还可使用`@inject`指令在视图文件中使用[依赖注入](views/dependency-injection.md)，：

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>过滤器

[过滤器](controllers/filters.md)帮助开发人员封装跨域问题，如异常处理或授权。 过滤器为控制器操作启用自定义的预处理或后处理逻辑，配置在给定的请求的处理管线的特定点执行。 过滤器可作为属性用于控制器或操作 （也可以全局运行）。 框架中包含多个过滤器 (如`Authorize`) 。


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>区域

[区域](controllers/areas.md)提供一种将大型 ASP.NET Core MVC Web 应用分成多个小的功能组的方法。 一个区域实际上是在应用程序内部的 MVC 结构。 在 MVC 项目中，逻辑组件，如模型、 控制器和视图放在不同的文件夹，并且 MVC 使用命名约定来创建这些组件之间的关系。 对于大型应用，将应用程序分成多个高级功能区域可能会更加有利。 例如，包含多个业务单元的电子商务应用，例如结算、 计费和搜索等等。这些单元每一个都具有自己的逻辑组件视图、 控制器和模型。

### <a name="web-apis"></a>Web API

ASP.NET Core MVC 不仅是构建网站的优秀平台，对于构建Web API同样有良好的支持。 你可以构建包括浏览器和移动设备在内的各种客户端都可以访问的服务。

框架支持将[数据格式化](models/formatting.md)为 JSON 或 XML来支持HTTP内容协商。 并可编写[自定义格式化程序](advanced/custom-formatters.md)添加对您自己的格式的支持。

使用链接生成来启用对超媒体的支持。 轻松启用对[跨域资源共享 (CORS)](http://www.w3.org/TR/cors/) 的支持，以便可以跨多个 Web 应用程序共享你的 Web Api。

### <a name="testability"></a>可测试性

框架对于接口和依赖注入的使用使其非常适合单元测试，并且框架也具有使[集成测试](../testing/integration-testing.md)快速又简单的特性（如 TestHost 和 InMemory 实体框架提供程序）。 详细了解[测试控制器逻辑](controllers/testing.md)。

### <a name="razor-view-engine"></a>Razor 视图引擎

[ASP.NET Core MVC 视图](views/overview.md)使用[Razor 视图引擎](views/razor.md)来呈现视图。 Razor 是使用嵌入 C# 代码的、用于定义视图的、简洁的、可读性高的、流畅的模板标记语言。 Razor 用于动态生成服务器上的 web 内容。 你完全可以混合服务器代码和客户端内容和代码。

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

使用 Razor 视图引擎可以定义[布局](views/layout.md)，[分部视图](views/partial.md)和可替换的部分。

### <a name="strongly-typed-views"></a>强类型视图

MVC中的 Razor 视图可以基于你的模型成为强类型的。 控制器可以将强类型模型传递给视图来使您的视图具有对类型检查和 IntelliSense 的支持。

例如，下面的视图定义了`IEnumerable<Product>`类型的模型:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>标记助手

[标记助手](views/tag-helpers/intro.md)使服务器端代码参与创建和呈现 Razor 文件中的 HTML 元素。 你可以使用标记助手来定义自定义标记 (例如， `<environment>`) 或修改现有标记的行为 (例如， `<label>`)。 标记助手根据元素名和属性绑定到特定的元素。 它们提供了服务端渲染的优点同时也保留了HTML编辑的体验。

有许多针对常规任务的内置标记助手-例如，创建窗体、 链接、 加载资产等，更多的帮助程序可在公共 GitHub 存储库获取或是作为 NuGet 程序包获取。 它们是使用 C# 编写的，并且是基于元素名称、 属性名称或父标记来生成 HTML 元素的。 例如，内置的 LinkTagHelper 可以用于创建一个链接到`Login`操作的链接`AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper`可用根据你的运行时环境，如开发、 过渡或生产时来包含不同的脚本 （例如，原始大小和最小化）：

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

标记助手提供 HTML 友好开发体验和用于创建 HTML 和 Razor 标记的丰富智能感知环境。 大部分内置标记帮助器都针对现有 HTML 元素，并为元素提供服务器端属性。

### <a name="view-components"></a>视图组件

[视图组件](views/view-components.md)可以打包渲染逻辑并整个应用程序中重用它。 它们类似于[分部视图](views/partial.md)，但具有关联的逻辑。
