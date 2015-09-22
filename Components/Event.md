Event Manager
=============

If you use events in you application, this component will help you to manage them. There's only one service (`\Krystal\Event\EventManager`), that helps you to accomplish the management.

# Available methods

## attach()

    \Krystal\Event\EventManager::attach($event, Closure $listener)

Attaches a listener (callable function) to an event. The first `$event` defines event name and the second `$closure` must be a function to be invoked when calling the event which is being defined.

## detach()

    \Krystal\Event\EventManager::detach($event)

Detaches an event. In case an event wasn't registered before, it'd throw `RuntimeException` indicating failure.

## detachAll()

    \Krystal\Event\EventManager::detachAll()

Detaches all attached events.

## trigger()

    \Krystal\Event\EventManager::trigger($event)

Triggers attached listener to an event, In case an event isn't defined, it'd throw `RuntimeException` indicating failure.

## has()

    \Krystal\Event\EventManager::has($event)

Checks whether an event is defined.

## countAll()

    \Krystal\Event\EventManager::countAll()

Counts amount of all attached events.

# Usage example:

    use Krystal\Event\EventManager;
    
    $text = 'Hello World!';
    
    $em = new EventManager();
    $em->attach('greet', function() use ($text){
      echo $text;
    });
    
    // Trigger it
    $em->trigger('greet'); // Will output "Hello World"
    
    // Or via method overloading
    $em->greet();