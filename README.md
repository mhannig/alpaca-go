

# Alpaca

An opinionated library for writing services on a MQTT message bus.


**IMPORTANT: I deceided to move this into an org: [CamelIoT](https://github.com/cameliot/alpaca)**


## Why?


Something along the lines of: After implementing [DaliQTT](https://github.com/cccb/daliqtt)
I wanted something more generalized to create new services more
easily.

I guess this will later need a python port aswell.

## How to use

Creating a new service in your network
is now as easy as:

```golang
dispatch, actions := alpaca.DialMqtt(
    "tcp://user:pass@localhost:1889",
    alpaca.Routes{
        "lights": "v1/upstairs/lights",
        "meta": "v1/_meta/",
    })

handle(actions, dispatch)
```

With a service handler like:

```golang
const GET_LIGHT_VALUE_REQUEST = "@lights/GET_LIGHT_VALUE_REQUEST"

type Light struct {
    Id     int `json:"id"`
    Value  int `json:"value"`
}

func handle(actions alpaca.Actions, dispatch alpaca.Dispatch) {
    // Do some more setup stuff...

    // Handle incoming actions
    for action := range actions {
        switch action.Type {
        case SET_LIGHT_VALUE_REQUEST:
            var light Light
            action.DecodePayload(&light)
            setLightValue(light.Id, light.Value) 
            dispatch(SetLightValueSuccess(light))
        }
    }
}
```

For a simple working example please checkout the [examples/simple/simple.go](https://github.com/mhannig/alpaca-go/blob/master/examples/simple/simple.go) string reversal service.


