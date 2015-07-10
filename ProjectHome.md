# Repository with NHibernate #

## Introduction ##

This project serves as an example on how to implement the Repository pattern in .NET using NHibernate (with FluentNHibernate). There is no official definition of the Repository pattern and many different implementations exist on the web. However, most sources will agree that the the Repository pattern is effectively the [Factory](http://en.wikipedia.org/wiki/Factory_method_pattern) pattern with the 4 basic CRUD operations (plus a LINQ-compatible `GetAll<T>()`), like so:

```
public interface IRepository<T>
{
    T Get(object id);
    void Save(T value);
    void Update(T value);
    void Delete(T value);
    IList<T> GetAll();
}
```

## Description ##

The beauty of Repository is in its simplicity. Grabbing an object from the database only requires two lines of code:

```
IRepository<Monkey> monkeyRepo = new NHibernateRepository<Monkey>();

Monkey m = monkeyRepo.Get(IdMonkey);
```

Additionally, by abstracting the DAL, you are able to replace the DAL with any other ORM or other implementation you please via simple dependency switch.

As with any kind of abstraction, you lose some amount of control by limiting yourself to these 5 methods. However, abstraction isn't always a bad thing, otherwise we'd program everything in assembly.

I'll offer a word of caution that this implementation should only be used for projects involving small amounts of data. `GetAll<T>()` could potentially retrieve every single object from the database, and _in some cases this is fine_, but don't blame me if your DBA/network admin lights your cubicle on fire.

If you're looking for a pattern that allows offers more control over lazyloading and transactional behavior, check out the [Unit of Work](http://code.google.com/p/nhibernate-unitofwork-example/) facade over NHibernate.

You may want to use this project as a starting point for your current project - go ahead. Just replace the domain classes with your own, write the mappings (or enable automapping), and away you go.