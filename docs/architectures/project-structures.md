# Project Structures

The project structure is straightforward. Under the root, there are four main modules: `ZapClient`, `ZapServer`, `Models`, and `Resources`.

![](https://user-images.githubusercontent.com/6410412/283739678-5b7ab499-9be9-478f-9f66-958b521f93c3.png)

`Models` include interfaces and type definitions generally referenced throughout the Zap system. On the other hand, `Resources` consist of implementations of `Zapable` that can be transmitted via Zap. For example, `ZapAccelerometer` is a `Zapable` implementation representing accelerometer data. These resource implementations are referenced to abstract the data obtained from data sources in `ZapClient` or `ZapServer` and are encapsulated in `ZappObject` for network transmission. For more detailed information on each component, please read the [Specifications](../specifications/index.md) section.
