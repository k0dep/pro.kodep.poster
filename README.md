# Poster

    Simple event bus for unity

## Send message sample

```csharp
using Poster;
using UnityEngine;

public class BallSpawnedMessage
{
    public GameObject BallObject;
}

public class BallSpawnService
{
    public IMessageSender Bus = MessageBusStatic.Bus;

    public void Spawn()
    {
        var newBall = Instantiate(...);
        Bus.Send(new BallSpawnedMessage
        {
            BallObject = newBall
        });
    }
}
```

## Receive message sample

```csharp
using Messages;
using Poster;
using UnityEngine;

public class Enemy : MonoBehaviour
{
    public IMessageBinder Bus = MessageBusStatic.Bus;

    private Transform _ball;

    public void OnEnable()
    {
        Bus.Bind<BallSpawnedMessage>(BallSpawned); // subsribe for receive messages of BallSpawnedMessage type
    }

    public void OnDisable()
    {
        Bus.UnBind<BallSpawnedMessage>(BallSpawned); // unsubscribe
    }

    private void BallSpawned(BallSpawnedMessage obj)
    {
        _ball = obj.BallObject.transform;
    }
}
```

# Using

To start using this package add a line to your `./Packages/manifest.json` like next sample:  
```json
{
  "dependencies": {
    "pro.kodep.poster": "https://github.com/k0dep/pro.kodep.poster.git"
  }
}
```

Or use it as dependency from another packages and auto include by [Originer](https://github.com/k0dep/Originer) package