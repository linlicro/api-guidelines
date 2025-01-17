# Microsoft REST API指南

## Microsoft REST API指南工作组

Name | Name | Name |
---------------------------- | -------------------------------------- | ----------------------------------------
Dave Campbell (CTO C+E)      | Rick Rashid (CTO ASG)                  | John Shewchuk (Technical Fellow, TED HQ)
Mark Russinovich (CTO Azure) | Steve Lucco (Technical Fellow, DevDiv) | Murali Krishnaprasad (Azure App Plat)
Rob Howard (ASG)             | Peter Torr  (OSG)                      | Chris Mullins (ASG)

<div style="font-size:150%">
Document editors: John Gossman (C+E), Chris Mullins (ASG), Gareth Jones (ASG), Rob Dolin (C+E), Mark Stafford (C+E)<br/>
</div>

## 1. 摘要

The Microsoft REST API Guidelines, as a design principle, encourages application developers to have resources accessible to them via a RESTful HTTP interface.
Microsoft REST API指南作为一种设计原则，鼓励应用程序开发人员通过RESTful HTTP接口访问资源。

To provide the smoothest possible experience for developers on platforms following the Microsoft REST API Guidelines, REST APIs SHOULD follow consistent design guidelines to make using them easy and intuitive.
REST APIs应该遵循一致的设计指南，给开发者提供最顺畅的体验，以使用它们更简单和流畅。

This document establishes the guidelines Microsoft REST APIs SHOULD follow so RESTful interfaces are developed consistently.
本文档建立了Microsoft REST API应该遵循的指导原则，以便统一一致的开发RESTful接口。

## 2. 目录
<!-- TOC depthFrom:2 depthTo:4 orderedList:false updateOnSave:false withLinks:true -->

- [Microsoft REST API Guidelines Working Group](#microsoft-rest-api-guidelines-working-group)
- [1. 摘要](#1-摘要)
- [2. 目录](#2-目录)
- [3. 介绍](#3-介绍)
  - [3.1. 推荐阅读](#31-推荐阅读)
- [4. 解读指导](#4-解读指导)
  - [4.1. 应用指南](#41-应用指南)
  - [4.2. 现有服务和服务版本控制的指南](#42-现有服务和服务版本控制的指南)
  - [4.3. 语言要求](#43-语言要求)
  - [4.4. 许可证](#44-许可证)
- [5. 分类](#5-分类)
  - [5.1. 错误](#51-错误)
  - [5.2. 故障](#52-故障)
  - [5.3. 延迟](#53-延迟)
  - [5.4. 完成时间](#54-完成时间)
  - [5.5. 长时间运行的API故障](#55-长时间运行的API故障)
- [6. 客户端指导](#6-客户端指导)
  - [6.1. 忽视规则](#61-忽视规则)
  - [6.2. 变量排序规则](#62-变量排序规则)
  - [6.3. 无声失败规则](#63-无声失败规则)
- [7. 基础](#7-基础)
  - [7.1. URL结构](#71-URL结构)
  - [7.2. URL长度](#72-URL长度)
  - [7.3. 规范标识符](#73-规范标识符)
  - [7.4. 支持的方法](#74-支持的方法)
    - [7.4.1. POST](#741-post)
    - [7.4.2. PATCH](#742-patch)
    - [7.4.3. 通过PATCH创建资源(UPSERT 定义)](#743-通过PATCH创建资源(UPSERT-定义))
    - [7.4.4. Options 标头 和 link headers 标签](#744-Options-标头-和-link-headers-标签)
  - [7.5. 标准的请求标头](#75-标准的请求标头)
  - [7.6. 标准响应标头](#76-标准响应标头)
  - [7.7. 自定义标头](#77-自定义标头)
  - [7.8. 指定头部为查询参数](#78-指定头部为查询参数)
  - [7.9. PII参数](#79-PII参数)
  - [7.10. 响应格式](#710-响应格式)
    - [7.10.1. 客户端指定响应格式](#7101-客户端指定响应格式)
    - [7.10.2. 错误的条件响应](#7102-错误的条件响应)
  - [7.11. HTTP状态代码](#711-HTTP状态代码)
  - [7.12. 可选的客户端库](#712-可选的客户端库)
- [8. CORS](#8-cors)
  - [8.1. Client guidance](#81-client-guidance)
    - [8.1.1. Avoiding preflight](#811-avoiding-preflight)
  - [8.2. Service guidance](#82-service-guidance)
- [9. Collections](#9-collections)
  - [9.1. Item keys](#91-item-keys)
  - [9.2. Serialization](#92-serialization)
  - [9.3. Collection URL patterns](#93-collection-url-patterns)
    - [9.3.1. Nested collections and properties](#931-nested-collections-and-properties)
  - [9.4. Big collections](#94-big-collections)
  - [9.5. Changing collections](#95-changing-collections)
  - [9.6. Sorting collections](#96-sorting-collections)
    - [9.6.1. Interpreting a sorting expression](#961-interpreting-a-sorting-expression)
  - [9.7. Filtering](#97-filtering)
    - [9.7.1. Filter operations](#971-filter-operations)
    - [9.7.2. Operator examples](#972-operator-examples)
    - [9.7.3. Operator precedence](#973-operator-precedence)
  - [9.8. Pagination](#98-pagination)
    - [9.8.1. Server-driven paging](#981-server-driven-paging)
    - [9.8.2. Client-driven paging](#982-client-driven-paging)
    - [9.8.3. Additional considerations](#983-additional-considerations)
  - [9.9. Compound collection operations](#99-compound-collection-operations)
- [10. Delta queries](#10-delta-queries)
  - [10.1. Delta links](#101-delta-links)
  - [10.2. Entity representation](#102-entity-representation)
  - [10.3. Obtaining a delta link](#103-obtaining-a-delta-link)
  - [10.4. Contents of a delta link response](#104-contents-of-a-delta-link-response)
  - [10.5. Using a delta link](#105-using-a-delta-link)
- [11. JSON standardizations](#11-json-standardizations)
  - [11.1. JSON formatting standardization for primitive types](#111-json-formatting-standardization-for-primitive-types)
  - [11.2. Guidelines for dates and times](#112-guidelines-for-dates-and-times)
    - [11.2.1. Producing dates](#1121-producing-dates)
    - [11.2.2. Consuming dates](#1122-consuming-dates)
    - [11.2.3. Compatibility](#1123-compatibility)
  - [11.3. JSON serialization of dates and times](#113-json-serialization-of-dates-and-times)
    - [11.3.1. The `DateLiteral` format](#1131-the-dateliteral-format)
    - [11.3.2. Commentary on date formatting](#1132-commentary-on-date-formatting)
  - [11.4. Durations](#114-durations)
  - [11.5. Intervals](#115-intervals)
  - [11.6. Repeating intervals](#116-repeating-intervals)
- [12. Versioning](#12-versioning)
  - [12.1. Versioning formats](#121-versioning-formats)
    - [12.1.1. Group versioning](#1211-group-versioning)
  - [12.2. When to version](#122-when-to-version)
  - [12.3. Definition of a breaking change](#123-definition-of-a-breaking-change)
- [13. Long running operations](#13-long-running-operations)
  - [13.1. Resource based long running operations (RELO)](#131-resource-based-long-running-operations-relo)
  - [13.2. Stepwise long running operations](#132-stepwise-long-running-operations)
    - [13.2.1. PUT](#1321-put)
    - [13.2.2. POST](#1322-post)
    - [13.2.3. POST, hybrid model](#1323-post-hybrid-model)
    - [13.2.4. Operations resource](#1324-operations-resource)
    - [13.2.5. Operation resource](#1325-operation-resource)
    - [13.2.6. Operation tombstones](#1326-operation-tombstones)
    - [13.2.7. The typical flow, polling](#1327-the-typical-flow-polling)
    - [13.2.8. The typical flow, push notifications](#1328-the-typical-flow-push-notifications)
    - [13.2.9. Retry-After](#1329-retry-after)
  - [13.3. Retention policy for operation results](#133-retention-policy-for-operation-results)
- [14. Throttling, Quotas, and Limits](#14-throttling-quotas-and-limits)
  - [14.1. Principles](#141-principles)
  - [14.2. Return Codes (429 vs 503)](#142-return-codes-429-vs-503)
  - [14.3. Retry-After and RateLimit Headers](#143-retry-after-and-ratelimit-headers)
  - [14.4. Service Guidance](#144-service-guidance)
    - [14.4.1. Responsiveness](#1441-responsiveness)
    - [14.4.2. Rate Limits and Quotas](#1442-rate-limits-and-quotas)
    - [14.4.3. Overloaded services](#1443-overloaded-services)
    - [14.4.4. Example Response](#1444-example-response)
  - [14.5. Caller Guidance](#145-caller-guidance)
  - [14.6. Handling callers that ignore Retry-After headers](#146-handling-callers-that-ignore-retry-after-headers)
- [15. Push notifications via webhooks](#15-push-notifications-via-webhooks)
  - [15.1. Scope](#151-scope)
  - [15.2. Principles](#152-principles)
  - [15.3. Types of subscriptions](#153-types-of-subscriptions)
  - [15.4. Call sequences](#154-call-sequences)
  - [15.5. Verifying subscriptions](#155-verifying-subscriptions)
  - [15.6. Receiving notifications](#156-receiving-notifications)
    - [15.6.1. Notification payload](#1561-notification-payload)
  - [15.7. Managing subscriptions programmatically](#157-managing-subscriptions-programmatically)
    - [15.7.1. Creating subscriptions](#1571-creating-subscriptions)
    - [15.7.2. Updating subscriptions](#1572-updating-subscriptions)
    - [15.7.3. Deleting subscriptions](#1573-deleting-subscriptions)
    - [15.7.4. Enumerating subscriptions](#1574-enumerating-subscriptions)
  - [15.8. Security](#158-security)
- [16. Unsupported requests](#16-unsupported-requests)
  - [16.1. Essential guidance](#161-essential-guidance)
  - [16.2. Feature allow list](#162-feature-allow-list)
    - [16.2.1. Error response](#1621-error-response)
- [17. Naming guidelines](#17-naming-guidelines)
  - [17.1. Approach](#171-approach)
  - [17.2. Casing](#172-casing)
  - [17.3. Names to avoid](#173-names-to-avoid)
  - [17.4. Forming compound names](#174-forming-compound-names)
  - [17.5. Identity properties](#175-identity-properties)
  - [17.6. Date and time properties](#176-date-and-time-properties)
  - [17.7. Name properties](#177-name-properties)
  - [17.8. Collections and counts](#178-collections-and-counts)
  - [17.9. Common property names](#179-common-property-names)
- [18. Appendix](#18-appendix)
  - [18.1. Sequence diagram notes](#181-sequence-diagram-notes)
    - [18.1.1. Push notifications, per user flow](#1811-push-notifications-per-user-flow)
    - [18.1.2. Push notifications, firehose flow](#1812-push-notifications-firehose-flow)

<!-- /TOC -->

## 3. 介绍

Developers access most Microsoft Cloud Platform resources via HTTP interfaces.
开发者通过HTTP接口访问Microsoft云平台资源。
Although each service typically provides language-specific frameworks to wrap their APIs, all of their operations eventually boil down to HTTP requests.
虽然各个服务通常提供特定语言框架来封装它们的APIs，但归根结底所有操作都是HTTP请求。
Microsoft must support a wide range of clients and services and cannot rely on rich frameworks being available for every development environment.
Microsoft必须支持广泛的客户端和服务端，以及不能依赖于每个开发环境使用的大量框架。
Thus, a goal of these guidelines is to ensure Microsoft REST APIs can be easily and consistently consumed by any client with basic HTTP support.
因此，本指南的目的是确保Microsoft REST API能够被各个客服端使用基础的HTTP支持，轻松和一致地使用。

To provide the smoothest possible experience for developers, it's important to have these APIs follow consistent design guidelines, thus making using them easy and intuitive.
为提供开发者最顺畅的体验，API设计需要遵循统一的设计指南很重要，从而使API的使用上更简易和直观。
This document establishes the guidelines to be followed by Microsoft REST API developers for developing such APIs consistently.
本文档建立Microsoft REST API开发者需遵循的指南，以使更一致性地开发的APIs。

The benefits of consistency accrue in aggregate as well; consistency allows teams to leverage common code, patterns, documentation and design decisions.
一致性的好处在于积累；一致性让团队可以使用共同的代码、模式、文档和设计策略。

These guidelines aim to achieve the following:

- Define consistent practices and patterns for all API endpoints across Microsoft.
- Adhere as closely as possible to accepted REST/HTTP best practices in the industry at-large. [\*]
- Make accessing Microsoft Services via REST interfaces easy for all application developers.
- Allow service developers to leverage the prior work of other services to implement, test and document REST endpoints defined consistently.
- Allow for partners (e.g., non-Microsoft entities) to use these guidelines for their own REST endpoint design.

这些准则目的如下:

- 为Microsoft平台的所有API端点定义一致的实践和模式。
- 尽可能坚持遵循行业普遍支持的 REST/HTTP 最佳实践。
- 应用开发者可以轻松地通过REST接口访问Microsoft服务。
- 允许应用开发者利用其它服务的基础上实现、测试和编写一致的REST端点。
- 允许合作伙伴(比如 非Microsoft团队)使用这些准则来设计他们自己的REST端点。

[\*] Note: The guidelines are designed to align with building services which comply with the REST architectural style, though they do not address or require building services that follow the REST constraints.
备注: 本指南为构建遵循REST架构风格的服务，但不要求构建遵循REST约束的服务。
The term "REST" is used throughout this document to mean services that are in the spirit of REST rather than adhering to REST by the book.*
本文档中使用的“REST”术语代指具有 RESTful风格的服务，而不是仅仅遵循 REST。

### 3.1. 推荐阅读

Understanding the philosophy behind the REST Architectural Style is recommended for developing good HTTP-based services.
If you are new to RESTful design, here are some good resources:
理解REST架构风格的理念，有助于开发优秀的基于HTTP的服务。如果你是刚接触到RESTful设计，请阅览下面的优秀资料:

[REST on Wikipedia][rest-on-wikipedia] -- Overview of common definitions and core ideas behind REST.
[REST on Wikipedia][rest-on-wikipedia]; 中文翻译: [表现层状态转换][表现层状态转换] -- 维基百科上关于REST的核心概念与思想的介绍。

[REST Dissertation][fielding] -- The chapter on REST in Roy Fielding's dissertation on Network Architecture, "Architectural Styles and the Design of Network-based Software Architectures"
[REST论文][fielding] -- Roy Fielding网络架构论文中关于REST的章节，“架构风格与基于网络的软件体系结构设计”。

[RFC 7231][rfc-7231] -- Defines the specification for HTTP/1.1 semantics, and is considered the authoritative resource.
[RFC 7231][rfc-7231] -- 定义HTTP/1.1 语义规范的权威资源。

[REST in Practice][rest-in-practice] -- Book on the fundamentals of REST.
[REST 实践][rest-in-practice] —— 关于REST的基础知识的入门书。

## 4. 解读指导

### 4.1. 应用指南

These guidelines are applicable to any REST API exposed publicly by Microsoft or any partner service.
这些准则适用于Microsoft或合作伙伴公开的任何REST API。
Private or internal APIs SHOULD also try to follow these guidelines because internal services tend to eventually be exposed publicly.
私有的或内部的APIs应该尝试遵循这些指导方针，因为内部服务最终会被公开。
Consistency is valuable to not only external customers but also internal service consumers, and these guidelines offer best practices useful for any service.
一致性不仅对外客户有价值，同时对内部人员也有价值，这些准则对任务服务有提供了有用价值。

There are legitimate reasons for exemption from these guidelines.
有合理理由场景可不遵循这些准则。
Obviously, a REST service that implements or must interoperate with some externally defined REST API must be compatible with that API and not necessarily these guidelines.
比如REST服务的实现或必须与某些外部REST API相互操作而需要兼容这些API，而无法满足这些准则。
Some services MAY also have special performance needs that require a different format, such as a binary protocol.
还有一些服务可能要特殊的性能要求而采用其他合适，比如二进制协议。

### 4.2. 现有服务和服务版本控制的指南

We do not recommend making a breaking change to a service that predates these guidelines simply for the sake of compliance.
我们不建议仅仅为了遵循准则而对现有的服务进行重大改变。
The service SHOULD try to become compliant at the next version release when compatibility is being broken anyway.
当服务兼容性会被破坏时，该服务应该尝试在下一个发布版本变的合规。
When a service adds a new API, that API SHOULD be consistent with the other APIs of the same version.
当一个服务添加新的API，该API应该与同版本的其他APIs保持一致。
So if a service was written against version 1.0 of the guidelines, new APIs added incrementally to the service SHOULD also follow version  1.0. The service can then upgrade to align with the latest version of the guidelines at the service's next major release.
因此一个服务针对1.0版本准则编写，向该服务增量新增的APIs应该遵循版本1.0准则。本服务能在下一个大版本升级时，再去遵循准则。

### 4.3. 语言要求

The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).
本文档中的关键字 "MUST"(必须), "MUST NOT"(禁止), "REQUIRED"(需要), "SHALL"(将要), "SHALL NOT"(最好不好), "SHOULD"(应该), "SHOULD NOT"(不应该), "RECOMMENDED"(推荐), "MAY"(可能), and "OPTIONAL"(可选) 的详细解析请见[RFC 2119](https://www.ietf.org/rfc/rfc2119.txt)。

### 4.4. 许可证

This work is licensed under the Creative Commons Attribution 4.0 International License.
To view a copy of this license, visit <http://creativecommons.org/licenses/by/4.0/> or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
本作品根据知识共享署名4.0国际许可协议授权。如需查看本授权的副本，请访问 <http://creativecommons.org/licenses/by/4.0/> 或致函 PO Box 1866, Mountain View, CA 94042, USA.

注：署名 4.0 国际，也就是允许在任何媒介以任何形式复制、发行本作品，允许修改、转换或以本作品为基础进行创作。允许任何用途，甚至商业目的。

## 5. 分类

As part of onboarding to Microsoft REST API Guidelines, services MUST comply with the taxonomy defined below.
Microsoft REST API准备的首要部分，服务必须遵守以下定义的分类(taxonomy的翻译怪怪的...)。

### 5.1. 错误

Errors, or more specifically Service Errors, are defined as a client passing invalid data to the service and the service _correctly_  rejecting that data.
错误，或者具体的业务错误，因客服端向服务的输入非法的数据而拒绝该请求。
Examples include invalid credentials, incorrect parameters, unknown version IDs, or similar.
包括无效的凭证，错误的入参，未知的版本号等。
These are generally "4xx" HTTP error codes and are the result of a client passing incorrect or invalid data.
客户端传递错误的或者不合法的数据时，通常返回 “4XX” 的 HTTP 错误代码。

Errors do _not_ contribute to overall API availability.
错误不会影响API的整体可用性。

注：错误可以理解成客户端参数错误，通常返回“4XX”状态码，并不影响整体的API使用。

### 5.2. 故障

Faults, or more specifically Service Faults, are defined as the service failing to correctly return in response to a valid client request.
These are generally "5xx" HTTP error codes.
故障, 更具体的说是服务故障，服务无法正常地返回数据以响应客户端的正确请求，通常返回"5xx"HTTP错误代码。

Faults _do_ contribute to the overall API availability.
故障会影响全部的API可用性。

Calls that fail due to rate limiting or quota failures MUST NOT count as faults.
因限流或者配额失败的请求不算做故障。
Calls that fail as the result of a service fast-failing requests (often for its own protection) do count as faults.
因服务的快速失败(自身的保护机制)会被视为故障。

### 5.3. 延迟

Latency is defined as how long a particular API call takes to complete, measured as closely to the client as possible.
延迟是指一个特定API请求完成需花费的时间，尽可能用客户端来测量。
This metric applies to both synchronous and asynchronous APIs in the same way.
这个度量准则对同步和异步API都适用。
For long running calls, the latency is measured on the initial request and measures how long that call (not the overall operation) takes to complete.
针对长时间运行的请求，延迟是初始化后的第一次请求所需要的时长(而不是整个操作)。

### 5.4. 完成时间

Services that expose long operations MUST track "Time to Complete" metrics around those operations.
暴露操作需耗时很长的服务必须跟进"完成时间"指标。

### 5.5. 长时间运行的API故障

For a Long Running API, it's possible for both the initial request which begins the operation and the request which retrieves the results to technically work (each passing back a 200) but for the underlying operation to have failed.
针对长时间运行的API，很可能请求都是正常的(每次返回200 HTTP代码)，但后端服务是失败的。
Long Running faults MUST roll up as faults into the overall Availability metrics.
这类长期运行故障必须汇总到总体可用性指标中。

## 6. 客户端指导

To ensure the best possible experience for clients talking to a REST service, clients SHOULD adhere to the following best practices:
为保障客户端更好地使用REST服务，客户端需遵循以下最佳实践:

### 6.1. 忽视规则

For loosely coupled clients where the exact shape of the data is not known before the call, if the server returns something the client wasn't expecting, the client MUST safely ignore it.
对于松耦合的客户端只有在发生真实调用服务时才能确认数据的确切定义，如果服务有时返回的数据非客户端预期的，客户端必须安全忽视它。
  
Some services MAY add fields to responses without changing versions numbers.
有些服务可能在不更改版本号的情况下新增字段。
Services that do so MUST make this clear in their documentation and clients MUST ignore unknown fields.
此类服务必须在其文档中注明，客户端必须忽略这些未知字段。

注：一个已发布的在线接口服务，如果不修改版本而增加字段，那么一定不能影响已有的客户端调用。

### 6.2. 变量排序规则

Clients MUST NOT rely on the order in which data appears in JSON service responses.
客户端禁止依赖服务的响应的JSON格式数据的顺序。
For example, clients SHOULD be resilient to the reordering of fields within a JSON object.
例如，客服端需弹性地解析处理重排序的JSON格式数据。
When supported by the service, clients MAY request that data be returned in a specific order.
当服务的支持时，客户端可以请求以特定排序返回数据。
For example, services MAY support the use of the _$orderBy_ querystring parameter to specify the order of elements within a JSON array.
例如，服务端可能支持请求参数_$orderBy_来指定特定的排序返回数据。
Services MAY also explicitly specify the ordering of some elements as part of the service contract.
服务端也可以在服务约定中明确指定一些字段可进行排序。
For example, a service MAY always return a JSON object's "type" information as the first field in an object to simplify response parsing on the client.
例如，服务端可以一直返回JSON对象信息的第一个字段是"type"，用来简化客户端解析响应的难度。
Clients MAY rely on ordering behavior explicitly identified by the service.
客服端可以依赖服务端明确指定的排序行为。

### 6.3. 无声失败规则

Clients requesting OPTIONAL server functionality (such as optional headers) MUST be resilient to the server ignoring that particular functionality.
当客户端请求时带上了可选的服务功能(例如，可选的头部信息)，必须对服务端的返回格式做一定兼容，以忽略这些特点功能。

## 7. 基础

### 7.1. URL结构

Humans SHOULD be able to easily read and construct URLs.
URL必须有友好的可读性和可构造性，能够被轻松的阅读和构造url。

This facilitates discovery and eases adoption on platforms without a well-supported client library.
这利于发现和简化调用，即使平台没有良好的SDK支持。

An example of a well-structured URL is:
结构良好的URL一个例子:

```text
https://api.contoso.com/v1.0/people/jdoe@contoso.com/inbox
```

注：通过以上URL我们可以获知API的版本、people资源、用户标识（邮箱）、收件箱，而且很容易获知——这是jdoe的收件箱的API。

An example URL that is not friendly is:
一个不友好的URL例子:

```text
https://api.contoso.com/EWS/OData/Users('jdoe@microsoft.com')/Folders('AAMkADdiYzI1MjUzLTk4MjQtNDQ1Yy05YjJkLWNlMzMzYmIzNTY0MwAuAAAAAACzMsPHYH6HQoSwfdpDx-2bAQCXhUk6PC1dS7AERFluCgBfAAABo58UAAA=')
```

A frequent pattern that comes up is the use of URLs as values.
一个常见的模式是使用URL作为参数。
Services MAY use URLs as values.
服务端可以使用URLs作为参数。
For example, the following is acceptable:
例如，下面的URL是可以接受的:

```text
https://api.contoso.com/v1.0/items?url=https://resources.contoso.com/shoes/fancy
```

### 7.2. URL长度

The HTTP 1.1 message format, defined in RFC 7230, in section [3.1.1][rfc-7230-3-1-1], defines no length limit on the Request Line, which includes the target URL.
RFC 7230中定义的HTTP 1.1消息格式没有限制长度，包括目标URL:
From the RFC:

> HTTP does not place a predefined limit on the length of a
   request-line. [...] A server that receives a request-target longer than any URI it wishes to parse MUST respond
   with a 414 (URI Too Long) status code.
> HTTP没有对请求行长度预定义的限制。如果服务器接收到的请求请求目标过长，那么必须响应414(URI太长)状态码。

Services that can generate URLs longer than 2,083 characters MUST make accommodations for the clients they wish to support.
服务如果能够生成超过2,083个字符的url，必须考虑兼容它支持的客户端。
Here are some sources for determining what target clients support:
不同客户端支持的最长 URL 长度参见以下资料：

1. [http://stackoverflow.com/a/417184](http://stackoverflow.com/a/417184)
2. [https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/](https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/)

Also note that some technology stacks have hard and adjustable URL limits, so keep this in mind as you design your services.
还请注意，一些技术栈有强制的的URL限定，所以在设计服务时要记住这一点。

### 7.3. 规范标识符

In addition to friendly URLs, resources that can be moved or be renamed SHOULD expose a URL that contains a unique stable identifier.
除了友好的URL之外，能够移动或者重命名的资源必须暴露一个包含唯一的稳定的标识符。
It MAY be necessary to interact with the service to obtain a stable URL from the friendly name for the resource, as in the case of the "/my" shortcut used by some services.
与服务端交互最好需要通过友好的命名来获取资源，像有些服务使用'/my'快捷方式那样。

The stable identifier is not required to be a GUID.
固定标识符不强制要用 GUID。

An example of a URL containing a canonical identifier is:
一个包含规范的标识符的URL 例子:

```text
https://api.contoso.com/v1.0/people/7011042402/inbox
```

注：一般是暴露主键字段，也可以是其他唯一的易于理解的字段，比如姓名、标题、邮箱等等。
注：GUID太长而且不易于理解和阅读，如果不是必须，尽量少用此字段。

### 7.4. 支持的方法

Operations MUST use the proper HTTP methods whenever possible, and operation idempotency MUST be respected.
客户端必须尽可能正确的使用HTTP方法来执行操作，并且必须支持操作的幂等性。
HTTP methods are frequently referred to as the HTTP verbs.
HTTP方法通常就是HTTP动词。
The terms are synonymous in this context, however the HTTP specification uses the term method.
在这里术语是同义词，HTTP规范使用术语方法。

Below is a list of methods that Microsoft REST services SHOULD support.
下面是Microsoft REST服务应该支持的方法列表。
Not all resources will support all methods, but all resources using the methods below MUST conform to their usage.
并不是所有资源都支持所有方法，但是使用以下方法的所有资源必须符合它们的用法。

Method  | Description | Is Idempotent
------- | ------------- | -------------
GET     | 返回对象的当前值 | True
PUT     | 替代对象，或者创建命名对象 | True
DELETE  | 删除对象 | True
POST    | 根据提供的数据创建新的对象，或者提交 一个命令 | False
HEAD    | 返回GET请求响应的元数据。支持GET方法的资源最好也支持HEAD方法 | True
PATCH   | 更新对象的部分应用 | False
OPTIONS | 获得关于请求的相关信息；详细见下面 | True

<small>Table 1</small>

#### 7.4.1. POST

POST operations SHOULD support the Location response header to specify the location of any created resource that was not explicitly named, via the Location header.
POST操作应该支持重定向响应标头，以便通过重定向标头指定任何创建资源的链接。

As an example, imagine a service that allows creation of hosted servers, which will be named by the service:
例如，假设一个服务允许创建托管服务，同时命名服务:

```http
POST http://api.contoso.com/account1/servers
```

The response would be something like:
响应会是这样:

```http
201 Created
Location: http://api.contoso.com/account1/servers/server321
```

Where "server321" is the service-allocated server name.
其中"server321"是服务分配的服务器名。

Services MAY also return the full metadata for the created item in the response.
服务也可以在响应返回创建项的完整元数据。

#### 7.4.2. PATCH

PATCH has been standardized by IETF as the method to be used for updating an existing object incrementally (see [RFC 5789][rfc-5789]).
PATCH已被IETF标准化，用于增量更新已存在的对象(参见 [RFC 5789][rfc-5789])。
Microsoft REST API Guidelines compliant APIs SHOULD support PATCH.
Microsoft REST API准则兼容的APIs应该支持PATCH。

#### 7.4.3. 通过PATCH创建资源(UPSERT 定义)

Services that allow callers to specify key values on create SHOULD support UPSERT semantics, and those that do MUST support creating resources using PATCH.
允许调用者创建对象时指定键值数据的服务应该支持 UPSERT 定义，这些服务必须支持通过PATCH创建资源。
Because PUT is defined as a complete replacement of the content, it is dangerous for clients to use PUT to modify data.
因PUT被定义为内容全部替换，客户端使用PUT修改数据是危险的。
Clients that do not understand (and hence ignore) properties on a resource are not likely to provide them on a PUT when trying to update a resource, hence such properties could be inadvertently removed.
当尝试更新资源时，不了解(因此忽视)资源某些属性的客户端，很可能忽视这些属性，导致这些属性被不经意地删除。
Services MAY optionally support PUT to update existing resources, but if they do they MUST use replacement semantics (that is, after the PUT, the resource's properties MUST match what was provided in the request, including deleting any server properties that were not provided).
服务可以选择支持PUT来更新现有资源，但必须是完整替换(即PUT之后，资源的属性必须匹配请求中提供的内容，包括删除没有提供的任何服务端的属性)。

Under UPSERT semantics, a PATCH call to a nonexistent resource is handled by the server as a "create," and a PATCH call to an existing resource is handled as an "update." To ensure that an update request is not treated as a create or vice versa, the client MAY specify precondition HTTP headers in the request.
在UPSERT语义下，对不存在资源的 PATCH 调用，由服务器作为“创建”处理，对已存在的资源的 PATCH 调用作为“更新”处理。为了确保更新请求不被视为创建（反之亦然），客户端可以在请求中指定预先定义的 HTTP 请求头。
The service MUST NOT treat a PATCH request as an insert if it contains an If-Match header and MUST NOT treat a PATCH request as an update if it contains an If-None-Match header with a value of "*".
如果 PATCH 请求包含if-match标头，则服务不能将其视为插入;如果 PATCH 请求包含值为 “*” 的if-none-match头，则服务不能将其视为更新。

If a service does not support UPSERT, then a PATCH call against a resource that does not exist MUST result in an HTTP "409 Conflict" error.
如果服务不支持UPSERT，则针对不存在的资源的 PATCH 调用必须导致 HTTP “409 Conflict”错误。

#### 7.4.4. Options 标头 和 link headers 标签

OPTIONS allows a client to retrieve information about a resource, at a minimum by returning the Allow header denoting the valid methods for this resource.
OPTIONS 允许客户端查询某个资源的元信息，并至少可以通过返回支持该资源的有效方法（支持的动词类别）的Allow 标头。

注：当发起跨域请求时，浏览器会自动发起OPTIONS请求进行检查。

In addition, services SHOULD include a Link header (see [RFC 5988][rfc-5988]) to point to documentation for the resource in question:
此外，建议服务返回应该包括一个指向有关资源的稳定链接（Link header）(见RFC [RFC 5988][rfc-5988]):

```http
Link: <{help}>; rel="help"
```

Where {help} is the URL to a documentation resource.
其中{help}是指向文档资源的URL。

For examples on use of OPTIONS, see [preflighting CORS cross-domain calls][cors-preflight].
有关选项使用的示例，请参见完善CORS跨域调用。

### 7.5. 标准的请求标头

The table of request headers below SHOULD be used by Microsoft REST API Guidelines services.
遵循Microsoft REST API准则的服务应该使用下面的请求表头表格。
Using these headers is not mandated, but if used they MUST be used consistently.
使用这些标题不是强制性的，但如果使用它们则必须始终一致地使用。

All header values MUST follow the syntax rules set forth in the specification where the header field is defined.
所有标头值都必须遵循规范中规定的标头字段所规定的语法规则。
Many HTTP headers are defined in [RFC7231][rfc-7231], however a complete list of approved headers can be found in the [IANA Header Registry][IANA-headers]."
许多HTTP标头在[RFC7231][rfc-7231]中定义，但是在IANA标头注册表中可以找到完整的已批准头列表。

Header 标头 | Type   类型 | Description 描述
------- | ----------| -------------------------
Authorization | String | 请求的授权标头
Date| Date | 请求的时间戳，基于客户端的时钟，采用[RFC 5322][rfc-5322-3-3]日期和时间格式。服务器不应该对客户端时钟的准确性做任何假设。此标头可以包含在请求中，但在提供时必须采用此格式。当提供此报头时，必须使用格林尼治平均时间(GMT)作为时区参考。例如：Wed, 24 Aug 2016 18:41:30 GMT. 请注意，GMT正好等于UTC（协调世界时）。
Accept | Content type | 响应请求的内容类型，如: application/xml、text/xml、application/json、text/javascript (for JSONP)。 根据HTTP准则，这只是一个提示，响应可能有不同的内容类型，例如blob fetch，其中成功的响应将只是blob流作为有效负载。对于遵循OData的服务，应该遵循OData中指定的首选项顺序。
Accept-Encoding | Gzip, deflate | 如果适用，REST端点应该支持GZIP和DEFLATE编码。对于非常大的资源，服务可能会忽略并返回未压缩的数据。
Accept-Language | "en", "es", etc. | 指定响应的首选语言。不需要服务来支持这一点，但是如果一个服务支持本地化，那么它必须通过Accept-Language头来支持本地化。
Accept-Charset | Charset type like "UTF-8" | 默认值是UTF-8，但服务应该能够处理ISO-8859-1
Content-Type | Content type | Mime type of request body (PUT/POST/PATCH)
Prefer | return=minimal, return=representation | 如果指定了return = minimal首选项，则服务应该返回一个空主体（empty body）以响应一次成功插入或更新。如果指定了return = representation，则服务应该在响应中返回创建或更新的资源。如果服务的场景中客户端有时会从响应中获益，但有时响应会对带宽造成太大的影响，那么它们应该支持这个报头。
If-Match, If-None-Match, If-Range | String | 使用乐观并发控制支持资源更新的服务必须支持If-Match标头。服务也可以使用其他与ETag相关的头，只要它们遵循HTTP规范。

### 7.6. 标准响应标头

Services SHOULD return the following response headers, except where noted in the "required" column.
服务应该返回以下响应标头，除非在“required”列中注明。

Response Header 响应报头| Required 必填| Description 描述
--- | ------- | ---------------
Date | 必须 | 根据服务器的时钟，以[RFC 5322][rfc-5322-3-3]日期和时间格式处理响应。这个头必须包含在响应中。此报头必须使用格林尼治平均时间(GMT)作为时区参考。例如: `Wed, 24 Aug 2016 18:41:30 GMT`.请注意，GMT正好等于协调世界时(UTC)。
Content-Type | 必须 | 内容类型
Content-Encoding | 必须 | GZIP或DEFLATE，视情况而定
Preference-Applied | 可选 | 是否应用了首选项请求头中指示的首选项
ETag | 可选 | ETag响应头字段为请求的变量提供实体标记的当前值。与If-Match、If-None-Match和If-Range一起使用，实现乐观并发控制

### 7.7. 自定义标头

Custom headers MUST NOT be required for the basic operation of a given API.
基础操作的API不应该支持自定义标头。

Some of the guidelines in this document prescribe the use of nonstandard HTTP headers.
本文档中的一些规则规定了非标准HTTP标头的使用。
In addition, some services MAY need to add extra functionality, which is exposed via HTTP headers.
此外，一些服务可以添加额外的功能，这些功能通过HTTP标头公开。
The following guidelines help maintain consistency across usage of custom headers.
以下准则有助于保持自定义标头的使用一致性。

Headers that are not standard HTTP headers MUST have one of two formats:
非标准的HTTP标头必须具有以下两个格式之一:

1. 使用IANA([RFC 3864][rfc-3864])注册为"临时"的标头的通用格式。
2. 注册标头的特定格式为特定用途。

These two formats are described below.
这两种格式如下所述。

### 7.8. 指定头部为查询参数

Some headers pose challenges for some scenarios such as AJAX clients, especially when making cross-domain calls where adding headers MAY not be supported.
有些标头对某些场景(如AJAX客户端)不兼容，特别是在不支持添加标头的跨域调用时。
As such, some headers MAY be accepted as Query Parameters in addition to headers, with the same naming as the header:
因此，除了常见的标头信息外，一些标头信息可以允许被作为查询参数传递给服务端，其命名与请求头中的名称保持一致:

Not all headers make sense as query parameters, including most standard HTTP headers.
并不是所有的标头都可以用作查询参数，包括大多数标准HTTP标头。

The criteria for considering when to accept headers as parameters are:
考虑何时接受标头作为参数的标准如下:

1. 任何自定义标头也必须作为参数接受。
2. 请求的标准标头也可以作为参数接受。
3. 具有安全敏感性的必需标头(例如，授权标头 Authorization)可能不适合作为参数;服务所有者应该具体情况具体分析。

The one exception to this rule is the Accept header.
此规则的一个例外是Accept头。
It's common practice to use a scheme with simple names instead of the full functionality described in the HTTP specification for Accept.
使用具有简单名称的方案，而不是使用HTTP规范中描述的用于Accept的完整功能，这是一种常见的实践。

### 7.9. PII参数

Consistent with their organization's privacy policy, clients SHOULD NOT transmit personally identifiable information (PII) parameters in the URL (as part of path or query string) because this information can be inadvertently exposed via client, network, and server logs and other mechanisms.
普遍的隐私政策一致，客户端不应该在URL中传输个人身份信息(PII[personally identifiable information])参数(作为路径或查询参数)，因为这些信息可能通过客户端、网络和服务器日志和其他机制无意暴露出来。

Consequently, a service SHOULD accept PII parameters transmitted as headers.
因此，服务应该接受PII参数作为标头传输。

However, there are many scenarios where the above recommendations cannot be followed due to client or software limitations.
然而在实践中，由于客户端或软件的限制，在许多情况下无法遵循上述建议。
To address these limitations, services SHOULD also accept these PII parameters as part of the URL consistent with the rest of these guidelines.
为了解决这些限制，服务也应该接受这些PII参数作为URL的一部分，与本指导原则的其余部分保持一致。

Services that accept PII parameters -- whether in the URL or as headers -- SHOULD be compliant with privacy policy specified by their organization's engineering leadership.
接受PII参数(无论是在URL中还是作为标头)的服务 应该符合其组织的隐私保护原则。
This will typically include recommending that clients prefer headers for transmission and implementations adhere to special precautions to ensure that logs and other service data collection are properly handled.
通常建议包括：客户端使用标头进行加密传输，并且实现要遵循特殊的预防措施，以确保日志和其他服务数据收集得到正确的处理。

注：PII——个人可标识信息。比如家庭地址，身份证信息。

### 7.10. 响应格式

For organizations to have a successful platform, they must serve data in formats developers are accustomed to using, and in consistent ways that allow developers to handle responses with common code.
一个成功的平台，往往提供可读性较好并且一致的响应结果，并允许开发人员使用公共 Http 代码处理响应。

Web-based communication, especially when a mobile or other low-bandwidth client is involved, has moved quickly in the direction of JSON for a variety of reasons, including its tendency to be lighter weight and its ease of consumption with JavaScript-based clients.
基于Web的通信，特别是当涉及移动端或其他低带宽客户端时，我们推荐使用JSON作为传输格式。主要是由于其更轻量以及易于与JavaScript交互。

JSON property names SHOULD be camelCased.
JSON属性名应该采用camelCasedE驼峰命名规范。

Services SHOULD provide JSON as the default encoding.
服务应该提供JSON作为默认输出格式。

#### 7.10.1. 客户端指定响应格式

In HTTP, response format SHOULD be requested by the client using the Accept header.
在HTTP中，客户端应该使用Accept头请求响应格式。
This is a hint, and the server MAY ignore it if it chooses to, even if this isn't typical of well-behaved servers.
服务端可以选择性的忽略
Clients MAY send multiple Accept headers and the service MAY choose one of them.
如客户端发送多个Accept标头，服务可以选择其中一个格式进行响应。

The default response format (no Accept header provided) SHOULD be application/json, and all services MUST support application/json.
默认的响应格式(没有提供Accept头)应该是application/json，并且所有服务必须支持application/json。

接受标头 | 响应类型 | 备注
---- | -----------| -------
application/json | 必须是返回json格式 | 同样接受JSONP请求的text/JavaScript

```http
GET https://api.contoso.com/v1.0/products/user
Accept: application/json
```

#### 7.10.2. 错误的条件响应

For non-success conditions, developers SHOULD be able to write one piece of code that handles errors consistently across different Microsoft REST API Guidelines services.
对于调用不成功的情况，开发人员应该能够用相同的代码库一致地处理错误。
This allows building of simple and reliable infrastructure to handle exceptions as a separate flow from successful responses.
这允许构建简单可靠的基础架构来处理异常，将异常作为成功响应的独立处理流程来处理。
The following is based on the OData v4 JSON spec.
下面的代码基于OData v4 JSON规范。
However, it is very generic and does not require specific OData constructs.
但是，它非常通用，不需要特定的OData构造。
APIs SHOULD use this format even if they are not using other OData constructs.
即使api没有使用其他OData结构，也应该使用这种格式。

The error response MUST be a single JSON object.
错误响应必须是单个JSON对象。
This object MUST have a name/value pair named "error." The value MUST be a JSON object.
该对象必须有一个名为“error”的 名称/值（name/value） 对。该值必须是JSON对象。

This object MUST contain name/value pairs with the names "code" and "message," and it MAY contain name/value pairs with the names "target," "details" and "innererror."
这个对象必须包含名称“code”和“message”的 键值对，并且它建议包含譬如“target”、“details”和 “innererror” 的键值对。

The value for the "code" name/value pair is a language-independent string.
“code”键值对的值 是一个与语言无关的字符串。
Its value is a service-defined error code that SHOULD be human-readable.
它的值是该服端务定义的错误代码，应该简单可读。
This code serves as a more specific indicator of the error than the HTTP error code specified in the response.
与响应中指定的HTTP错误代码相比，此代码用作错误的更具体的指示。
Services SHOULD have a relatively small number (about 20) of possible values for "code," and all clients MUST be capable of handling all of them.
服务应该具有相对较少的“code”数量(别超过20个)，并且所有客户端必须能够处理所有这些错误信息。
Most services will require a much larger number of more specific error codes, which are not interesting to all clients.
大多数服务将需要更大数量的更具体的错误代码以满足所有的客户端请求。
These error codes SHOULD be exposed in the "innererror" name/value pair as described below.
这些错误代码应该在“innererror” 键值对中公开，如下所述。
Introducing a new value for "code" that is visible to existing clients is a breaking change and requires a version increase.
为现有客户端可见的“代码”引入新值是一个破坏性的更改，需要增加版本。
Services can avoid breaking changes by adding new error codes to "innererror" instead.
服务可以通过向“innererror”添加新的错误代码来避免中断服务更改。

The value for the "message" name/value pair MUST be a human-readable representation of the error.
“message”键值对的值 必须是错误提示消息，必须是可读且易于理解。
It is intended as an aid to developers and is not suitable for exposure to end users.
它旨在是帮助开发人员，不适合暴露给最终用户。
Services wanting to expose a suitable message for end users MUST do so through an [annotation][odata-json-annotations] or custom property.
想要为最终用户公开合适消息的服务必须通过[annotation][odata-json-annotations]注释或其他自定义属性来公开。
Services SHOULD NOT localize "message" for the end user, because doing so might make the value unreadable to the app developer who may be logging the value, as well as make the value less searchable on the Internet.
服务不应该为最终用户本地化“message”，因为这样对于开发者变得非常不友好并且难以处理。

The value for the "target" name/value pair is the target of the particular error (e.g., the name of the property in error).
“target”键值对的值 是指向错误的具体的目标(例如，错误中属性的名称)。

The value for the "details" name/value pair MUST be an array of JSON objects that MUST contain name/value pairs for "code" and "message," and MAY contain a name/value pair for "target," as described above.
“details”键值对的值 必须是JSON对象数组，其中必须包含“code”和“message”的键值对，还可能包含“target”的键值对，如上所述。
The objects in the "details" array usually represent distinct, related errors that occurred during the request.
“details”数组中的对象通常表示请求期间发生的不同的、相关的错误。

See example below.
请参见下面的例子。

The value for the "innererror" name/value pair MUST be an object.
“innererror”键值对的值 必须是一个对象。
The contents of this object are service-defined.
这个对象的内容是服务端定义的。
Services wanting to return more specific errors than the root-level code MUST do so by including a name/value pair for "code" and a nested "innererror." Each nested "innererror" object represents a higher level of detail than its parent.
想要返回比根级别代码更具体的错误的服务，必须包含“code”的键值对和嵌套的“innererror”。每个嵌套的“innererror”对象表示比其父对象更高层次的细节。
When evaluating errors, clients MUST traverse through all of the nested "innererrors" and choose the deepest one that they understand.
在评估错误时，客户端必须遍历所有嵌套的“内部错误”，并选择他们能够理解的最深的一个。
This scheme allows services to introduce new error codes anywhere in the hierarchy without breaking backwards compatibility, so long as old error codes still appear.
这个方案允许服务在层次结构的任何地方引入新的错误代码，而不破坏向后兼容性，只要旧的错误代码仍然出现。
The service MAY return different levels of depth and detail to different callers.
服务可以向不同的调用者返回不同级别的深度和细节。
For example, in development environments, the deepest "innererror" MAY contain internal information that can help debug the service.
例如，在开发环境中，最深的“innererror”可能包含有助于调试服务的内部信息。
To guard against potential security concerns around information disclosure, services SHOULD take care not to expose too much detail unintentionally.
为了防范信息公开带来的潜在安全问题，服务应注意不要无意中暴露过多的细节。
Error objects MAY also include custom server-defined name/value pairs that MAY be specific to the code.
错误对象还可以包括特定于代码的自定义服务器定义的键值对。
Error types with custom server-defined properties SHOULD be declared in the service's metadata document.
See example below.
带有自定义服务器定义属性的错误类型应该在服务的元数据文档中声明。请参见下面的例子。

Error responses MAY contain [annotations][odata-json-annotations] in any of their JSON objects.
错误响应返回的的任何JSON对象中都可能包含注释[annotations][odata-json-annotations]。

We recommend that for any transient errors that may be retried, services SHOULD include a Retry-After HTTP header indicating the minimum number of seconds that clients SHOULD wait before attempting the operation again.
我们建议，对于任何可能重试的临时错误，服务应该包含一个 Retry-After HTTP头，告诉客户端在再次尝试操作之前应该等待的最小秒数。

##### ErrorResponse : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`error` | Error | ✔ | The error object.

##### Error : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String | ✔ | 服务器定义的错误代码集之一。
`message` | String | ✔ | A human-readable representation of the error.
`target` | String |  | The target of the error.
`details` | Error[] |  | An array of details about specific errors that led to this reported error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### InnerError : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String |  | A more specific error code than was provided by the containing error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### Examples

Example of "innererror":
内部错误的例子:

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Previous passwords may not be reused",
    "target": "password",
    "innererror": {
      "code": "PasswordError",
      "innererror": {
        "code": "PasswordDoesNotMeetPolicy",
        "minLength": "6",
        "maxLength": "64",
        "characterTypes": ["lowerCase","upperCase","number","symbol"],
        "minDistinctCharacterTypes": "2",
        "innererror": {
          "code": "PasswordReuseNotAllowed"
        }
      }
    }
  }
}
```

In this example, the most basic error code is "BadArgument," but for clients that are interested, there are more specific error codes in "innererror."
在本例中，基本的错误代码是“BadArgument”，但是对于感兴趣的客户端，“innererror”中提供了更具体的错误代码。
The "PasswordReuseNotAllowed" code may have been added by the service at a later date, having previously only returned "PasswordDoesNotMeetPolicy."
“passwordreusenotal”代码可能是在之后的迭代中由该服务添加的，之前只返回“passwordnotmeetpolicy”。
Existing clients do not break when the new error code is added, but new clients MAY take advantage of it.
这种增量型的添加方式并不会破坏老的客户端的处理过程，而又可以给开发者一些更详细的信息。
The "PasswordDoesNotMeetPolicy" error also includes additional name/value pairs that allow the client to determine the server's configuration, validate the user's input programmatically, or present the server's constraints to the user within the client's own localized messaging.
“PasswordDoesNotMeetPolicy”错误还包括额外的键值对，这些键值对 允许客户机确定服务器的配置、以编程方式验证用户的输入，或者在客户机自己的本地化消息传递中向用户显示服务器的约束。

Example of "details":
“details”例子:

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Multiple errors in ContactInfo data",
    "target": "ContactInfo",
    "details": [
      {
        "code": "NullValue",
        "target": "PhoneNumber",
        "message": "Phone number must not be null"
      },
      {
        "code": "NullValue",
        "target": "LastName",
        "message": "Last name must not be null"
      },
      {
        "code": "MalformedValue",
        "target": "Address",
        "message": "Address is not valid"
      }
    ]
  }
}
```

In this example there were multiple problems with the request, with each individual error listed in "details."
在本例中，请求存在多处问题，每个错误都列在 “details” 字段中进行返回了。

### 7.11. HTTP状态代码

Standard HTTP Status Codes SHOULD be used; see the HTTP Status Code definitions for more information.
应使用标准HTTP状态码作为响应状态码; 更多信息，请参见HTTP状态代码定义。

### 7.12. 可选的客户端库

Developers MUST be able to develop on a wide variety of platforms and languages, such as Windows, macOS, Linux, C#, Python, Node.js, and Ruby.
开发人员必须能够在各种平台和语言上进行开发，比如Windows、macOS、Linux、c#、Python和Node.js或是Ruby。

Services SHOULD be able to be accessed from simple HTTP tools such as curl without significant effort.
服务应该能够让简单的HTTP工具(如curl)进行访问，而不需要做太多的工作。

Service developer portals SHOULD provide the equivalent of "Get Developer Token" to facilitate experimentation and curl support.
该服务提供给开发人员的网站应该提供相当于“获得开发者令牌(Get developer Token)的功能，以帮助开发人员测试并应提供curl支持。

## 8. CORS 跨域

Services compliant with the Microsoft REST API Guidelines MUST support [CORS (Cross Origin Resource Sharing)][cors].
符合Microsoft REST API准则的服务必须支持[CORS (Cross Origin Resource Sharing)][cors](跨源资源共享)。
Services SHOULD support an allowed origin of CORS * and enforce authorization through valid OAuth tokens.
服务应该支持CORS *的允许起源，并通过有效的OAuth令牌强制授权。
Services SHOULD NOT support user credentials with origin validation.
服务不应该支持带有源验证的用户凭据。
There MAY be exceptions for special cases.
特殊情况可例外。

### 8.1. Client guidance 客户端指导

Web developers usually don't need to do anything special to take advantage of CORS.
Web开发人员通常不需要做任何特殊处理来利用CORS。
All of the handshake steps happen invisibly as part of the standard XMLHttpRequest calls they make.
作为标准XMLHttpRequest调用的一部分，所有握手步骤都是不可见的。
Many other platforms, such as .NET, have integrated support for CORS.
许多其他平台（如.NET）已集成了对CORS的支持。

#### 8.1.1. Avoiding preflight 避免额外的预检查

Because the CORS protocol can trigger preflight requests that add additional round trips to the server, performance-critical apps might be interested in avoiding them.
由于CORS协议会触发向服务器添加额外往返的预检请求，因此，注重性能的应用程序可能会有意避免这些请求。
The spirit behind CORS is to avoid preflight for any simple cross-domain requests that old non-CORS-capable browsers were able to make.
CORS背后的精神是避免对旧的不支持CORS功能的浏览器能够做出的任何简单的跨域请求进行预检。
All other requests require preflight.
所有其他请求都需要预检。

A request is "simple" and avoids preflight if its method is GET, HEAD or POST, and if it doesn't contain any request headers besides Accept, Accept-Language and Content-Language.
请求是“简单类型请求“，如果其方法是GET，HEAD或POST，并且除了Accept，Accept-Language和Content-Language之外它不包含任何请求标头，则可以免去预检。
For POST requests, the Content-Type header is also allowed, but only if its value is "application/x-www-form-urlencoded," "multipart/form-data" or "text/plain."
对于POST请求，也允许使用Content-Type标头，但前提是其值为“application/x-www-form-urlencoded”，“multipart/form-data”或“text/plain”。
For any other headers or values, a preflight request will happen.
对于任何其他标头或值，将发生预检请求。

### 8.2. Service guidance 服务指南

 At minimum, services MUST:
 服务必须至少：

- 了解浏览器在跨域请求上发送的Origin请求标头，以及他们在检查访问权限的预检OPTIONS 请求上发送的  Access-Control-Request-Method请求标头。
- 如果请求中存在Origin标头：
  - 如果请求使用 OPTIONS 方法并包含 Access-Control-Request-Method标头，则它是一个预检请求，用于在实际请求之前探测访问。否则，这是一个实际的请求。对于预检请求，除了执行以下步骤添加标头之外，服务必须不执行任何额外处理，并且必须返回 200 OK。对于非预检请求，除了请求的常规处理之外，还会添加以下标头。
  - 服务向响应添加 Access-Control-Allow-Origin 标头，其中包含与Origin 请求标头相同的值。请注意，这需要服务来动态生成标头值。不需要cookie或任何其他形式的[user credentials][cors-user-credentials]可以使用通配符星号(\*)进行响应。请注意，通配符仅在此处可接受，但不适用于下面描述的其他表头。
  - 如果调用者需要访问不属于[简单响应头][cors-simple-headers]集合中的响应头（Cache-Control，Content-Language，Content-Type，Expires，Last-Modified，Pragma），同时添加一个Access-Control-Expose-Headers标头，其中包含客户端应有权访问的其他响应标头名称列表。
  - 如果请求需要cookie，则添加一个Access-Control-Allow-Credentials头，并将其设置为“true”。
  - 如果请求是预检请求(见第一个项目符号)，则服务必须满足:
    - 添加一个Access-Control-Allow-Headers响应标头，其中包含允许客户端使用的请求标头名称列表。这个列表只需要包含不在[简单请求头][cors-simple-headers]  (Accept、Accept- language、Content-Language)集合中的头。如果服务接受的报头没有限制，则服务可以简单地返回与客户机发送的访问-控制-请求-报头报头相同的值。
    - 添加一个Access-Control-Allow-Methods响应头，其中包含允许调用方使用的HTTP方法列表。

添加一个Access-Control-Max-Age pref响应头，其中包含此预检前响应有效的秒数(因此可以在后续实际请求之前避免)。注意，虽然习惯上使用较大的值，比如2592000(30天)，但是许多浏览器会自动设置一个更低的限制(例如，5分钟)。

Because browser preflight response caches are notoriously weak, the additional round trip from a preflight response hurts performance.
众所周知，由于浏览器预检响应缓存很弱，因此预检响应的额外往返会损害性能。
Services used by interactive Web clients where performance is critical SHOULD avoid patterns that cause a preflight request
注重性能端的交互式 Web客户端使用的服务端应该避免使用导致预检的请求。

- 对于GET和HEAD调用，请避免要求不属于上述简单集的请求标头。最好是允许将它们作为查询参数提供。
  - Authorization标头不是简单集的一部分，因此对于需要验证的资源，必须通过“access_token”查询参数发送验证令牌。请注意，不建议在URL中传递身份验证令牌，因为它可能导致令牌记录在服务器日志中，并暴露给有权访问这些日志的任何人。通过URL接受身份验证令牌的服务必须采取措施来降低安全风险，例如使用短期身份验证令牌，禁止记录身份验证令牌以及控制对服务器日志的访问。

- 避免要求cookie。如果设置了“withCredentials”属性，XmlHttpRequest将仅在跨域请求上发送cookie; 这也会导致预检请求。
  - 需要基于cookie的身份验证的服务必须使用“动态验证码（dynamic canary）”。注: 服务器生成某种验证码，客户端获取后，服务器再进行验证的操作。来保护所有接受cookie的API。

- 对于POST调用，在适用的情况下，选择简单的内容类型(“application/x-www-form-urlencoded”、“multipart/form-data”、“text/plain”)。其他任何内容类型都会引发预检请求。
  - 服务不得以避免CORS预检请求的名义违反其他API指南。由于内容类型的原因，大多数POST请求实际上需要预检请求。
  - 如果非要取消预检工作，那么服务支持的其他的替代数据传输机制必须遵循本指南。

此外，当适当的服务可以支持JSONP模式时，只需简单的GET跨域访问。
在JSONP中，服务采用指示格式的参数($format=json)和表示回调的参数($callback=someFunc)，并返回一个 text/javascript 文档，其中包含用指定名称封装在函数调用中的JSON响应。
更多关于JSONP的信息，请访问 [JSONP](https://en.wikipedia.org/wiki/JSONP).

## 9. Collections 集合

### 9.1. Item keys 主键

Services MAY support durable identifiers for each item in the collection, and that identifier SHOULD be represented in JSON as "id". These durable identifiers are often used as item keys.
服务可以支持集合中每个项的持久标识符(主键)，该标识符应用JSON表示为”id” , 这些持久标识符通常用作项目的key。

Collections that support durable identifiers MAY support delta queries.
支持持久标识符(主键)的集合可以支持增量查询。

### 9.2. Serialization 序列化

Collections are represented in JSON using standard array notation.
集合使用标准数组表示法以JSON表示。

### 9.3. Collection URL patterns 集合的URL模式

Collections are located directly under the service root when they are top level, or as a segment under another resource when scoped to that resource.
集合在顶级时直接位于服务的根目录下，或者作用于该资源时作为另一个资源下的段。

For example:
例如:

```http
GET https://api.contoso.com/v1.0/people
```

Whenever possible, services MUST support the "/" pattern.
服务必须尽可能支持“/” 匹配。

For example:
例如:

```http
GET https://{serviceRoot}/{collection}/{id}
```

Where:

- {serviceRoot} – 站点URL (site URL) + 服务的根路径的组合
- {collection} – 集合的名称，未缩写，复数
- {id} – 唯一id属性的值. 当使用 “/“ 匹配必须属于 string/number/guid value 不带引号，转义正确以适应URL。

#### 9.3.1. Nested collections and properties 嵌套集合和属性

Collection items MAY contain other collections.
集合可以包含其他集合。
For example, a user collection MAY contain user resources that have multiple addresses:
例如，用户集合可能包含多个地址的用户资源:

```http
GET https://api.contoso.com/v1.0/people/123/addresses
```

```json
{
  "value": [
    { "street": "1st Avenue", "city": "Seattle" },
    { "street": "124th Ave NE", "city": "Redmond" }
  ]
}
```

### 9.4. Big collections 大集合

As data grows, so do collections.
随着数据的增长，集合也在增长
Planning for pagination is important for all services.
所以计划采用分页对所有服务都很重要。
Therefore, when multiple pages are available, the serialization payload MUST contain the opaque URL for the next page as appropriate.
因此，当数据包含多页时，序列化有效负载(payload)必须适当地包含下一页的不透明URL。

Refer to the paging guidance for more details.
有关详细信息，请参阅分页指南。

Clients MUST be resilient to collection data being either paged or nonpaged for any given request.
客户端必须能够恰当的处理请求返回的任何给定的分页或非分页集合数据。

```json
{
  "value":[
    { "id": "Item 1","price": 99.95,"sizes": null},
    { … },
    { … },
    { "id": "Item 99","price": 59.99,"sizes": null}
  ],
  "@nextLink": "{opaqueUrl}"
}
```

### 9.5. Changing collections 变化的集合

POST requests are not idempotent.
POST请求不是幂等的。
This means that two POST requests sent to a collection resource with exactly the same payload MAY lead to multiple items being created in that collection.
这意味着发送到具有完全相同的有效负载(payload)的集合资源的两次POST请求可能导致在该集合中创建多个项。
This is often the case for insert operations on items with a server-side generated id.
例如，对于具有服务器端生成的id的项的插入操作，通常就是这种情况。

For example, the following request:
例如，以下请求:

```http
POST https://api.contoso.com/v1.0/people
```

Would lead to a response indicating the location of the new collection item:
会导致响应，指示新集合项的位置：

```http
201 Created
Location: https://api.contoso.com/v1.0/people/123
```

And once executed again, would likely lead to another resource:
一旦再次执行，可能会导致创建另一个资源：

```http
201 Created
Location: https://api.contoso.com/v1.0/people/124
```

While a PUT request would require the indication of the collection item with the corresponding key instead:
而PUT请求则需要使用相应的键来指示集合项:

```http
PUT https://api.contoso.com/v1.0/people/123
```

### 9.6. Sorting collections 可排序集合

The results of a collection query MAY be sorted based on property values.
可以基于属性值对集合查询的结果进行排序。
The property is determined by the value of the _$orderBy_ query parameter.
该属性由_$orderBy_查询参数的值确定。

The value of the _$orderBy_ parameter contains a comma-separated list of expressions used to sort the items.
$orderBy 参数的值包含用于对项目进行排序表达式列表，用逗号分隔的。
A special case of such an expression is a property path terminating on a primitive property.
这种表达式的特殊情况是属性路径终止于基本属性。

The expression MAY include the suffix "asc" for ascending or "desc" for descending, separated from the property name by one or more spaces.
表达式可以包含升序的后缀“asc”或降序的后缀“desc”，它们与属性名之间用一个或多个空格分隔。
If "asc" or "desc" is not specified, the service MUST order by the specified property in ascending order.
如果没有指定“asc”或“desc”，则服务必须按照指定的属性以升序排序。

NULL values MUST sort as "less than" non-NULL values.
空值(NULL)必须排序为“小于”非空值。

Items MUST be sorted by the result values of the first expression, and then items with the same value for the first expression are sorted by the result value of the second expression, and so on.
必须根据第一个表达式的结果值对项进行排序，然后根据第二个表达式的结果值对第一个表达式具有相同值的项进行排序，以此类推。
The sort order is the inherent order for the type of the property.
排序顺序是属性类型的固有顺序。

For example:
例如:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name
```

Will return all people sorted by name in ascending order.
将返回按name进行升序排序的所有人员。

For example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc
```

Will return all people sorted by name in descending order.
将返回按name进行降序排序的所有人。

Sub-sorts can be specified by a comma-separated list of property names with OPTIONAL direction qualifier.
可以通过逗号分隔的属性名称列表以及可选方向限定符来指定子排序。

For example:
例如:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc,hireDate
```

Will return all people sorted by name in descending order and a secondary sort order of hireDate in ascending order.
将返回按姓名降序排列的所有人员，并按雇佣日期降序排列的次要排序。

Sorting MUST compose with filtering such that:
排序必须与筛选相结合，如下:

```http
GET https://api.contoso.com/v1.0/people?$filter=name eq 'david'&$orderBy=hireDate
```

Will return all people whose name is David sorted in ascending order by hireDate.
将返回所有名称为David的人，按雇佣日期按升序排列。

#### 9.6.1. Interpreting a sorting expression

Sorting parameters MUST be consistent across pages, as both client and server-side paging is fully compatible with sorting.
跨页面的排序参数必须一致，因为客户端和服务器端分页都依赖该排序该参数进行排序。

If a service does not support sorting by a property named in a _$orderBy_ expression, the service MUST respond with an error message as defined in the Responding to Unsupported Requests section.
如果服务不支持按_$orderBy_表达式中命名的属性排序，则服务必须按照“响应不支持的请求”部分中定义的错误消息进行响应。

### 9.7. Filtering 过滤

The _$filter_ querystring parameter allows clients to filter a collection of resources that are addressed by a request URL.
$filter_querystring 参数允许客户端通过URL过滤集合。
The expression specified with _$filter_ is evaluated for each resource in the collection, and only items where the expression evaluates to true are included in the response.
使用_$filter_指定的表达式将为集合中的每个资源求值，只有表达式求值为true的项才包含在响应中。
Resources for which the expression evaluates to false or to null, or which reference properties that are unavailable due to permissions, are omitted from the response.
表达式计算为false或null的资源，或由于权限而不可用的引用属性，将从响应中省略。

Example: return all Products whose Price is less than $10.00
例如:返回所有产品的价格低于10.00美元

```http
GET https://api.contoso.com/v1.0/products?$filter=price lt 10.00
```

The value of the _$filter_ option is a Boolean expression.
$filter_选项的值是 一个布尔表达式 表示 price less than 10.00。

#### 9.7.1. Filter operations 过滤操作

Services that support _$filter_ SHOULD support the following minimal set of operations.
支持_$filter_的服务应该支持以下最小操作集。

Operator             | Description           | Example
-------------------- | --------------------- | -----------------------------------------------------
Comparison Operators |                       |
eq                   | Equal                 | city eq 'Redmond'
ne                   | Not equal             | city ne 'London'
gt                   | Greater than          | price gt 20
ge                   | Greater than or equal | price ge 10
lt                   | Less than             | price lt 20
le                   | Less than or equal    | price le 100
Logical Operators    |                       |
and                  | Logical and           | price le 200 and price gt 3.5
or                   | Logical or            | price le 3.5 or price gt 200
not                  | Logical negation      | not price le 3.5
Grouping Operators   |                       |
( )                  | Precedence grouping   | (priority eq 1 or city eq 'Redmond') and price gt 100

#### 9.7.2. Operator examples 示例

The following examples illustrate the use and semantics of each of the logical operators.
下面的示例说明了每个逻辑操作符的用法和语义。

Example: all products with a name equal to 'Milk'
示例:所有名称等于“Milk”的产品

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk'
```

Example: all products with a name not equal to 'Milk'
示例:所有名称不等于“Milk”的产品

```http
GET https://api.contoso.com/v1.0/products?$filter=name ne 'Milk'
```

Example: all products with the name 'Milk' that also have a price less than 2.55:
示例:所有标有“Milk”的产品价格都低于2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' and price lt 2.55
```

Example: all products that either have the name 'Milk' or have a price less than 2.55:
示例:所有标有“Milk”字样或价格低于2.55美元的产品:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' or price lt 2.55
```

Example 54: all products that have the name 'Milk' or 'Eggs' and have a price less than 2.55:
示例:所有名称为“牛奶”或“鸡蛋”且价格低于2.55的产品:

```http
GET https://api.contoso.com/v1.0/products?$filter=(name eq 'Milk' or name eq 'Eggs') and price lt 2.55
```

#### 9.7.3. Operator precedence 操作符的优先级

Services MUST use the following operator precedence for supported operators when evaluating _$filter_ expressions.
在计算_$filter_表达式时，服务使用以下操作符优先级。
Operators are listed by category in order of precedence from highest to lowest.
操作符按类别按优先级从高到低排列。
Operators in the same category have equal precedence:
同一类别的运算符具有同等优先级:

| Group           | Operator | Description           |
|:----------------|:---------|:----------------------|
| Grouping        | ( )      | Precedence grouping   |
| Unary           | not      | Logical Negation      |
| Relational      | gt       | Greater Than          |
|                 | ge       | Greater than or Equal |
|                 | lt       | Less Than             |
|                 | le       | Less than or Equal    |
| Equality        | eq       | Equal                 |
|                 | ne       | Not Equal             |
| Conditional AND | and      | Logical And           |
| Conditional OR  | or       | Logical Or            |

### 9.8. Pagination 分页

RESTful APIs that return collections MAY return partial sets.
返回集合的RESTful API可能返回部分集。
Consumers of these services MUST expect partial result sets and correctly page through to retrieve an entire set.
这些服务的消费者清楚将获得部分结果集，并能正确地翻页以检索整个结果集。

There are two forms of pagination that MAY be supported by RESTful APIs.
RESTful API可能支持两种形式的分页。
Server-driven paging mitigates against denial-of-service attacks by forcibly paginating a request over multiple response payloads.
服务器驱动的分页：通过在多个响应有效载荷上强制分页请求来减轻拒绝服务攻击。
Client-driven paging enables clients to request only the number of resources that it can use at a given time.
客户端驱动的分页：允许客户机只请求它在给定时间可以使用的资源数量。

Sorting and Filtering parameters MUST be consistent across pages, because both client- and server-side paging is fully compatible with both filtering and sorting.
跨页面的排序和筛选参数必须一致，因为客户端和服务器端分页都完全兼容于筛选和排序。

#### 9.8.1. Server-driven paging 服务器驱动的分页

Paginated responses MUST indicate a partial result by including a continuation token in the response.
分页响应必须通过在响应中包含延续分页标记来告诉客户端这是部分结果。
The absence of a continuation token means that no additional pages are available.
没有延续分页标记意味着没有下一页了。

Clients MUST treat the continuation URL as opaque, which means that query options may not be changed while iterating over a set of partial results.
客户端必须将延续URL视为不透明的，这意味着在迭代一组部分结果时，查询选项可能不会更改。

Example:

```http
GET http://api.contoso.com/v1.0/people HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  ...,
  "value": [...],
  "@nextLink": "{opaqueUrl}"
}
```

#### 9.8.2. Client-driven paging 客户端驱动的分页

Clients MAY use _$top_ and _$skip_ query parameters to specify a number of results to return and an offset into the collection.
客户端可以使用$top_和$skip_查询参数来指定返回的结果数量和跳过的集合数量。

The server SHOULD honor the values specified by the client; however, clients MUST be prepared to handle responses that contain a different page size or contain a continuation token.
服务器应遵守客户端指定的参数; 但是，客户端必须做好准备处理包含不同页面大小的响应或包含延续分页标记的响应

When both _$top_ and _$skip_ are given by a client, the server SHOULD first apply _$skip_ and then _$top_ on the collection.
当客户端同时提供$top_和$skip_时，服务器应该首先应用$skip_，然后对集合应用$top_。

Note: If the server can't honor _$top_ and/or _$skip_, the server MUST return an error to the client informing about it instead of just ignoring the query options.
注意:如果服务器不能执行$top_和/或$skip_，服务器必须返回一个错误给客户端，告知它，而不是忽略该查询参数。
This will avoid the risk of the client making assumptions about the data returned.
这将避免客户端对返回的数据做出假设的风险。

Example:

```http
GET http://api.contoso.com/v1.0/people?$top=5&$skip=2 HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  ...,
  "value": [...]
}
```

#### 9.8.3. Additional considerations 其他注意事项

**Stable order prerequisite:** Both forms of paging depend on the collection of items having a stable order.
The server MUST supplement any specified order criteria with additional sorts (typically by key) to ensure that items are always ordered consistently.
**固定的顺序先决条件:** 两种分页形式都依赖于具有固定顺序的项的集合。
服务器必须使用额外的排序(通常是按键排序)来补充任何指定的顺序标准，以确保项目始终保持一致的顺序。

**Missing/repeated results:** Even if the server enforces a consistent sort order, results MAY be missing or repeated based on creation or deletion of other resources.
Clients MUST be prepared to deal with these discrepancies.
**缺失/重复结果**：即使服务器强制执行一致的排序顺序，结果也可能会因创建或删除其他资源而导致丢失或重复。客户端必须准备好处理这些差异。
The server SHOULD always encode the record ID of the last read record, helping the client in the process of managing repeated/missing results.
服务器应该总是编码最后读取记录的记录ID，帮助客户端管理重复/丢失的结果。

**Combining client- and server-driven paging:** Note that client-driven paging does not preclude server-driven paging.
If the page size requested by the client is larger than the default page size supported by the server, the expected response would be the number of results specified by the client, paginated as specified by the server paging settings.
**结合客户端和服务驱动的分页：** 请注意，客户端驱动的分页不排除服务器驱动的分页。
如果客户端请求的页面大小大于服务器支持的默认页面大小，则预期响应将是客户端指定的结果数，否则按服务端分页设置的指定分页。

**Page Size:** Clients MAY request server-driven paging with a specific page size by specifying a _$maxpagesize_ preference.
**页面大小：**客户端可以通过指定_$maxpagesize_首选项来请求具有特定页面大小的服务端驱动的分页。
The server SHOULD honor this preference if the specified page size is smaller than the server's default page size.
如果指定的页面大小小于服务端的默认页面大小，服务器应该遵循此首选项。

**Paginating embedded collections:** It is possible for both client-driven paging and server-driven paging to be applied to embedded collections.
**分页嵌入式集合：**客户端驱动的分页和服务端驱动的分页都可以应用于嵌入式集合。
If a server paginates an embedded collection, it MUST include additional continuation tokens as appropriate.
如果服务端对嵌入式集合进行分页，则必须包含其他适当的延续分页标记。

**Recordset count:** Developers who want to know the full number of records across all pages, MAY include the query parameter _$count=true_ to tell the server to include the count of items in the response.
**记录集计数：**想要知道所有页面中的完整记录数的开发人员可以包含查询参数_$ count=true_，以告知服务端包含响应中的记录数。

### 9.9. Compound collection operations 集合复合操作

Filtering, Sorting and Pagination operations MAY all be performed against a given collection.
筛选、排序和分页操作都可以针对给定的集合执行。
When these operations are performed together, the evaluation order MUST be:
当这些操作一起执行时，评估顺序必须是:

1. **筛选**。这包括作为AND操作执行的所有范围表达式。
2. **排序**。可能已过滤的列表根据排序条件进行排序。
3. **分页**。经过筛选和排序的列表上显示了实现分页视图。这适用于服务器驱动的分页和客户端驱动的分页。

## 10. Delta queries 增量查询

Services MAY choose to support delta queries.
服务可以支持增量查询。

注：增量查询可以使客户端能够发现新创建、更新或者删除的实体，无需使用每个请求对目标资源执行完全读取。这让客户端的调用更加高效。

### 10.1. Delta links Delta链接

Delta links are opaque, service-generated links that the client uses to retrieve subsequent changes to a result.
Delta链接是不透明的、由服务生成的链接，客户端使用Delta链接对结果做后续修改。

At a conceptual level delta links are based on a defining query that describes the set of results for which changes are being tracked.
在概念层面上，delta链接基于一个定义查询，该查询描述正在跟踪更改的一组结果集。

The delta link encodes the collection of entities for which changes are being tracked, along with a starting point from which to track changes.
Delta链接编码正在跟踪变更的实体集合，以及跟踪更改的起点。

If the query contains a filter, the response MUST include only changes to entities matching the specified criteria.
如果查询包含筛选器，则响应必须只包含对匹配指定条件的实体的更改。
The key principles of the Delta Query are:
Delta查询的主要原则是:

- Every item in the set MUST have a persistent identifier. That identifier SHOULD be represented as "id". This identifier is a service defined opaque string that MAY be used by the client to track object across calls. 集合中的每个项目必须具有持久标识符（永久不变的主键）。该标识符应该表示为“id”。此标识符由服务定义，客户端可以使用该字符串跨调用跟踪对象。
- The delta MUST contain an entry for each entity that newly matches the specified criteria, and MUST contain a "@removed" entry for each entity that no longer matches the criteria. delta 必须包含每个与指定条件新匹配的实体的条目，并且必须为每个不再符合条件的实体包含“@removed”条目。
- Re-evaluate the query and compare it to original set of results; every entry uniquely in the current set MUST be returned as an Add operation, and every entry uniquely in the original set MUST be returned as a "remove" operation. 重新调用查询并将其与原始结果集进行比较; 必须将当前集合中惟一的每个条目作为”add”操作返回，并且必须将原始集合中惟一的每个条目作为“remove”操作返回。。
- Each entity that previously did not match the criteria but matches it now MUST be returned as an "add"; conversely, each entity that previously matched the query but no longer does MUST be returned as a "@removed" entry. 以前与标准不匹配但现在匹配的每个实体必须作为”add”返回; 相反，先前与查询匹配但不再必须返回的每个实体必须作为“@removed”条目返回。
- Entities that have changed MUST be included in the set using their standard representation. 已更改的实体必须使用其标准表示形式包含在集合中。
- Services MAY add additional metadata to the "@removed" node, such as a reason for removal, or a "removed at" timestamp. We recommend teams coordinate with the Microsoft REST API Guidelines Working Group on extensions to help maintain consistency. 服务可以向“@remove”节点添加额外的元数据，例如删除的原因或“removed at”时间戳。我们建议团队与Microsoft REST API指导原则工作组协调，以帮助维护一致性。

The delta link MUST NOT encode any client top or skip value.
Delta链接不能编码任何客户端 top 或 skip 值。

### 10.2. Entity representation

Added and updated entities are represented in the entity set using their standard representation.
添加和更新的实体使用其标准表示在实体集中表示。
From the perspective of the set, there is no difference between an added or updated entity.
从集合的角度来看，添加或更新的实体之间没有区别。

Removed entities are represented using only their "id" and an "@removed" node.
删除的实体仅使用其“id”和“@removed”节点表示。
The presence of an "@removed" node MUST represent the removal of the entry from the set.
“@removed”节点的存在必须表示从集合中删除条目。

### 10.3. Obtaining a delta link 获取delta链接

A delta link is obtained by querying a collection or entity and appending a $delta query string parameter.
通过查询集合或实体并附加 $delta 查询字符串参数来获得 Delta 链接。
For example:

```http
GET https://api.contoso.com/v1.0/people?$delta
HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  "value":[
    { "id": "1", "name": "Matt"},
    { "id": "2", "name": "Mark"},
    { "id": "3", "name": "John"}
  ],
  "@deltaLink": "{opaqueUrl}"
}
```

Note: If the collection is paginated the deltaLink will only be present on the final page but MUST reflect any changes to the data returned across all pages.
注意:如果集合分页，deltaLink将只出现在最后一页，但必须反映对所有页面返回的数据的任何更改。

### 10.4. Contents of a delta link response delta链接的响应内容

Added/Updated entries MUST appear as regular JSON objects, with regular item properties.
添加/更新的条目必须以常规JSON对象的形式出现，并带有常规项目属性。
Returning the added/modified items in their regular representation allows the client to merge them into their existing "cache" using standard merge concepts based on the "id" field.
在常规表示中返回添加/修改的项，允许客户端使用基于“id”字段的标准合并概念将它们合并到现有的“缓存”中。

Entries removed from the defined collection MUST be included in the response.
从定义的集合中删除的条目必须包含在响应中。
Items removed from the set MUST be represented using only their "id" and an "@removed" node.
从集合中删除的项必须仅使用它们的“id”和“@remove”节点表示。

### 10.5. Using a delta link 使用 dela链接

The client requests changes by invoking the GET method on the delta link.
客户端通过调用delta链接上的GET方法请求更改。
The client MUST use the delta URL as is -- in other words the client MUST NOT modify the URL in any way (e.g., parsing it and adding additional query string parameters).
客户端必须按原样使用delta URL——换句话说，客户端不能以任何方式修改URL(例如，解析URL并添加额外的查询字符串参数)。
In this example:
例子:

```http
GET https://{opaqueUrl} HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  "value":[
    { "id": "1", "name": "Mat"},
    { "id": "2", "name": "Marc"},
    { "id": "3", "@removed": {} },
    { "id": "4", "name": "Luc"}
  ],
  "@deltaLink": "{opaqueUrl}"
}
```

The results of a request against the delta link may span multiple pages but MUST be ordered by the service across all pages in such a way as to ensure a deterministic result when applied in order to the response that contained the delta link.
针对delta链接的请求的结果可以跨多个页面，但是必须由服务跨所有页面进行排序，以便在应用到包含delta链接的响应时确保得到确定的结果。

If no changes have occurred, the response is an empty collection that contains a delta link for subsequent changes if requested.
如果没有发生任何更改，则响应是一个空集合，其中包含一个delta链接，用于根据请求进行后续更改。
This delta link MAY be identical to the delta link resulting in the empty collection of changes.
这个delta链接可能与delta链接相同，从而导致更改的空集合。

If the delta link is no longer valid, the service MUST respond with _410 Gone_. The response SHOULD include a Location header that the client can use to retrieve a new baseline set of results.
如果delta链接不再有效，则服务必须使用_410 Gone_响应。响应应该包含一个Location头，客户端可以使用它来检索新的基线结果集。

## 11. JSON standardizations

### 11.1. JSON formatting standardization for primitive types

Primitive values MUST be serialized to JSON following the rules of [RFC8259][rfc-8259].

**Important note for 64bit integers:** JavaScript will silently truncate integers larger than `Number.MAX_SAFE_INTEGER` (2^53-1) or numbers smaller than `Number.MIN_SAFE_INTEGER` (-2^53+1). If the service is expected to return integer values outside the range of safe values, strongly consider returning the value as a string in order to maximize interoperability and avoid data loss.

### 11.2. Guidelines for dates and times

#### 11.2.1. Producing dates

Services MUST produce dates using the `DateLiteral` format, and SHOULD use the `Iso8601Literal` format unless there are compelling reasons to do otherwise.
Services that do use the `StructuredDateLiteral` format MUST NOT produce dates using the `T` kind unless BOTH the additional precision is REQUIRED, and ECMAScript clients are explicitly unsupported.
(Non-Normative statement: When deciding which particular `DateKind` to standardize on, the approximate order of preference is `E, C, U, W, O, X, I, T`.
This optimizes for ECMAScript, .NET, and C++ programmers, in that order.)

#### 11.2.2. Consuming dates

Services MUST accept dates from clients that use the same `DateLiteral` format (including the `DateKind`, if applicable) that they produce, and SHOULD accept dates using any `DateLiteral` format.

#### 11.2.3. Compatibility

Services MUST use the same `DateLiteral` format (including the same `DateKind`, if applicable) for all resources of the same type, and SHOULD use the same `DateLiteral` format (and `DateKind`, if applicable) for all resources across the entire service.

Any change to the `DateLiteral` format produced by the service (including the `DateKind`, if applicable) and any reductions in the `DateLiteral` formats (and `DateKind`, if applicable) accepted by the service MUST be treated as a breaking change.
Any widening of the `DateLiteral` formats accepted by the service is NOT considered a breaking change.

### 11.3. JSON serialization of dates and times

Round-tripping serialized dates with JSON is a hard problem.
Although ECMAScript supports literals for most built-in types, it does not define a literal format for dates.
The Web has coalesced around the [ECMAScript subset of ISO 8601 date formats (ISO 8601)][iso-8601], but there are situations where this format is not desirable.
For those cases, this document defines a JSON serialization format that can be used to unambiguously represent dates in different formats.
Other serialization formats (such as XML) could be derived from this format.

#### 11.3.1. The `DateLiteral` format

Dates represented in JSON are serialized using the following grammar.
Informally, a `DateValue` is either an ISO 8601-formatted string or a JSON object containing two properties named `kind` and `value` that together define a point in time.
The following is not a context-free grammar; in particular, the interpretation of `DateValue` depends on the value of `DateKind`, but this minimizes the number of productions required to describe the format.

```text
DateLiteral:
  Iso8601Literal
  StructuredDateLiteral

Iso8601Literal:
  A string literal as defined in http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15. Note that the full grammar for ISO 8601 (such as "basic format" without separators) is not supported.
  All dates default to UTC unless specified otherwise.

StructuredDateLiteral:
  { DateKindProperty , DateValueProperty }
  { DateValueProperty , DateKindProperty }

DateKindProperty
  "kind" : DateKind

DateKind:
  "C"            ; see below
  "E"            ; see below
  "I"            ; see below
  "O"            ; see below
  "T"            ; see below
  "U"            ; see below
  "W"            ; see below
  "X"            ; see below

DateValueProperty:
  "value" : DateValue

DateValue:
  UnsignedInteger        ; not defined here
  SignedInteger        ; not defined here
  RealNumber        ; not defined here
  Iso8601Literal        ; as above
```

#### 11.3.2. Commentary on date formatting

A `DateLiteral` using the `Iso8601Literal` production is relatively straightforward.
Here is an example of an object with a property named `creationDate` that is set to February 13, 2015, at 1:15 p.m. UTC:

```json
{ "creationDate" : "2015-02-13T13:15Z" }
```

The `StructuredDateLiteral` consists of a `DateKind` and an accompanying `DateValue` whose valid values (and their interpretation) depend on the `DateKind`. The following table describes the valid combinations and their meaning:

DateKind | DateValue       | Colloquial Name & Interpretation                                                                                                                  | More Info
-------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------
C        | UnsignedInteger | "CLR"; number of milliseconds since midnight January 1, 0001; negative values are not allowed. *See note below.*                                  | [MSDN][clr-time]
E        | SignedInteger   | "ECMAScript"; number of milliseconds since midnight, January 1, 1970.                                                                             | [ECMA International][ecmascript-time]
I        | Iso8601Literal  | "ISO 8601"; a string limited to the ECMAScript subset.                                                                                            |
O        | RealNumber      | "OLE Date"; integral part is the number of days since midnight, December 31, 1899, and fractional part is the time within the day (0.5 = midday). | [MSDN][ole-date]
T        | SignedInteger   | "Ticks"; number of ticks (100-nanosecond intervals) since midnight January 1, 1601. *See note below.*                                             | [MSDN][ticks-time]
U        | SignedInteger   | "UNIX"; number of seconds since midnight, January 1, 1970.                                                                                        | [MSDN][unix-time]
W        | SignedInteger   | "Windows"; number of milliseconds since midnight January 1, 1601. *See note below.*                                                               | [MSDN][windows-time]
X        | RealNumber      | "Excel"; as for `O` but the year 1900 is incorrectly treated as a leap year, and day 0 is "January 0 (zero)."                                     | [Microsoft Support][excel-time]

**Important note for `C` and `W` kinds:** The native CLR and Windows times are represented by 100-nanosecond "tick" values.
To interoperate with ECMAScript clients that have limited precision, _these values MUST be converted to and from milliseconds_ when (de)serialized as a `DateLiteral`.
One millisecond is equivalent to 10,000 ticks.

**Important note for `T` kind:** This kind preserves the full fidelity of the Windows native time formats (and is trivially convertible to and from the native CLR format) but is incompatible with ECMAScript clients.
Therefore, its use SHOULD be limited to only those scenarios that both require the additional precision and do not need to interoperate with ECMAScript clients.

Here is the same example of an object with a property named creationDate that is set to February 13, 2015, at 1:15 p.m. UTC, using several formats:

```json
[
  { "creationDate" : { "kind" : "O", "value" : 42048.55 } },
  { "creationDate" : { "kind" : "E", "value" : 1423862100000 } }
]
```

One of the benefits of separating the kind from the value is that once a client knows the kind used by a particular service, it can interpret the value without requiring any additional parsing.
In the common case of the value being a number, this makes coding easier for developers:

```csharp
// We know this service always gives out ECMAScript-format dates
var date = new Date(serverResponse.someObject.creationDate.value);
```

### 11.4. Durations

[Durations][wikipedia-iso8601-durations] need to be serialized in conformance with [ISO 8601][wikipedia-iso8601-durations].
Durations are "represented by the format `P[n]Y[n]M[n]DT[n]H[n]M[n]S`."
From the standard:

- P is the duration designator (historically called "period") placed at the start of the duration representation.
- Y is the year designator that follows the value for the number of years.
- M is the month designator that follows the value for the number of months.
- W is the week designator that follows the value for the number of weeks.
- D is the day designator that follows the value for the number of days.
- T is the time designator that precedes the time components of the representation.
- H is the hour designator that follows the value for the number of hours.
- M is the minute designator that follows the value for the number of minutes.
- S is the second designator that follows the value for the number of seconds.

For example, "P3Y6M4DT12H30M5S" represents a duration of "three years, six months, four days, twelve hours, thirty minutes, and five seconds."

### 11.5. Intervals

[Intervals][wikipedia-iso8601-intervals] are defined as part of [ISO 8601][wikipedia-iso8601-intervals].

- Start and end, such as "2007-03-01T13:00:00Z/2008-05-11T15:30:00Z"
- Start and duration, such as "2007-03-01T13:00:00Z/P1Y2M10DT2H30M"
- Duration and end, such as "P1Y2M10DT2H30M/2008-05-11T15:30:00Z"
- Duration only, such as "P1Y2M10DT2H30M," with additional context information

### 11.6. Repeating intervals

[Repeating Intervals][wikipedia-iso8601-repeatingintervals], as per [ISO 8601][wikipedia-iso8601-repeatingintervals], are:

> Formed by adding "R[n]/" to the beginning of an interval expression, where R is used as the letter itself and [n] is replaced by the number of repetitions.
Leaving out the value for [n] means an unbounded number of repetitions.

For example, to repeat the interval of "P1Y2M10DT2H30M" five times starting at "2008-03-01T13:00:00Z," use "R5/2008-03-01T13:00:00Z/P1Y2M10DT2H30M."

## 12. Versioning

**All APIs compliant with the Microsoft REST API Guidelines MUST support explicit versioning.** It's critical that clients can count on services to be stable over time, and it's critical that services can add features and make changes.

### 12.1. Versioning formats

Services are versioned using a Major.Minor versioning scheme.
Services MAY opt for a "Major" only version scheme in which case the ".0" is implied and all other rules in this section apply.
Two options for specifying the version of a REST API request are supported:

- Embedded in the path of the request URL, at the end of the service root: `https://api.contoso.com/v1.0/products/users`
- As a query string parameter of the URL: `https://api.contoso.com/products/users?api-version=1.0`

Guidance for choosing between the two options is as follows:

1. Services co-located behind a DNS endpoint MUST use the same versioning mechanism.
2. In this scenario, a consistent user experience across the endpoint is paramount. The Microsoft REST API Guidelines Working Group recommends that new top-level DNS endpoints are not created without explicit conversations with your organization's leadership team.
3. Services that guarantee the stability of their REST API's URL paths, even through future versions of the API, MAY adopt the query string parameter mechanism. This means the naming and structure of the relationships described in the API cannot evolve after the API ships, even across versions with breaking changes.
4. Services that cannot ensure URL path stability across future versions MUST embed the version in the URL path.

Certain bedrock services such as Microsoft's Azure Active Directory may be exposed behind multiple endpoints.
Such services MUST support the versioning mechanisms of each endpoint, even if that means supporting multiple versioning mechanisms.

#### 12.1.1. Group versioning

Group versioning is an OPTIONAL feature that MAY be offered on services using the query string parameter mechanism.
Group versions allow for logical grouping of API endpoints under a common versioning moniker.
This allows developers to look up a single version number and use it across multiple endpoints.
Group version numbers are well known, and services SHOULD reject any unrecognized values.

Internally, services will take a Group Version and map it to the appropriate Major.Minor version.

The Group Version format is defined as YYYY-MM-DD, for example 2012-12-07 for December 7, 2012. This Date versioning format applies only to Group Versions and SHOULD NOT be used as an alternative to Major.Minor versioning.

##### Examples of group versioning

| Group      | Major.Minor |
|:-----------|:------------|
| 2012-12-01 | 1.0         |
|            | 1.1         |
|            | 1.2         |
| 2013-03-21 | 1.0         |
|            | 2.0         |
|            | 3.0         |
|            | 3.1         |
|            | 3.2         |
|            | 3.3         |

Version Format                | Example                | Interpretation
----------------------------- | ---------------------- | ------------------------------------------
{groupVersion}                | 2013-03-21, 2012-12-01 | 3.3, 1.2
{majorVersion}                | 3                      | 3.0
{majorVersion}.{minorVersion} | 1.2                    | 1.2

Clients can specify either the group version or the Major.Minor version:

For example:

```http
GET http://api.contoso.com/acct1/c1/blob2?api-version=1.0
```

```http
PUT http://api.contoso.com/acct1/c1/b2?api-version=2011-12-07
```

### 12.2. When to version

Services MUST increment their version number in response to any breaking API change.
See the following section for a detailed discussion of what constitutes a breaking change.
Services MAY increment their version number for nonbreaking changes as well, if desired.

Use a new major version number to signal that support for existing clients will be deprecated in the future.
When introducing a new major version, services MUST provide a clear upgrade path for existing clients and develop a plan for deprecation that is consistent with their business group's policies.
Services SHOULD use a new minor version number for all other changes.

Online documentation of versioned services MUST indicate the current support status of each previous API version and provide a path to the latest version.

### 12.3. Definition of a breaking change

Changes to the contract of an API are considered a breaking change.
Changes that impact the backwards compatibility of an API are a breaking change.

Teams MAY define backwards compatibility as their business needs require.
For example, Azure defines the addition of a new JSON field in a response to be not backwards compatible.
Office 365 has a looser definition of backwards compatibility and allows JSON fields to be added to responses.

Clear examples of breaking changes:

1. Removing or renaming APIs or API parameters
2. Changes in behavior for an existing API
3. Changes in Error Codes and Fault Contracts
4. Anything that would violate the [Principle of Least Astonishment][principle-of-least-astonishment]

Services MUST explicitly define their definition of a breaking change, especially with regard to adding new fields to JSON responses and adding new API arguments with default fields.
Services that are co-located behind a DNS Endpoint with other services MUST be consistent in defining contract extensibility.

The applicable changes described [in this section of the OData V4 spec][odata-breaking-changes] SHOULD be considered part of the minimum bar that all services MUST consider a breaking change.

## 13. Long running operations

Long running operations, sometimes called async operations, tend to mean different things to different people.
This section sets forth guidance around different types of long running operations, and describes the wire protocols and best practices for these types of operations.

1. One or more clients MUST be able to monitor and operate on the same resource at the same time.
2. The state of the system SHOULD be discoverable and testable at all times. Clients SHOULD be able to determine the system state even if the operation tracking resource is no longer active. The act of querying the state of a long running operation should itself leverage principles of the web. i.e. well-defined resources with uniform interface semantics. Clients MAY issue a GET on some resource to determine the state of a long running operation
3. Long running operations SHOULD work for clients looking to "Fire and Forget" and for clients looking to actively monitor and act upon results.
4. Cancellation does not explicitly mean rollback. On a per-API defined case it may mean rollback, or compensation, or completion, or partial completion, etc. Following a cancelled operation, It SHOULD NOT be a client's responsibility to return the service to a consistent state which allows continued service.

### 13.1. Resource based long running operations (RELO)

Resource based modeling is where the status of an operation is encoded in the resource and the wire protocol used is the standard synchronous protocol.
In this model state transitions are well defined and goal states are similarly defined.

_This is the preferred model for long running operations and should be used wherever possible._ Avoiding the complexity and mechanics of the LRO Wire Protocol makes things simpler for our users and tooling chain.

An example may be a machine reboot, where the operation itself completes synchronously but the GET operation on the virtual machine resource would have a "state: Rebooting", "state: Running" that could be queried at any time.

This model MAY integrate Push Notifications.

While most operations are likely to be POST semantics, in addition to POST semantics, services MAY support PUT semantics via routing to simplify their APIs.
For example, a user that wants to create a database named "db1" could call:

```http
PUT https://api.contoso.com/v1.0/databases/db1
```

In this scenario the databases segment is processing the PUT operation.

Services MAY also use the hybrid defined below.

### 13.2. Stepwise long running operations

A stepwise operation is one that takes a long, and often unpredictable, length of time to complete, and doesn't offer state transition modeled in the resource.
This section outlines the approach that services should use to expose such long running operations.

Service MAY expose stepwise operations.

> Stepwise Long Running Operations are sometimes called "Async" operations.
This causes confusion, as it mixes elements of platforms ("Async / await", "promises", "futures") with elements of API operation.
This document uses the term "Stepwise Long Running Operation" or often just "Stepwise Operation" to avoid confusion over the word "Async".

Services MUST perform as much synchronous validation as practical on stepwise requests.
Services MUST prioritize returning errors in a synchronous way, with the goal of having only "Valid" operations processed using the long running operation wire protocol.

For an API that's defined as a Stepwise Long Running Operation the service MUST go through the Stepwise Long Running Operation flow even if the operation can be completed immediately.
In other words, APIs must adopt and stick with an LRO pattern and not change patterns based on circumstance.

#### 13.2.1. PUT

Services MAY enable PUT requests for entity creation.

```http
PUT https://api.contoso.com/v1.0/databases/db1
```

In this scenario the _databases_ segment is processing the PUT operation.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

For services that need to return a 201 Created here, use the hybrid flow described below.

The 202 Accepted should return no body.
The 201 Created case should return the body of the target resource.

#### 13.2.2. POST

Services MAY enable POST requests for entity creation.

```http
POST https://api.contoso.com/v1.0/databases/

{
  "fileName": "someFile.db",
  "color": "red"
}
```

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

#### 13.2.3. POST, hybrid model

Services MAY respond synchronously to POST requests to collections that create a resource even if the resources aren't fully created when the response is generated.
In order to use this pattern, the response MUST include a representation of the incomplete resource and an indication that it is incomplete.

For example:

```http
POST https://api.contoso.com/v1.0/databases/ HTTP/1.1
Host: api.contoso.com
Content-Type: application/json
Accept: application/json

{
  "fileName": "someFile.db",
  "color": "red"
}
```

Service response says the database has been created, but indicates the request is not completed by including the Operation-Location header.
In this case the status property in the response payload also indicates the operation has not fully completed.

```http
HTTP/1.1 201 Created
Location: https://api.contoso.com/v1.0/databases/db1
Operation-Location: https://api.contoso.com/v1.0/operations/123

{
  "databaseName": "db1",
  "color": "red",
  "Status": "Provisioning",
  [ … other fields for "database" …]
}
```

#### 13.2.4. Operations resource

Services MAY provide a "/operations" resource at the tenant level.

Services that provide the "/operations" resource MUST provide GET semantics.
GET MUST enumerate the set of operations, following standard pagination, sorting, and filtering semantics.
The default sort order for this operation MUST be:

Primary Sort           | Secondary Sort
---------------------- | -----------------------
Not Started Operations | Operation Creation Time
Running Operations     | Operation Creation Time
Completed Operations   | Operation Creation Time

Note that "Completed Operations" is a goal state (see below), and may actually be any of several different states such as "successful", "cancelled", "failed" and so forth.

#### 13.2.5. Operation resource

An operation is a user addressable resource that tracks a stepwise long running operation.
Operations MUST support GET semantics.
The GET operation against an operation MUST return:

1. The operation resource, it's state, and any extended state relevant to the particular API.
2. 200 OK as the response code.

Services MAY support operation cancellation by exposing DELETE on the operation.
If supported DELETE operations MUST be idempotent.

> Note: From an API design perspective, cancellation does not explicitly mean rollback.
On a per-API defined case it may mean rollback, or compensation, or completion, or partial completion, etc.
Following a cancelled operation, It SHOULD NOT be a client's responsibility to return the service to a consistent state which allows continued service.

Services that do not support operation cancellation MUST return a 405 Method Not Allowed in the event of a DELETE.

Operations MUST support the following states:

1. NotStarted
2. Running
3. Succeeded. Terminal State.
4. Failed. Terminal State.

Services MAY add additional states, such as "Cancelled" or "Partially Completed". Services that support cancellation MUST sufficiently describe their cancellation such that the state of the system can be accurately determined, and any compensating actions may be run.

Services that support additional states should consider this list of canonical names and avoid creating new names if possible: Cancelling, Cancelled, Aborting, Aborted, Tombstone, Deleting, Deleted.

An operation MUST contain, and provide in the GET response, the following information:

1. The timestamp when the operation was created.
2. A timestamp for when the current state was entered.
3. The operation state (notstarted / running / completed).

Services MAY add additional, API specific, fields into the operation.
The operation status JSON returned looks like:

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-01-03.45Z",
  "status": "notstarted | running | succeeded | failed"
}
```

##### Percent complete

Sometimes it is impossible for services to know with any accuracy when an operation will complete.
Which makes using the Retry-After header problematic.
In that case, services MAY include, in the operationStatus JSON, a percent complete field.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "percentComplete": "50",
  "status": "running"
}
```

In this example the server has indicated to the client that the long running operation is 50% complete.

##### Target resource location

For operations that result in, or manipulate, a resource the service MUST include the target resource location in the status upon operation completion.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://api.contoso.com/v1.0/databases/db1"
}
```

#### 13.2.6. Operation tombstones

Services MAY choose to support tombstoned operations.
Services MAY choose to delete tombstones after a service defined period of time.

#### 13.2.7. The typical flow, polling

- Client invokes a stepwise operation by invoking an action using POST
- The server MUST indicate the request has been started by responding with a 202 Accepted status code. The response SHOULD include the location header containing a URL that the client should poll for the results after waiting the number of seconds specified in the Retry-After header.
- Client polls the location until receiving a 200 response with a terminal operation state.

##### Example of the typical flow, polling

Client invokes the restart action:

```http
POST https://api.contoso.com/v1.0/databases HTTP/1.1
Accept: application/json

{
  "fromFile": "myFile.db",
  "color": "red"
}
```

The server response indicates the request has been created.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

Client waits for a period of time then invokes another request to try to get the operation status.

```http
GET https://api.contoso.com/v1.0/operations/123
Accept: application/json
```

Server responds that results are still not ready and optionally provides a recommendation to wait 30 seconds.

```http
HTTP/1.1 200 OK
Retry-After: 30

{
  "createdDateTime": "2015-06-19T12-01-03.4Z",
  "status": "running"
}
```

Client waits the recommended 30 seconds and then invokes another request to get the results of the operation.

```http
GET https://api.contoso.com/v1.0/operations/123
Accept: application/json
```

Server responds with a "status:succeeded" operation that includes the resource location.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://api.contoso.com/v1.0/databases/db1"
}
```

#### 13.2.8. The typical flow, push notifications

1. Client invokes a long running operation by invoking an action using POST. The client has a push notification already setup on the parent resource.
2. The service indicates the request has been started by responding with a 202 Accepted status code. The client ignores everything else.
3. Upon completion of the overall operation the service pushes a notification via the subscription on the parent resource.
4. The client retrieves the operation result via the resource URL.

##### Example of the typical flow, push notifications existing subscription

Client invokes the backup action.
The client already has a push notification subscription setup for db1.

```http
POST https://api.contoso.com/v1.0/databases/db1?backup HTTP/1.1
Accept: application/json
```

The server response indicates the request has been accepted.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

The caller ignores all the headers in the return.

The target URL receives a push notification when the operation is complete.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "value": [
    {
      "subscriptionId": "1234-5678-1111-2222",
      "context": "subscription context that was specified at setup",
      "resourceUrl": "https://api.contoso.com/v1.0/databases/db1",
      "userId" : "contoso.com/user@contoso.com",
      "tenantId" : "contoso.com"
    }
  ]
}
```

#### 13.2.9. Retry-After

In the examples above the Retry-After header indicates the number of seconds that the client should wait before trying to get the result from the URL identified by the location header.

The HTTP specification allows the Retry-After header to alternatively specify a HTTP date, so clients should be prepared to handle this as well.

```http
HTTP/1.1 202 Accepted
Operation-Location: http://api.contoso.com/v1.0/operations/123
Retry-After: 60
```

Note: The use of the HTTP Date is inconsistent with the use of ISO 8601 Date Format used throughout this document, but is explicitly defined by the HTTP standard in [RFC 7231][rfc-7231-7-1-1-1]. Services SHOULD prefer the integer number of seconds (in decimal) format over the HTTP date format.

### 13.3. Retention policy for operation results

In some situations, the result of a long running operation is not a resource that can be addressed.
For example, if you invoke a long running Action that returns a Boolean (rather than a resource).
In these situations, the Location header points to a place where the Boolean result can be retrieved.

Which begs the question: "How long should operation results be retained?"

A recommended minimum retention time is 24 hours.

Operations SHOULD transition to "tombstone" for an additional period of time prior to being purged from the system.

## 14. Throttling, Quotas, and Limits

### 14.1. Principles

Services should be as responsive as possible, so as not to block callers.
As a rule of thumb any API call that is expected to take longer than 0.5 seconds in the 99th percentile, should consider using the Long-running Operations pattern for those calls.
Obviously, services cannot guarantee these response times in the face of potentially unlimited load from callers. Services should therefore design and document call request limits for clients, and respond with appropriate, actionable errors and error messages if these limits are exceeded.
Services should respond quickly with an error when they are generally overloaded, rather than simply respond slowly.
Finally, many services will have quotas on calls, perhaps a number of operations per hour or day, usually related to a service plan or price.
When these quotas are exceeded services must also provide immediate, actionable errors.
Quotas and Limits should be scoped to a customer unit: a subscription, a tenant, an application, a plan, or without any other identification a range of ip addresses…as appropriate to the service goals so that the load is properly shared and one unit is not interfering with another.

### 14.2. Return Codes (429 vs 503)

HTTP specifies two return codes for these scenarios: '429 Too Many Requests' and '503 Service Unavailable'.
Services should use 429 for cases where clients are making too many calls and can fix the situation by changing their call pattern.
Services should respond with 503 in cases where general load or other problems outside the control of the individual callers is responsible for the service becoming slow.
In all cases, services should also provide information suggesting how long the callers should wait before trying in again.
Clients should respect these headers and also implement other transient fault handling techniques.
However, there may be clients that simply retry immediately upon failure, potentially increasing the load on the service.
To handle this, services should design so that returning 429 or 503 is as inexpensive as possible, either by putting in special fastpath code, or ideally by depending on a common frontdoor or load balancer that provides this functionality.

### 14.3. Retry-After and RateLimit Headers

The Retry-After header is the standard way for responding to clients who are being throttled.
It is also common, but optional, in the case of limits and quotas (but not overall system load) to respond with header describing the limit that was exceeded.
However, services across Microsoft and the industry use a wide range of different headers for this purpose.
We recommend using three headers to describe the limit, the number of calls remaining under the limit, and the time when the limit will reset.
However, other headers may be appropriate for specific types of limits. In all cases these must be documented.

### 14.4. Service Guidance

Services should choose time windows as appropriate for the SLAs or business objectives.
In the case of Quotas, the Retry-After time and time window may be very long (hours, days, weeks, even months. Services use 429 to indicate the specific caller has made too many calls, and 503 to indicate that the service is load shedding but that it is not the caller’s responsibility.

#### 14.4.1. Responsiveness

1. Services MUST respond quickly in all circumstances, even when under load.
2. Calls that take longer than 1s to respond in the 99th percentile SHOULD use the Long-Running Operation pattern
3. Calls that take longer than 0.5s to respond in the 99th percentile should strongly consider the LRO pattern
4. Services SHOULD NOT introduce sleeps, pauses, etc. that block callers or are not actionable (“tar-pitting”).

#### 14.4.2. Rate Limits and Quotas

When a caller has made too many calls

1. Services MUST return a 429 code
2. Services MUST return a standard error response describing the specifics so that a programmer can make appropriate changes
3. Services MUST return a Retry-After header that indicates how long clients should wait before retrying
4. Services MAY return RateLimit headers that document the limit or quota that has been exceeded
5. Services MAY return RateLimit-Limit: the number of calls the client is allowed to make in a time window
6. Services MAY return RateLimit-Remaining: the number of calls remaining in the time window
7. Services MAY return RateLimit-Reset: the time at which the window resets in UTC epoch seconds
8. Services MAY return other service specific RateLimit headers as appropriate for more detailed information or specific limits or quotas

#### 14.4.3. Overloaded services

When services are generally overloaded and  load shedding

1. Services MUST Return a 503 code
2. Services MUST Return a standard error response (see 7.10.2) describing the specifics so that a programmer can make appropriate changes
3. Services MUST Return a Retry-After header that indicates how long clients should wait before retrying
4. In the 503 case, the service SHOULD NOT return RateLimit headers

#### 14.4.4. Example Response

```http
HTTP/1.1 429 Too Many Requests
Content-Type: application/json
Retry-After: 5
RateLimit-Limit: 1000
RateLimit-Remaining: 0
RateLimit-Reset: 1538152773
{
  "error": {
    "code": "requestLimitExceeded",
    "message": "The caller has made too many requests in the time period.",
    "details": {
      "code": "RateLimit",
       "limit": "1000",
       "remaining": "0",
       "reset": "1538152773",
      }
    }
}
```

### 14.5. Caller Guidance

Callers include all users of the API: tools, portals, other services, not just user clients

1. Callers MUST wait for a minimum of time indicated in a response with a Retry-After before retrying a request.
2. Callers MAY assume that request is retriable after receiving a response with a Retry-After header without making any changes to the request.
3. Clients SHOULD use shared SDKs and common transient fault libraries to implement the proper behavior

See: <https://docs.microsoft.com/en-us/azure/architecture/best-practices/transient-faults>

### 14.6. Handling callers that ignore Retry-After headers

Ideally, 429 and 503 returns are so low cost that even clients that retry immediately can be handled.
In these cases, if possible the service team should make an effort to contact or fix the client.
If it is a known partner, a bug or incident should be filed.
In extreme cases it may be necessary to use DoS style protections such as blocking the caller.

## 15. Push notifications via webhooks

### 15.1. Scope

Services MAY implement push notifications via web hooks.
This section addresses the following key scenario:

> Push notification via HTTP Callbacks, often called Web Hooks, to publicly-addressable servers.

The approach set forth is chosen due to its simplicity, broad applicability, and low barrier to entry for service subscribers.
It's intended as a minimal set of requirements and as a starting point for additional functionality.

### 15.2. Principles

The core principles for services that support web hooks are:

1. Services MUST implement at least a poke/pull model. In the poke/pull model, a notification is sent to a client, and clients then send a request to get the current state or the record of change since their last notification. This approach avoids complexities around message ordering, missed messages, and change sets.  Services MAY add more data to provide rich notifications.
2. Services MUST implement the challenge/response protocol for configuring callback URLs.
3. Services SHOULD have a recommended age-out period, with flexibility for services to vary based on scenario.
4. Services SHOULD allow subscriptions that are raising successful notifications to live forever and SHOULD be tolerant of reasonable outage periods.
5. Firehose subscriptions MUST be delivered only over HTTPS. Services SHOULD require other subscription types to be HTTPS. See the "Security" section for more details.

### 15.3. Types of subscriptions

There are two subscription types, and services MAY implement either, both, or none.
The supported subscription types are:

1. Firehose subscriptions – a subscription is manually created for the subscribing application, typically in an app registration portal.  Notifications of activity that any users have consented to the app receiving are sent to this single subscription.
2. Per-resource subscriptions – the subscribing application uses code to programmatically create a subscription at runtime for some user-specific entity(s).

Services that support both subscription types SHOULD provide differentiated developer experiences for the two types:

1. Firehose – Services MUST NOT require developers to create code except to directly verify and respond to notifications.  Services MUST provide administrative UI for subscription management.  Services SHOULD NOT assume that end users are aware of the subscription, only the subscribing application's functionality.
2. Per-user – Services MUST provide an API for developers to create and manage subscriptions as part of their app as well as verifying and responding to notifications.  Services MAY expect end users to be aware of subscriptions and MUST allow end users to revoke subscriptions where they were created directly in response to user actions.

### 15.4. Call sequences

The call sequence for a firehose subscription MUST follow the diagram below.
It shows manual registration of application and subscription, and then the end user making use of one of the service's APIs.
At this part of the flow, two things MUST be stored:

1. The service MUST store the end user's act of consent to receiving notifications from this specific application (typically a background usage OAUTH scope.)
2. The subscribing application MUST store the end user's tokens in order to call back for details once notified of changes.

The final part of the sequence is the notification flow itself.

Non-normative implementation guidance:  A resource in the service changes and the service needs to run the following logic:

1. Determine the set of users who have access to the resource, and could thus expect apps to receive notifications about it on their behalf.
2. See which of those users have consented to receiving notifications and from which apps.
3. See which apps have registered a firehose subscription.
4. Join 1, 2, 3 to produce the concrete set of notifications that must be sent to apps.

It should be noted that the act of user consent and the act of setting up a firehose subscription could arrive in either order.
Services SHOULD send notifications with setup processed in either order.

![Firehose subscription setup][websequencediagram-firehose-subscription-setup]

For a per-user subscription, app registration is either manual or automated.
The call flow for a per-user subscription MUST follow the diagram below.
It shows the end user making use of one of the service's APIs, and again, the same two things MUST be stored:

1. The service MUST store the end user's act of consent to receiving notifications from this specific application (typically a background usage OAUTH scope).
2. The subscribing application MUST store the end user's tokens in order to call back for details once notified of changes.  

In this case, the subscription is set up programmatically using the end-user's token from the subscribing application.
The app MUST store the ID of the registered subscription alongside the user tokens.

Non normative implementation guidance: In the final part of the sequence, when an item of data in the service changes and the service needs to run the following logic:

1. Find the set of subscriptions that correspond via resource to the data that changed.
2. For subscriptions created under an app+user token, send a notification to the app per subscription with the subscription ID and user id of the subscription-creator.

- For subscriptions created with an app only token, check that the owner of the changed data or any user that has visibility of the changed data has consented to notifications to the application, and if so send a set of notifications per user id to the app per subscription with the subscription ID.

  ![User subscription setup][websequencediagram-user-subscription-setup]

### 15.5. Verifying subscriptions

When subscriptions change either programmatically or in response to change via administrative UI portals, the subscribing service needs to be protected from malicious or unexpected calls from services pushing potentially large volumes of notification traffic.

For all subscriptions, whether firehose or per-user, services MUST send a verification request as part of creation or modification via portal UI or API request, before sending any other notifications.

Verification requests MUST be of the following format as an HTTP/HTTPS POST to the subscription's _notificationUrl_.

```http
POST https://{notificationUrl}?validationToken={randomString}
ClientState: clientOriginatedOpaqueToken (if provided by client on subscription-creation)
Content-Length: 0
```

For the subscription to be set up, the application MUST respond with 200 OK to this request, with the _validationToken_ value as the sole entity body.
Note that if the _notificationUrl_ contains query parameters, the _validationToken_ parameter must be appended with an `&`.

If any challenge request does not receive the prescribed response within 5 seconds of sending the request, the service MUST return an error, MUST NOT create the subscription, and MUST NOT send further requests or notifications to _notificationUrl_.

Services MAY perform additional validations on URL ownership.

### 15.6. Receiving notifications

Services SHOULD send notifications in response to service data changes that do not include details of the changes themselves, but include enough information for the subscribing application to respond appropriately to the following process:

1. Applications MUST identify the correct cached OAuth token to use for a callback
2. Applications MAY look up any previous delta token for the relevant scope of change
3. Applications MUST determine the URL to call to perform the relevant query for the new state of the service, which MAY be a delta query.

Services that are providing notifications that will be relayed to end users MAY choose to add more detail to notification packets in order to reduce incoming call load on their service.
 Such services MUST be clear that notifications are not guaranteed to be delivered and may be lossy or out of order.

Notifications MAY be aggregated and sent in batches.
Applications MUST be prepared to receive multiple events inside a single push notification.

The service MUST send all Web Hook data notifications as POST requests.

Services MUST allow for a 30-second timeout for notifications.
If a timeout occurs or the application responds with a 5xx response, then the service SHOULD retry the notification with exponential back-off.
All other responses will be ignored.

The service MUST NOT follow 301/302 redirect requests.

#### 15.6.1. Notification payload

The basic format for notification payloads is a list of events, each containing the id of the subscription whose referenced resources have changed, the type of change, the resource that should be consumed to identify the exact details of the change and sufficient identity information to look up the token required to call that resource.

For a firehose subscription, a concrete example of this may look like:

```json
{
  "value": [
    {
      "subscriptionId": "32b8cbd6174ab18b",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files?$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    }
  ]
}
```

For a per-user subscription, a concrete example of this may look like:

```json
{
  "value": [
    {
      "subscriptionId": "32b8cbd6174ab183",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files/$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    },
    {
      "subscriptionId": "97b391179fa22",
      "clientState ": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files/$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    }
  ]
}
```

Following is a detailed description of the JSON payload.

A notification item consists a top-level object that contains an array of events, each of which identified the subscription due to which this notification is being sent.

Field | Description
----- | --------------------------------------------------------------------------------------------------
value | Array of events that have been raised within the subscription’s scope since the last notification.

Each item of the events array contains the following properties:

Field              | Description
------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
subscriptionId     | The id of the subscription due to which this notification has been sent.<br/>Services MUST provide the *subscriptionId* field.
clientState        | Services MUST provide the *clientState* field if it was provided at subscription creation time.
expirationDateTime | Services MUST provide the *expirationDateTime* field if the subscription has one.
resource           | Services MUST provide the resource field. This URL MUST be considered opaque by the subscribing application.  In the case of a richer notification it MAY be subsumed by message content that implicitly contains the resource URL to avoid duplication.<br/>If a service is providing this data as part of a more detailed data packet, then it need not be duplicated.
userId             | Services MUST provide this field for user-scoped resources.  In the case of user-scoped resources, the unique identifier for the user should be used.<br/>In the case of resources shared between a specific set of users, multiple notifications must be sent, passing the unique identifier of each user.<br/>For tenant-scoped resources, the user id of the subscription should be used.
tenantId           | Services that wish to support cross-tenant requests SHOULD provide this field. Services that provide notifications on tenant-scoped data MUST send this field.

### 15.7. Managing subscriptions programmatically

For per-user subscriptions, an API MUST be provided to create and manage subscriptions.
The API must support at least the operations described here.

#### 15.7.1. Creating subscriptions

A client creates a subscription by issuing a POST request against the subscriptions resource.
The subscription namespace is client-defined via the POST operation.

```text
https://api.contoso.com/apiVersion/$subscriptions
```

The POST request contains a single subscription object to be created.
That subscription object has the following properties:

Property Name   | Required | Notes
--------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------
resource        | Yes      | Resource path to watch.
notificationUrl | Yes      | The target web hook URL.
clientState     | No       | Opaque string passed back to the client on all notifications. Callers may choose to use this to provide tagging mechanisms.

If the subscription was successfully created, the service MUST respond with the status code 201 CREATED and a body containing at least the following properties:

Property Name      | Required | Notes
------------------ | -------- | -------------------------------------------------------------------------------------------
id                 | Yes      | Unique ID of the new subscription that can be used later to update/delete the subscription.
expirationDateTime | No       | Uses existing Microsoft REST API Guidelines defined time formats.

Creation of subscriptions SHOULD be idempotent.
The combination of properties scoped to the auth token, provides a uniqueness constraint.

Below is an example request using a User + Application principal to subscribe to notifications from a file:

```http
POST https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}

{
  "resource": "http://api.service.com/v1.0/files/file1.txt",
  "notificationUrl": "https://contoso.com/myCallbacks",
  "clientState": "clientOriginatedOpaqueToken"
}
```

The service SHOULD respond to such a message with a response format minimally like this:

```json
{
  "id": "32b8cbd6174ab18b",
  "expirationDateTime": "2016-02-04T11:23Z"
}
```

Below is an example using an Application-Only principal where the application is watching all files to which it's authorized:

```http
POST https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {ApplicationPrincipalBearerToken}

{
  "resource": "All.Files",
  "notificationUrl": "https://contoso.com/myCallbacks",
  "clientState": "clientOriginatedOpaqueToken"
}
```

The service SHOULD respond to such a message with a response format minimally like this:

```json
{
  "id": "8cbd6174abb391179",
  "expirationDateTime": "2016-02-04T11:23Z"
}
```

#### 15.7.2. Updating subscriptions

Services MAY support amending subscriptions.
 To update the properties of an existing subscription, clients use PATCH requests providing the ID and the properties that need to change.
Omitted properties will retain their values.
To delete a property, assign a value of JSON null to it.

As with creation, subscriptions are individually managed.

The following request changes the notification URL of an existing subscription:

```http
PATCH https://api.contoso.com/files/v1.0/$subscriptions/{id} HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}

{
  "notificationUrl": "https://contoso.com/myNewCallback"
}
```

If the PATCH request contains a new _notificationUrl_, the server MUST perform validation on it as described above.
If the new URL fails to validate, the service MUST fail the PATCH request and leave the subscription in its previous state.

The service MUST return an empty body and `204 No Content` to indicate a successful patch.

The service MUST return an error body and status code if the patch failed.

The operation MUST succeed or fail atomically.

#### 15.7.3. Deleting subscriptions

Services MUST support deleting subscriptions.
Existing subscriptions can be deleted by making a DELETE request against the subscription resource:

```http
DELETE https://api.contoso.com/files/v1.0/$subscriptions/{id} HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}
```

As with update, the service MUST return `204 No Content` for a successful delete, or an error body and status code to indicate failure.

#### 15.7.4. Enumerating subscriptions

To get a list of active subscriptions, clients issue a GET request against the subscriptions resource using a User + Application or Application-Only bearer token:

```http
GET https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}
```

The service MUST return a format as below using a User + Application principal bearer token:

```json
{
  "value": [
    {
      "id": "32b8cbd6174ab18b",
      "resource": " http://api.contoso.com/v1.0/files/file1.txt",
      "notificationUrl": "https://contoso.com/myCallbacks",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z"
    }
  ]
}
```

An example that may be returned using Application-Only principal bearer token:

```json
{
  "value": [
    {
      "id": "6174ab18bfa22",
      "resource": "All.Files ",
      "notificationUrl": "https://contoso.com/myCallbacks",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z"
    }
  ]
}
```

### 15.8. Security

All service URLs must be HTTPS (that is, all inbound calls MUST be HTTPS). Services that deal with Web Hooks MUST accept HTTPS.

We recommend that services that allow client defined Web Hook Callback URLs SHOULD NOT transmit data over HTTP.
This is because information can be inadvertently exposed via client, network, server logs and other mechanisms.

However, there are scenarios where the above recommendations cannot be followed due to client endpoint or software limitations.
Consequently, services MAY allow web hook URLs that are HTTP.

Furthermore, services that allow client defined HTTP web hooks callback URLs SHOULD be compliant with privacy policy specified by engineering leadership.
This will typically include recommending that clients prefer SSL connections and adhere to special precautions to ensure that logs and other service data collection are properly handled.

For example, services may not want to require developers to generate certificates to onboard.
Services might only enable this on test accounts.

## 16. Unsupported requests

RESTful API clients MAY request functionality that is currently unsupported.
RESTful APIs MUST respond to valid but unsupported requests consistent with this section.

### 16.1. Essential guidance

RESTful APIs will often choose to limit functionality that can be performed by clients.
For instance, auditing systems allow records to be created but not modified or deleted.
Similarly, some APIs will expose collections but require or otherwise limit filtering and ordering criteria, or MAY not support client-driven pagination.

### 16.2. Feature allow list

If a service does not support any of the below API features, then an error response MUST be provided if the feature is requested by a caller.
The features are:

- Key Addressing in a collection, such as: `https://api.contoso.com/v1.0/people/user1@contoso.com`
- Filtering a collection by a property value, such as: `https://api.contoso.com/v1.0/people?$filter=name eq 'david'`
- Filtering a collection by range, such as: `http://api.contoso.com/v1.0/people?$filter=hireDate ge 2014-01-01 and hireDate le 2014-12-31`
- Client-driven pagination via $top and $skip, such as: `http://api.contoso.com/v1.0/people?$top=5&$skip=2`
- Sorting by $orderBy, such as: `https://api.contoso.com/v1.0/people?$orderBy=name desc`
- Providing $delta tokens, such as: `https://api.contoso.com/v1.0/people?$delta`

#### 16.2.1. Error response

Services MUST provide an error response if a caller requests an unsupported feature found in the feature allow list.
The error response MUST be an HTTP status code from the 4xx series, indicating that the request cannot be fulfilled.
Unless a more specific error status is appropriate for the given request, services SHOULD return "400 Bad Request" and an error payload conforming to the error response guidance provided in the Microsoft REST API Guidelines.
Services SHOULD include enough detail in the response message for a developer to determine exactly what portion of the request is not supported.

Example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name HTTP/1.1
Accept: application/json
```

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": {
    "code": "ErrorUnsupportedOrderBy",
    "message": "Ordering by name is not supported."
  }
}
```

## 17. Naming guidelines

### 17.1. Approach

Naming policies should aid developers in discovering functionality without having to constantly refer to documentation.
Use of common patterns and standard conventions greatly aids developers in correctly guessing common property names and meanings.
Services SHOULD use verbose naming patterns and SHOULD NOT use abbreviations other than acronyms that are the dominant mode of expression in the domain being represented by the API, (e.g. Url).

### 17.2. Casing

- Acronyms SHOULD follow the casing conventions as though they were regular words (e.g. Url).
- All identifiers including namespaces, entityTypes, entitySets, properties, actions, functions and enumeration values SHOULD use lowerCamelCase.
- HTTP headers are the exception and SHOULD use standard HTTP convention of Capitalized-Hyphenated-Terms.

### 17.3. Names to avoid

Certain names are so overloaded in API domains that they lose all meaning or clash with other common usages in domains that cannot be avoided when using REST APIs, such as OAUTH.
Services SHOULD NOT use the following names:

- Context
- Scope
- Resource

### 17.4. Forming compound names

- Services SHOULD avoid using articles such as 'a', 'the', 'of' unless needed to convey meaning.
 - e.g. names such as aUser, theAccount, countOfBooks SHOULD NOT be used, rather user, account, bookCount SHOULD be preferred.
- Services SHOULD add a type to a property name when not doing so would cause ambiguity about how the data is represented or would cause the service not to use a common property name.
- When adding a type to a property name, services MUST add the type at the end, e.g. createdDateTime.

### 17.5. Identity properties

- Services MUST use string types for identity properties.
- For OData services, the service MUST use the OData @id property to represent the canonical identifier of the resource.
- Services MAY use the simple 'id' property to represent a local or legacy primary key value for a resource.
- Services SHOULD use the name of the relationship postfixed with 'Id' to represent a foreign key to another resource, e.g. subscriptionId.
 - The content of this property SHOULD be the canonical ID of the referenced resource.

### 17.6. Date and time properties

- For properties requiring both date and time, services MUST use the suffix 'DateTime'.
- For properties requiring only date information without specifying time, services MUST use the suffix 'Date', e.g. birthDate.
- For properties requiring only time information without specifying date, services MUST use the suffix 'Time', e.g. appointmentStartTime.

### 17.7. Name properties

- For the overall name of a resource typically shown to users, services MUST use the property name 'displayName'.
- Services MAY use other common naming properties, e.g. givenName, surname, signInName.

### 17.8. Collections and counts

- Services MUST name collections as plural nouns or plural noun phrases using correct English.
- Services MAY use simplified English for nouns that have plurals not in common verbal usage.
 - e.g. schemas MAY be used instead of schemata.
- Services MUST name counts of resources with a noun or noun phrase suffixed with 'Count'.

### 17.9. Common property names

Where services have a property, whose data matches the names below, the service MUST use the name from this table.
This table will grow as services add terms that will be more commonly used.
Service owners adding such terms SHOULD propose additions to this document.

| |
|------------- |
 attendees     |
 body          |  
 createdDateTime |
 childCount    |  
 children      |  
 contentUrl    |  
 country       |  
 createdBy     |  
 displayName   |
 errorUrl      |
 eTag          |
 event         |
 expirationDateTime |
 givenName     |
 jobTitle      |
 kind          |  
 id            |
 lastModifiedDateTime |
 location      |
 memberOf      |
 message       |
 name          |  
 owner         |
 people        |  
 person        |  
 postalCode    |
 photo         |
 preferredLanguage |
 properties    |
 signInName    |
 surname       |
 tags          |
 userPrincipalName |
 webUrl        |

## 18. Appendix

### 18.1. Sequence diagram notes

All sequence diagrams in this document are generated using the [WebSequenceDiagrams.com](https://www.websequencediagrams.com/). To generate them, paste the text below into the web tool.

#### 18.1.1. Push notifications, per user flow

```
=== Begin Text ===
note over Developer, Automation, App Server:
     An App Developer like MovieMaker
     Wants to integrate with primary service like Dropbox
end note
note over DB Portal, DB App Registration, DB Notifications, DB Auth, DB Service: The primary service like Dropbox
note over Client: The end users' browser or installed app

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Manual App Registration


Developer <--> DB Portal : Login into Portal, App Registration UX
DB Portal -> +DB App Registration: App Name etc.
note over DB App Registration: Confirm Portal Access Token

DB App Registration -> -DB Portal: App ID
DB Portal <--> App Server: Developer copies App ID

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Manual Notification Registration

Developer <--> DB Portal: webhook registration UX
DB Portal -> +DB Notifications: Register: App Server webhook URL, Scope, App ID
Note over DB Notifications : Confirm Portal Access Token
DB Notifications -> -DB Portal: notification ID
DB Portal --> App Server : Developer may copy notification ID


note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Client Authorization

Client -> +App Server : Request access to DB protected information
App Server -> -Client : Redirect to DB Authorization endpoint with authorization request
Client -> +DB Auth : Redirected authorization request
Client <--> DB Auth : Authorization UX
DB Auth -> -Client : Redirect back to App Server with code
Client -> +App Server : Redirect request back to access server with access code
App Server -> +DB Auth : Request tokens with access code
note right of DB Service: Cache that this User ID provided access to App ID
DB Auth -> -App Server : Response with access, refresh, and ID tokens
note right of App Server : Cache tokens by user ID
App Server -> -Client : Return information to client

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Flow

Client <--> DB Service: Changes to user data - typical via interacting with App Server via Client
DB Service -> App Server : Notification with notification ID and user ID
App Server -> +DB Service : Request changed information with cached access tokens and "since" token
note over DB Service: Confirm User Access Token
DB Service -> -App Server : Response with data and new "since" token
note right of App Server: Update status and cache new "since" token
=== End Text ===
```

#### 18.1.2. Push notifications, firehose flow

```
=== Begin Text ===
note over Developer, Automation, App Server:
     An App Developer like MovieMaker
     Wants to integrate with primary service like Dropbox
end note
note over DB Portal, DB App Registration, DB Notifications, DB Auth, DB Service: The primary service like Dropbox
note over Client: The end users' browser or installed app

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : App Registration

alt Automated app registration
   Developer <--> Automation: Configure
   Automation -> +DB App Registration: App Name etc.
   note over DB App Registration: Confirm App Access Token
   DB App Registration -> -Automation: App ID, App Secret
   Automation --> App Server : Embed App ID, App Secret
else Manual app registration
   Developer <--> DB Portal : Login into Portal, App Registration UX
   DB Portal -> +DB App Registration: App Name etc.
   note over DB App Registration: Confirm Portal Access Token

   DB App Registration -> -DB Portal: App ID
   DB Portal <--> App Server: Developer copies App ID
end

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Client Authorization

Client -> +App Server : Request access to DB protected information
App Server -> -Client : Redirect to DB Authorization endpoint with authorization request
Client -> +DB Auth : Redirected authorization request
Client <--> DB Auth : Authorization UX
DB Auth -> -Client : Redirect back to App Server with code
Client -> +App Server : Redirect request back to access server with access code
App Server -> +DB Auth : Request tokens with access code
note right of DB Service: Cache that this User ID provided access to App ID
DB Auth -> -App Server : Response with access, refresh, and ID tokens
note right of App Server : Cache tokens by user ID
App Server -> -Client : Return information to client



note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Registration

App Server->+DB Notifications: Register: App server webhook URL, Scope, App ID
note over DB Notifications : Confirm User Access Token
DB Notifications -> -App Server: notification ID
note right of App Server : Cache the Notification ID and User Access Token



note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Flow

Client <--> DB Service: Changes to user data - typical via interacting with App Server via Client
DB Service -> App Server : Notification with notification ID and user ID
App Server -> +DB Service : Request changed information with cached access tokens and "since" token
note over DB Service: Confirm User Access Token
DB Service -> -App Server : Response with data and new "since" token
note right of App Server: Update status and cache new "since" token



=== End Text ===
```

[fielding]: https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
[IANA-headers]: http://www.iana.org/assignments/message-headers/message-headers.xhtml
[rfc7231-7-1-1-1]: https://tools.ietf.org/html/rfc7231#section-7.1.1.1
[rfc-7230-3-1-1]: https://tools.ietf.org/html/rfc7230#section-3.1.1
[rfc-7231]: https://tools.ietf.org/html/rfc7231
[rest-in-practice]: http://www.amazon.com/REST-Practice-Hypermedia-Systems-Architecture/dp/0596805829/
[rest-on-wikipedia]: http://en.wikipedia.org/wiki/Representational_state_transfer
[表现层状态转换]: https://wiki.tw.wjbk.site/wiki/REST
[rfc-5789]: http://tools.ietf.org/html/rfc5789
[rfc-5988]: http://tools.ietf.org/html/rfc5988
[rfc-3339]: https://tools.ietf.org/html/rfc3339
[rfc-5322-3-3]: https://tools.ietf.org/html/rfc5322#section-3.3
[cors-preflight]: http://www.w3.org/TR/cors/#resource-preflight-requests
[rfc-3864]: http://www.ietf.org/rfc/rfc3864.txt
[odata-json-annotations]: http://docs.oasis-open.org/odata/odata-json-format/v4.0/os/odata-json-format-v4.0-os.html#_Instance_Annotations
[cors]: http://www.w3.org/TR/access-control/
[cors-user-credentials]: http://www.w3.org/TR/access-control/#user-credentials
[cors-simple-headers]: http://www.w3.org/TR/access-control/#simple-header
[rfc-4627]: https://tools.ietf.org/html/rfc4627
[iso-8601]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15
[clr-time]: https://msdn.microsoft.com/en-us/library/System.DateTime(v=vs.110).aspx
[ecmascript-time]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.1
[ole-date]: https://docs.microsoft.com/en-us/windows/desktop/api/oleauto/nf-oleauto-varianttimetosystemtime
[ticks-time]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[unix-time]: https://msdn.microsoft.com/en-us/library/1f4c8f33.aspx
[windows-time]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[excel-time]: http://support.microsoft.com/kb/214326?wa=wsignin1.0
[wikipedia-iso8601-durations]: http://en.wikipedia.org/wiki/ISO_8601#Durations
[wikipedia-iso8601-intervals]: http://en.wikipedia.org/wiki/ISO_8601#Time_intervals
[wikipedia-iso8601-repeatingintervals]: http://en.wikipedia.org/wiki/ISO_8601#Repeating_intervals
[principle-of-least-astonishment]: http://en.wikipedia.org/wiki/Principle_of_least_astonishment
[odata-breaking-changes]: http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part1-protocol/odata-v4.0-errata02-os-part1-protocol-complete.html#_Toc406398209
[websequencediagram-firehose-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDogTWFudWFsAIFzEQoKCgCDAgo8LS0-AIIqCiA6IExvZ2luIGludG8Agj8JAII1ECBVWCAKACoKLT4gKwCCWBM6AIQGBU5hbWUgZXRjLgCDFQ4AGxJDb25maXJtAIEBCEFjY2VzcyBUb2tlbgoKAIM3EyAtPiAtAINkCQBnBklEAIEMCwCBVQUAhQIMAIR3CmNvcGllcwArCACCIHAAhHMMAIMKDwCDABg6IHdlYmhvb2sgcgCCeg4AgnUSAIVQDToAhXYHZXIAgwgGAIcTBgBECVVSTCwgU2NvcGUAhzIGSUQKTgCGPQwAhhwNIACDBh4AHhEAgxEPbgCBagwAgxwNAIMaDiAAgx0MbWF5IGNvcHkALREAhVtqAIZHB0F1dGhvcml6AIY7BwCGXQctPiArAIEuDVJlcXVlc3QgYQCFOQZ0byBEQiBwcm90ZWN0ZWQgaW5mb3IAiiQGCgCDBQstPiAtAIctCVJlZGlyZWN0ADYHAGwNIGVuZHBvaW50AIoWBmEADw1yAHYGAIEQDACJVAcASwtlZAAYHgCICAgAMAcAcA4AhGoGAE0FAIEdFmJhY2sgdG8AhF8NaXRoIGNvZGUAghoaaQCBagcAgToHAD0JAII-B3MAPgsAglEHAEsFAIIzDgCBXw0Agn8GdG9rZW5zACcSAI0_BXJpZ2h0IG9mAItpDUNhY2hlIHRoYXQgdGhpcyBVc2VyIElEIHByb3ZpZGVkAINNCwCIZgoAggcJAIN7D3Nwb25zAI0_BwCECgYsIHJlZnJlc2gsIGFuZCBJRACBHAcAgQMPAIYADQCBDAcAgUUGYnkAjFkFIElEAIQkG3R1cm4AhF4MIHRvIGMAjR8FAIwRagCJVw1GbG93AIYqCQCMaQgAgmoKaGFuZ2UAj3YFAIFXBWRhdGEgLSB0eXBpY2FsIHZpYQCQDgVyYWN0aW5nAJAPBgCJQQt2aWEAjnsHCgCPNgogAIhDEACKZw0AkFMFAIkBDwCDDAUAgkYWKwBNCwCHWApjAIEyBQCHRg0AhWUHYWNoAIQeDACEfwVhbmQgInNpbmNlIgCFEQYAkSQOAIR3CgCNfwcAhHQFAIpQEACBUgsAhFAcAII8BWFuZCBuZXcAYRQAhFUTOiBVcGRhdGUgc3RhdHUAgSkGAIFDBQAxEwoKCg&s=mscgen
[websequencediagram-user-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDoAgWwRCgphbHQAgyUIAIEHBiByABQMICAAgxsLPC0tPgCDTws6IENvbmZpZ3VyZQogIACDaAsgLT4gKwCCWBMAegZOYW1lIGV0Yy4AhAgFAIMaDQAfEgBdBXJtAIQ_BUFjY2VzcyBUb2tlAIETBgCDOxIgLT4gLQCBFgxBcHAgSUQAhHwIY3JldACBGxAtPgCFFgsgOiBFbWJlZAAkFGVsc2UgTWFudWFsAIIEJACEbQkgOiBMb2dpbiBpbnRvAIUBCQCBKRFVWACGGAUALQoAgh8mAIIZKwCBCAcAgjoNAIIsHACGLwkAgj8IAIESDgCECAYAh1ELAIdFCmNvcGllcwAuCGVuZACEeGoAhWQHQXV0aG9yaXoAhV8HAIV6By0-ICsAg2ANUmVxdWVzdCBhAIRVBnRvIERCIHByb3RlY3RlZCBpbmZvcgCJQQYKAIQaCy0-IC0AhkoJUmVkaXJlY3QANgcAbA0gZW5kcG9pbnQAiTMGYQAPDXIAdgYAgRAMAIhxBwBLC2VkABgeAIRjCAAwB0EAcQxVWAoASQgAgRwWYmFjayB0bwCFdAwAilwFY29kZQCCGRppAIFpBwCBOQcAPQkAgj0HcwA-CwCCUAcASwUAgjIOAIFeDQCCfgZ0b2tlbnMAJxIAjFsFcmlnaHQgb2YAiwUNQ2FjaGUgdGhhdCB0aGlzIFVzZXIgSUQgcHJvdmlkZWQAg0wLAIU6BwCCBAwAg3oPc3BvbnMAjFsHAIQJBiwgcmVmcmVzaCwgYW5kIElEAIEcBwCBAw8AiDENAIEMBwCBRQZieQCLdQUgSUQAhCMbdHVybgCEXQwgdG8gYwCMOwUKCgCLL2oAjXUMAIwTDwCPNQotPisAjhwQOgCORQdlcgCMVwYAg3YIZWJob29rIFVSTCwgU2NvcGUAkAEGSUQAjwoOAI5rDSAAi2UKAINFBQCLYw0AHBEAgzUOOiBuAIE2DABgCACDCB1oZQCBaQ5JRACDYwUAahIAghB4RmxvdwCJMwkAjE0IAIV0CmhhbmdlAJIcBQCEYQVkYXRhIC0gdHlwaWNhbCB2aWEAkjQFcmFjdGluZwCSNQYAjV8LdmlhAJEhBwoAkVwKIACNfhAAhAsNAJJ5BQCCWQ8AhhYFAIVQFisATQsAimEKYwCBMgUAik8NAIhvB2FjaACHKAwAiAkFYW5kICJzaW5jZSIAiBsGAJNKDgCIAQoAhB0cAIFSCwCHWhwAgjwFYW5kIG5ldwBhFACHXxM6IFVwZGF0ZSBzdGF0dQCBKQYAgUMFADETCgoK&s=mscgen
