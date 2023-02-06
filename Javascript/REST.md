REpresentation
State
Transfer

A server is RESTful if it responds to CRUD requests in a standard way.

A RESTful application treats all server URLs as access points for the various resources on the server.

| HTTP Method | URL used by RESTful app    | REST Action                                        |
| ----------- | -------------------------- | --------------------------------------------- |
| GET         | http://example.com/users   | Gets a list of every item in the resource     |
| POST        | http://example.com/users   | Creates a new user                            |
| GET         | http://example.com/users/1 | Gets the single resource with the given ID    |
| PUT         | http://example.com/users/1 | Updates the single resource with the given ID |
| DELETE      | http://example.com/users/1 |      Deletes the single resource with the given ID                                         |                                               |

