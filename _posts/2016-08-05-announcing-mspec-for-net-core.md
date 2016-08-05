---
title: "Announcing Machine.Specifications (MSpec) 0.11 for .NET Core, .NET CLI and .NET Standard"
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

I am delighted to announce that [MSpec](https://github.com/machine/machine.specifications/wiki) (Machine.Specifications) is now available for .NET Standard 1.3+ together with a new .NET CLI (`dotnet test`) test runner for .NET Core and .NET 4.5+.

If you are not sure what MSpec is - scroll down for a quick intro. In short it's a "context/specification" test framework.

This is backwards compatible version, so there shouldn't be any issues using this version with ReSharper, CodeRush or other test runners when targeting the .NET Framework.


| Package                            | Version      | NuGet |
|----------------------------------- |--------------|-------|
| dotnet-test-mspec                  | 0.2.0  | [NuGet](https://www.nuget.org/packages/dotnet-test-mspec)     |
| Machine.Specifications             | 0.11.0 | [NuGet](https://www.nuget.org/packages/Machine.Specifications)      |
| Machine.Specifications.Should      | 0.11.0  | [NuGet](https://www.nuget.org/packages/Machine.Specifications.Should)      |
| Machine.Fakes                      | 2.8.0-beta3   | [NuGet](https://www.nuget.org/packages/Machine.Fakes/2.8.0-beta3)      |
| Machine.Fakes.Moq                  | 2.8.0-beta3   | [NuGet](https://www.nuget.org/packages/Machine.Fakes.Moq/2.8.0-beta3)     |

Machine.Fakes.Moq is in beta due to the underlying Moq dependency not having yet a stable release for .NET Standard.

**Useful Links**

* [MSpec documentation](https://github.com/machine/machine.specifications/wiki)
* [MSpec issue tracker](https://github.com/machine/machine.specifications/issues)


## Getting started on .Net Core and .NET CLI

Here is an example `project.json` of a tests project which targets both .NET Core and .NET 4.5. As you can see it includes:

1. Dependency on `Machine.Specifications` and `dotnet-test-mspec`
2. `"testRunner": "mspec"`

**project.json**
```json
{
    "testRunner": "mspec",
    "frameworks": {
        "net45": { },
        "netcoreapp1.0": {
            "dependencies": {
                "Microsoft.NETCore.App": {
                    "version": "1.*",
                    "type": "platform"
                }
            }
        }
    },
    "dependencies": {
        "FluentAssertions": "4.12.0",
        "Machine.Specifications": "0.11.0",
        "dotnet-test-mspec": "0.2.0"
    }
}
```

We can now use `dotnet test` to run the tests:

```
> dotnet test

Project Example (.NETCoreApp,Version=v1.0) was previously compiled. Skipping compilation.

dotnet-test-mspec: 0.2.0
Machine.Specifications: 0.11.0.0
Specs in Example:
...***

Contexts: 3, Specifications: 6, Time: 0.13 seconds
  3 passed, 0 failed, 3 not implemented

Project Example (.NETFramework,Version=v4.5) was previously compiled. Skipping compilation.

dotnet-test-mspec: 0.2.0
Machine.Specifications: 1.0.*
Specs in Example:
...***

Contexts: 3, Specifications: 6, Time: 0.09 seconds
  3 passed, 0 failed, 3 not implemented

SUMMARY: Total: 2 targets, Passed: 2, Failed: 0.
```

### Known Issues with the dotnet cli (dotnet test) runner

1. IDE integration (aka Design-Time) is not yet implemented, so you won't be able to see or run test in Visual Studio IDE: [#303](https://github.com/machine/machine.specifications/issues/303)
2. The console output format doesn't have colours and enough new line padding, etc. This is an upstream issue with `dotnet test` and hopefully will be fixed in the next preview. [#311](https://github.com/machine/machine.specifications/issues/311). This is potentially blocking the TeamCity and AppVeyor auto-detect outputs.

Hopefully they should land in v0.3 of the test runner.

## What is MSpec?

MSpec is called a "context/specification" test framework because of the "grammar" that is used in describing and coding the tests or "specs". That grammar reads roughly like this

> When the system is in such a state, and a certain action occurs, it should do such-and-such or be in some end state.

You should be able to see the components of the traditional Arrange-Act-Assert model in there. To support readability and remove as much "noise" as possible, MSpec eschews the traditional attribute-on-method model of test construction. It instead uses custom .NET delegates that you assign anonymous methods and asks you to name them following a certain convention.

You can read more about MSpec [in the docs here](https://github.com/machine/machine.specifications/wiki).

```csharp
[Subject("Authentication")]
public class When_authenticating_an_admin_user
{
    Establish context = () => {
        Subject = new SecurityService();
    };

    Because of = () => {
        Token = Subject.Authenticate("username", "password");
    };

    It should_indicate_the_users_role = () => {
        Token.Role.ShouldEqual(Roles.Admin);
    };

    It should_have_a_unique_session_id = () => {
        Token.SessionId.ShouldNotBeNull();
    };

    static SecurityService Subject;
    static UserToken Token;
}
```

and an example of Machine.Fakes usage in MSpec:

```csharp
[Subject("Authentication")]
public class When_authenticating_an_admin_user : WithFakes
{
    Establish context = () => {
        Subject = new SecurityService(The<SecurityClient>());

        The<SecurityClient>()
            .WhenToldTo(client => client.SendRequest(Param<string>.IsAny))
            .Returns(true);
    };

    Because of = () => {
        Subject.Authenticate("username", "password");
    };

    It should_request_a_token = () => {
        The<SecurityClient>()
            .WasTaldTo(client => client.RequestToken());
    };

    static SecurityService Subject;
}
```