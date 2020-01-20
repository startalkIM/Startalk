# Introduction
STARTALK is an open source IM system platform. It covers all the target platforms and target systems you can think of, excellent performance.

Startalk focuses on the application scenarios of toB.

The goal of Startalk is, to slove the problem that enterprises do not have a unified IM platform in the past 20 years:

```
In contrast, after more than 20 years of evolution in major operation systems,with the advent of nginx, deploying a set of http services has become very simple.
```


put it simply, the advantages of our system are very obvious:

```
1. Performance, stability and platform adaptability can be regarded as the best in the industry.
2. Supporting all platforms in theoretically.
3. Almost all the code is open sourced.
4. Pure communication software, covering most scenarios what you can think of.
```

The disadvantages are also:

```
1. The system is huge and complex
2. The technology stack involved is very wide. 
3. It cannot be fully grasped by personal.
```

you can take a look the [previous version](https://github.com/startalkIM/startalk_backup).


### Contents

* [Our slogan!](#Slogan)
* [Platform features](#Features)
    * [Security](#Security)
    * [Performance and characteristics](#Performance)
    * [native coding, all platform support](#Supprting)
* Start trial(there are 3 steps for the basic services deployment)
    1. [Deploying backend services](Choose one out of three)
        * [Use the one-click installation package]()
        * [by the docker](docker-file-en.md)
        * [by source code documentation](source-build-en.md)
    1. [User registration](create-user-en.md)
    1. Client side Deployment
        * [Download](config-client-en.md)
        * [Configuration](config-client-en.md)
* Extensions
    1. [Notification Services(Push Services)](https://github.com/startalkIM/push_service)
    1. [audio and video call](https://github.com/startalkIM/call_room_server)
    1. [Red envelope, AA Pay(under construction)]()


# Slogan

```
Even if not a milestone, at least be a paving stone!
```


# Features

The characteristics of the platform are relected in the following aspects:

## Security

* private protocol

```
Startalk's communication protocol is private,
so the complexity and difficulty of eavesdropping during transamission
are increased.
```

* Binary protocol

```
Different from common text protocols(such as xmpp, sip, http, etc.), Startalk's protocol is binary.
it also increases the complexity and diffculty of being intercepted during transmission.
```

* TCP with TLS, end-to-end encryption supported

* You have more than just the front-end, event the back-end.

## Performance

### back-end
* Easily to support millions of users online, easily to support expansion

### front-end

* Power saving on mobile, up to 90%
* Data saving on mobile, up to 90%
* Never miss a message
* Even with a lot of messages, it is still very smooth