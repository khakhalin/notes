# APIs

Parents: [[system_design]]
See also:

#api #systems


Application Programming Interface.

Types:
* [[restful]] - stateless for the server, built on top of http methods (so standard status codes)
* soap - xml-based, old, secure (financial services like it), verbose. May be both stateful and stateless
* [[websocket]] - stateful, fast (near-realtime), bidirectional
* rpc - for microservices to talk to each other, fast and modern
* [[graphql]] - asking for specific data, developed by Facebook
* webhook - asynchronous
