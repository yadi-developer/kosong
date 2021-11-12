## Guide Heyoo Rest Api

Copyright © 2021 Suhandi/Heyoo.company (www.heyoo.id) All rights reserved.

Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:

- Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.
- Redistributions in binary form must reproduce the above copyright notice, this
  list of conditions and the following disclaimer in the documentation and/or
  other materials provided with the distribution.
- Neither the name of “Heyoo Company” nor the names of its contributors may be used to
  endorse or promote products derived from this software without specific prior
  written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS “AS IS” AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

# Preliminary

RESTful API / REST API is an application of the API (Application Programming Interface).
While REST (Representational State Transfer) is an architectural method of communication
that uses the HTTP protocol for data exchange where this method is often applied in
application development.

# REST API Structure

- CORS security ( CORS stands for Cross Origin Resource Sharing, which is a technique of using HTTP requests to allow browsers in one domain to gain access to servers on different resources. )
- Response type (JSON)
- End-to-end encryption
- Request sent type must be JSON or form-data
- JWT token
- Pusher ( Realtime )
- Method API :
  - Authentification [ A method for Register new account, edit {name, email, password} ] (POST, UPDATE)
  - LoggedIn [ A method for login user ] (POST)
  - PostData [ A method for get, post, edit, or delete a post ] (GET, POST, DELETE)
  - Reaction [ A method for sent react like from some post ] (POST)
  - Comment [ A method for get, post comment to some post ] (POST)
  - Search [ A method for seacrhing data {fullname, username, text on some post} ] (GET)
  - Friends [ A method for get or add friends ] (POST, GET)
  - Profile [ A method for get profile data by private or someone ] (GET, UPDATE)
  - Verify [ A method for verification new user ] (POST)

# Authentification

    #1 Register new Account
    - REQ : https://domainbackendAPI.example/Authentification (type = POST)
    - DATA : { fullname, email, password }
    - RESPONSE : JSON
    " If the response status from the backend is true or successful then all data has been prepared
      and a verification email has been sent to the user's mail, message the user to check the email
      and verify immediately because the token will expire in 25 minutes. "

    #2.1 Change Name
    - REQ : https://ex.ex/Authentification/changeName (type = PUT)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : {new_name}
    - RESPONSE : JSON

    #2.2 Change Password
    - REQ : https://ex.ex/Authentification/changePassword (type = PUT)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : {old_password, new_password, confirm_new_password}
    - RESPONSE : JSON

    #2.3 Change Email
    - REQ : https://ex.ex/Authentification/changeName (type = PUT)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : {email (i mean is new email), password}
    - RESPONSE : JSON

"" ATTENTION : When registering, we will send the default url for verification to the registered email,
it's a good idea to change the default url according to the frontend url, and also in the url there
is a verification token which you can later decode by type (URLENCODE and BASE64) , Remember!
that when you send this token to us (Backend) we don't do automatic decoding,
so you have to decode the token first and then send it to us for processing. ""

# LoggedIn

    - REQ : https://ex.ex/LoggedIn (type = POST)
    - DATA : {email, password}
    - RESPONSE : JSON

# PostData

    #1 Get All Friends Post
    - REQ : https://ex.ex/PostData (type = GET)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - RESPONSE : JSON

    #2 Create new post (timeline)
    - REQ : https://ex.ex/PostData
    - HEADER : 'Athorization' => 'Bearer xtoken'
    - DATA : {text, [image]} (type = form-data)
    - RESPONSE : JSON
    " Uploading 2 or more images in one execution will make it easier and can cut the time shorter,
      in the image parameter you need to change the type of the parameter to an array first."

    #3 Edit Post
    // on project

    #4 Delete Some Post
    - REQ : https://ex.ex/PostData/:id (type = DELETE)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - RESPONSE : JSON
    " In deleting a post, of course, not just any access is granted,
      we have filtered arbitrary actions in deleting other people's posts,
      this method only applies to private posts, for routers, input 'id_post' id or post id to be deleted.
      ex : https://ex.ex/PostData/324657xxx "

# Reaction

    - REQ : https://ex.ex/Reaction
    - HEADER : 'Authorization' => 'Bearer xtoke'
    - DATA : {reacti_id}
    - RESPONSE : JSON
    " Seeing the previous error in creating a method to execute the react feature we have made it
      so that there are no more errors such as spam and 'anonymous user reaction' we have solved that problem,
      to use this method you just send us your react_id and we will manage This action is the best "

# Comment

    #1 Get all coments on a Post
    - REQ : https://ex.ex/Comment/:id (type = GET)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - RESPONSE : JSON
    " In this method you need to send a data from the post id which you want to retrieve all the comment data in it,
      instead of sending the id using form-data it is better to send it via router or segment in url.
      ex : urls/method/37736xxx "

    #2 Sent Comment for a Post
    - REQ : https://ex.ex/Comment (type = POST)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : {text, pid}
    - RESPONSE : JSON

# Search

    - REQ : https://ex.ex/Search/:keyword (type = GET)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - RESPONSE : JSON
    " Learning from the shortcomings of the past, we slightly added capabilities to this method
      to make it more interesting and better in carrying out its duties as a search engine, you only need to send keywords (any)
      through a router or url segment, this engine can search for data based on the level of match between keywords and data in the database,
      it produces results with 3 types, namely account, username/nickname data and post data according to the percentage of similarity between keywords and the text in the post.
      to find nickname, you must accompany the tag or sign (@) before the keyword.
      ex : @heyoo21 "

# Friends

    #1 Get My Friend List
    - REQ : https://ex.ex/Friends/me (type = GET)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - RESPONSE : JSON

    #2 Get friend request that is on my account
    - REQ : https://ex.ex/Friends/request (type = GET)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - RESPONSE : JSON

    #3 Add a someone to be friend
    - REQ : https://ex.ex/Friends (type = POST)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : {id_user}
    - RESPONSE : JSON

    #4 Confirm request friends
    - REQ : https://ex.ex/Friends/confirm/:id_user (type = PUT)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : {id_user}
    - RESPONSE : JSON

    #4 Refusing request friends
    - REQ : https://ex.ex/Friends/refuso/:id_user (type = PUT)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : {id_user}
    - RESPONSE : JSON

"" what is meant by 'id_user' is that the id is not our id as the recipient but the id of someone who is asking for friendship. ""

# Verify

    - REQ : https://ex.ex/Verify (type = POST)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - DATA : NULL
    - RESPONSE : JSON
    " To use the verification method, previously on new account registration you have been asked to verify,
      you can send us a JWT token which is obtained from the email url there we send jwt tokens wrapped in URLENCODE and BASE64,
      you have to decode the token and send it to us via this method and via the data authorization header.
      ATTENTION: TOKEN MUST BE DECODE AND SENT THROUGH HEADER "

# Profile

    #1 Get USer Profile Data (type Of data : include me or someone )
    - REQ : https://ex.ex/PostData/[typeOfData]/:id (type = GET)
    - HEADER : 'Authorization' => 'Bearer xtoken'
    - RESPONSE : JSON
    " Instead of sending a rule by entering the rule in the form-data,
      we better use a router to determine which rule we will use, in this method there are 2 rules,
      namely (me and someone) me which states that data is privately owned while someone is a rule for retrieve data belonging to someone's profile.
      make sure to put this rule before putting the id, for the private rule it is specifically so as not to include the id. "
