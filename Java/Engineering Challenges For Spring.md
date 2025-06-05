# Where to put the WeightLogEntry class

The user's weight log contains objects as the individual entries. Each object has a weight and a date key. This is represented by the WeightLogEntry class. Originally I creates a class and put it in the User (model) class. This didn't seem clean.

One option was to put it in its own file inside the model directory. Or to create a subdirectory called embedded to hold the WeightLogEntry class file. This keeps the model and related class together, making the relationship explicit.

Another option was to keep it out of the model directory and put it in a value-objects directory (vo). This is a domain-driven solution as:
- Entities are unique and distinguished by their identity (i.e. two objects may have the exact same properties but are still individuals). They can also change whilst maintaining their identity (User).
- Value objects don't change and are defined by their attributes. Two value objects containing the same attributes are considered interchangeable (WeightLogObject).

The third solution is the use the @Embeddable annotation and keep it in the model. This makes the relationship clear. This can be spruced up by mixing in the first option and adding an embedded subdirectory in the model directory. This structure scales well if you add further embedded object like FitnessGoal or HealthMetric. 

Embedding wasn't necessary because:
- MongoDB treats lists of objects as subdocuments automatically, so no need for a special "embedded" distinction.

I ended up making putting it in a shared folder because it gets used in the controller.

I would have just kept it in the model directory but the only pitfall is it's not immediately obvious that there's no table in the mongodb database corresponding to WeightLogEntry. 

# Using the `/api` route

This avoids CORS complications and authentication cookies work seamlessly across backend and frontend. You can also serve both from the same sever. CORS aviided because both components originate from the same place. Clear separation of concerns due to the /api route/ . There are also SEO benefits to having your site as a coherent whole.

Best practice is to have api versioning (`api/v1/users`). 
Standardise API responses with fields like success, data, and error.

