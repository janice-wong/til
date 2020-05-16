# Enums have a default value, duh

Today, I commented on a PR, "Should we be checking this `Enum` for `null`?" and I got the response:
>Enums have a default value, duh.

Just kidding - that's terrible. No one should need to work with someone who speaks that way.

Point is - in C#, **non-nullable** `Enum`s have a default value and cannot be be `null`, though C# does offer _nullable_ `Enum`s.

```
public NonNullableEnum someNonNullableEnum = NonNullableEnum.Undefined;
public NullableEnum? someNullableEnum = null;
```

Duh.

***

#### What's the default value of an enum?
It depends. If you don't specify custom values, the `default` is the first element in the `Enum`. If you want to override the default value, you can do the following:

```
enum HelloEnum
{
    Hi,
    Hello = 0,      // default
    Yo,
}
```
