# Please do this stuff if you're shipping code
TL;DR 
- Support 10x scale. If 0 users will run your code, build to support 1. If 10, support 100, etc 
- One git repo per service
-- ft. README saying What it is, How to run it locally for dev and How to ship it
- If your service is callable from other services, expose an interface + client to call it 
- Duplicate code in a service should be a function 

# Not TL;DR
![image](https://user-images.githubusercontent.com/2851307/182747517-c1ebc6fc-f2f7-4116-a07d-9be9d20db1c7.png)

Anyone who writes / approves / ships code is responsible for it's outcome on product

If you add a bug, it'll be caught by a poor soul & prioritized as P0-P3 

Priority = “What are you willing to ask team mates to go through to fix this?”

P3 = "Maybe work on it if you’ve got nothing else to do + feel inspired" 
- (e.g., UI button flickers sometimes)

P2 = "Needs to be done but have to balance it against other priorities, slot it in when you get time"
- e.g., sometimes web app just crashes and you have to refresh, but doesn’t block user, its just dumb

P1 = "Don’t work on anything else until this is fixed, but take the time you need"
- e.g., Build pipeline is down

P0 = "Fix this now, even if it's 3am on a Saturday on Christmas. 1 person is awake at all times, take turns sleeping to gain energy to fix. Order some pizza, buckle up for the ride."
- e.g., global prod outage, inference is down

A P0 signals systemic problems and shouldn’t be used lightly.

Don’t cause a P0 unless you’re willing to own it

You can delegate bugs you own to those who are willing but good luck maintaining positive relationships this way. 

# Code Guidelines

## Split logical bundles of code into microservices
A microservice is a standalone app, exposing an HTTP server /w clearly defined endpoints

If you make breaking changes to endpoints, update them via /v2/, /v3, etc versions to maintain backwards compatibility

Microservice is runnable with docker, supports healthchecking, loadbalancing, can be deployed on Zeet or another k8 platform

## What do you call the same code twice? A function.
If you've written the same block of code (say, >3 lines) twice, it should most likely be a function.

If you can't make it a function because it's slightly different then you haven't written the same code twice. 

example: if you see code like this multiple times. i.e., if error, unlock mutex + throw json payload 
```go
if err != nil {
	fmt.Println(errorPrefix, ":", err)
	GlobalMutex.Unlock()
	return c.Status(500).JSON(fiber.Map{
		"error": err.Error(),
	})
}
```
rewrite it into a function like this
```go
if err != nil {
	return UnlockAndThrowError("an error occured:", err)
}
```

## Does this support 10x scale?

Before merging something in ask this question. 

Your code needs to support the next 10x growth because if we're growing fast this will happen in <6 months

If your code doesn't scale you'll end up playing wack-a-mole with bugs the moment more users hit the product

If you don't know if anyone wants the thing you made support 1 user and ship it fast

If you have 1 user waiting you should be building to support 10

If you have 10 users waiting you should be building to support 100
