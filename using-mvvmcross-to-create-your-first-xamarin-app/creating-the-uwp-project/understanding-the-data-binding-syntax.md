# Understanding the data-binding syntax

As we mentioned before, the default data-binding type in UWP is `One-Way`, so we need to set a specific value as `TwoWay`:

```markup
<TextBox Text="{Binding UserName, Mode=TwoWay}"></TextBox>
```

Now you can get an app shown below:

![](../../.gitbook/assets/image%20%2823%29.png)

In the next section, I will add some real API Services to get some data from the online APIs and add some commands.

