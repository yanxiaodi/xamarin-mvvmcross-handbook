# Introduction

## Foreword

![](.gitbook/assets/image%20%2825%29.png)

As an UWP developer, I am used to using the `MVVM` pattern to develop my APPs. But when I started to learn `Xamarin`, I felt a little inconvenience because I have to use the native UI and add a lot of boring event handlers to the controls. I compared several `MVVM` frameworks and found that `MvvmCross` might be a good choice to use the `MVVM` pattern for `Xamarin` APPs.

`MvvmCross` is a cross-platform `MVVM` framework which supports `Xamarin`, `iOS`, `Xamarin.Android`, `Xamarin.Mac`, `Xamarin.Forms`, `UWP` and `WPF`. It implements the `Dependecy Injection` pattern, also provide us with various plugins, such as `Messenger`, `Network`, `File`, `ResourceLoader` and `Location`, etc, to support different target platforms. We can easily deliver a `Xamarin` App with it. But as the same as the other `MVVM` framework, the users need to have a fundamental understanding of `MVVM`. If not, do not worry. I will show you some basic points regarding the `MVVM` pattern, and guide you to create your first `Xamarin` App with `MvvmCross`.

![](.gitbook/assets/image%20%2838%29.png)

Just note that `MvvmCross` updates quickly so you might need to fine-adjust the codes in this article. The current version of `MvvmCross` is V6.2.1.

The first question is that which type of the shared project you should use. In the .NET world, there are several different ways that we share the codes across different platforms, such as Shared Project, Portable Class Library, and .NET Standard. I highly recommend using the latest .NET Standard library as the Core project, rather than the PCL library. Why? One reason is that you cannot create a new PCL project by VS 2017. And the .NET Standard Library is a formal specification of .NET APIs, so we will have a greater uniformity. According to the docs, it is the next generation of PCL. That is why I think the .NET Standard library is the better one. But there is still a small problem. If you select .NET Standard 2.0 for the Core project, it will not support the old version of Windows 10. It only supports version 10.0.16299 and higher version of Windows 10. Another issue is that some third libraries might not update to support .NET Standard. So, choose the right approach according to your real scenario.

Although `MvvmCross` provides quite a few templates \(some of them are contributed by the third-party developers\), it is necessary to understand how to build a new App from zero. So we do not use any third-party templates. I am going to demonstrate how to use `MvvmCross` to show these features:

* Data-Binding
* Lifecycle of the ViewModel
* Navigation
* Command
* ……

It is very boring to introduce a lot of knowledge before you write the first line of code. I will start it directly, then come back to understand the meaning of the codes. Some basic sample codes come from the official documentation of `MvvmCross`.

You can check the source code of this demo on my GitHub: [https://github.com/yanxiaodi/MvvmCrossDemo](https://github.com/yanxiaodi/MvvmCrossDemo).

Let's get started!

For the better reading experience, you can read it [here](https://yanxiaodi.gitbook.io/xamarin-mvvmcross-handbook/).

