# Encapsulate your collections, damn it!

This has been yet another lesson in domain-driven design (DDD). Say you're working in a puppy-breeding DDD app and you have this basic class:

```
public class Breeder
{
    public List<Guid> PuppyIds { get; private set; }

    public Breeder(List<Guid> puppyIds) { PuppyIds = puppyIds; }
}
```

There may have been someone at some point who thought this approach was fine since you could only set `PuppyIds` in the constructor. Au contraire, this implementation does not support DDD at all because you could do something like:

```
breeder.PuppyIds.Add(Guid.NewGuid());
```

`PuppyIds` has not been encapsulated, so we've now exposed the ability to modify this list. We can address this issue by making the public getter read-only.

```
public class Breeder
{
    private static List<Guid> _puppyIds;

    public IReadOnlyCollection<Guid> PuppyIds { get; } = _puppyIds.AsReadOnly();

    public Breeder(List<Guid> puppyIds) { _puppyIds = puppyIds; }
}
```
