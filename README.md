# Conference Central
Conference Central webapp project to practice Google AppEngine, Python, AngularJS

You can simply access [here](https://conference-central-1226.appspot.com) and test it.


## Requirement
The following needs to be installed first:

- [Python 2.7.x or higher](http://python.org)
- [Google App Engine SDK for Python](https://cloud.google.com/appengine/downloads#Google_App_Engine_SDK_for_Python) with command-line interface tools installed option


## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console](https://console.developers.google.com).
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080](http://localhost:8080).)
1. (Optional) Generate your client library(ies) with [the endpoints tool](https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool).
1. Deploy your application.


## Design Choices - Session and Speaker Implementation
Session functionality is organized by the following classes:

* Session(ndb.Model): NDB kind that would be saved in Google Datastore
    * name = ndb.StringProperty(required=True)
    * highlights = ndb.StringProperty()
    * speaker = ndb.StringProperty()        **speaker is simply implemented as String property.**
    * duration = ndb.IntegerProperty()
    * typeOfSession = ndb.StringProperty(repeated=True)
    * date = ndb.DateProperty()
    * startTime = ndb.TimeProperty()
    * conferenceId = ndb.StringProperty()

* SessionForm(messages.Message): Session outbound form message managed as view
    * name = messages.StringField(1)
    * highlights = messages.StringField(2)
    * speaker = messages.StringField(3)
    * duration = messages.IntegerProperty(4, variant=messages.Variant.INT32)
    * typeOfSession = messages.StringField(5, repeated=True)
    * date = messages.StringField(6)  **would saved as DateProperty on Datastore**
    * startTime = messages.StringField(7)   **would saved as TimeProperty on Datastore**
    * conferenceId = messages.StringField(8)    **parent conference ID**
    * websafeKey = messages.StringField(9)  **websafeKey for URL presentation**
    * conferenceDisplayName = messages.StringField(10)

* SessionForms(messages.Message): Set of session messages for view
    * items = messages.MessageField(SessionForm, 1, repeated=True)

Speaker is simply implemented as String property of Session kind.
