App Engine application for the Udacity training course.

## Products
- [App Engine][1] : Conference Central App

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool


## Task 1 
1. Session(ndb.Model)  
    name            = ndb.StringProperty(required=True) 
    conferenceName  = ndb.StringProperty()
    speaker         = ndb.StringProperty()
    highlights      = ndb.StringProperty()
    date            = ndb.DateProperty() 
    startTime       = ndb.TimeProperty()
    duration        = ndb.FloatProperty(required=True)
    typeOfsession   = ndb.StringProperty(required=True)
    inWishlist      = ndb.IntegerProperty(required=True) 

  SessionForm(messages.Message):
    """SessionForm -- Session outbound form message"""
    name            = messages.StringField(1)
    conferenceName  = messages.StringField(2)
    speaker         = messages.StringField(3)
    highlights      = messages.StringField(4) 
    date            = messages.StringField(5) #DateTimeField() FORMAT: yyyy-mm-dd 
    startTime       = messages.StringField(6) #DateTimeField() FORMAT: HH:MM
    duration        = messages.FloatField(7) # 1 means 1h
    typeOfsession   = messages.StringField(8)
    websafeKey      = messages.StringField(9)


  To create a session, session name and conference key are required. If the user does not put info for some entries would be set by default values in DEFAULT_SESSION dictionary in conferece.py.

  Session is a child of Conference object. Therefore, when being created, its parent key is given as a conference key.

  When a session is created, the variable inWishlist is set to be 0. As a user add(or remove) a session to(from) his or her wishlist, inWishlist of the session will increase(decreased) by one.

  The variable duration is of FloatProperty. If its value is 1, that means the duration is for an hour. O.5 means 30min.

  The variable startTime is of StringProperty and represents the start time of a session. When created, the input should be of the form HH:MM where HH is in range(0,23), am/pm distinction is NOT supported.
  When converted to Session from SessionFrom, it will be shown as HH:MM, same as the input.
  
  The function _copySessionToForm converts Session to SessionForms(ProtoRPC).


## Task 3 
1. Query for sessions in a conference by popularity. Order the session objects in a 
   conference (given by websafeConferenceKey) by inWishlist. Returns SessionForms  
1. Query for conferences in which a given speaker speaks. To do this, first find the  
   sessions in which the speaker speaks. Then, find the parent keys for the sessions to  find conferences. Returns ConferenceForms.  
1. If we were to query for sessions != workshop AND time < 19h, we encounter a problem   
   because we can't use two inequalities on different properties. Therefore, we should   take, for example, a composite query looks like the following:  

    step 1:  
      CompositeFilter Operator OR  
      TypeOfSession EQAL Workshop   
      startTime EQUAL 19  
      startTime EQUAL 20  
      startTime EQUAL 21  
      startTime EQUAL 22  
      startTime EQUAL 23  
      startTime EQUAL 24  
    step 2: 
      Take NOT EQUAL TO (or Complement set of) the result of step 1.




