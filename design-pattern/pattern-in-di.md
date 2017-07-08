## How To Support Programmer Use Pattern In DI Container

---

Ex: \(DotNet Core DI\)

i have following pattern want other programmer may use this pattern

```
public interface ICommandHandler<in TCommand> where TCommand : class
{
    void Invoke(TCommand command);
}

public interface ICommandHandlerDecorator<TCommand> : ICommandHandler<TCommand> where TCommand : class
{
}
```

so 

```
//-------extensions methods-------
public static IServiceCollection AddHandler<T, TServiceType, TImplementationType>(this IServiceCollection services)
    where TServiceType : class, ICommandHandler<T>
    where TImplementationType : class, TServiceType
    where T : class
{
    services.InGeneric<T>(srv => srv.AddSingleton(
        typeof(TServiceType).GetGenericTypeDefinition(),
        typeof(TImplementationType).GetGenericTypeDefinition()));
    return services;
}

public static IServiceCollection AddHandlerDecorator<T, TServiceType, TImplementationType>(this IServiceCollection services)
    where TServiceType : class, ICommandHandlerDecorator<T>
    where TImplementationType : class, TServiceType
    where T : class
{
    services.InGeneric<T>(srv => srv.AddSingleton(
        typeof(TServiceType).GetGenericTypeDefinition(),
        typeof(TImplementationType).GetGenericTypeDefinition()));
    return services;
}

private static IServiceCollection InGeneric<T>(this IServiceCollection services,Action<IServiceCollection<T>> config)
{
    config(new ServiceCollectionDecorator<T>(services));
    return services;
}
//--------decorate IServiceCollection-----
internal class ServiceCollectionDecorator<T> : IServiceCollection<T>
{
    private readonly IServiceCollection _services;

    public ServiceCollectionDecorator(IServiceCollection services)
    {
        _services = services;
    }

    void ICollection<ServiceDescriptor>.Add(ServiceDescriptor item)
    {
        if (item.ServiceType.IsGenericType
            && (item.ImplementationType?.IsGenericType ?? false))
        {
            item = ServiceDescriptor.Describe(
                item.ServiceType.GetGenericTypeDefinition(),
                item.ImplementationType.GetGenericTypeDefinition(),
                item.Lifetime);
        }
        this.Add(item);
    }
    #region impl code
    //impl etc...
    #endregion
}
```

then other programmer should register like follow script:

```
new ServiceCollection().AddHandler<object,IWriteToCommandHandler<object>, OutputWithStringCommandHandler<object>>()
    .AddHandler<object, IWriteToCommandHandler<object>, OutputWithJsonCommandHandler<object>>()
    .AddHandlerDecorator<object, IDoManyTimesCommandHandlerDecorator<object>,TwiceCommandHandlerDecorator<object>>()
    .AddHandlerDecorator<object, IDoManyTimesCommandHandlerDecorator<object>, ThreeTimesCommandHandlerDecorator<object>>()
```



