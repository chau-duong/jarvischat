****************************************************************************
SocialMiner - Customer Chat Sample
Cisco Systems, Inc.
http://www.cisco.com/
http://developer.cisco.com/web/socialminer
****************************************************************************

I. Disclaimer
-------------------------------------------------------------------------------

   The customer chat sample is intended to serve as an example of using the 
   SocialMiner Chat API from Javascript.
   
   This sample illustrates the use of the following REST APIs:
     - POST to initiate chat sessions
     - GET to poll for chat events
     - GET to download chat transcripts
     - PUT to send chat messages
     - PUT to leave the chat session

   This sample also includes a node-based HTTP server. This server serves up
   the chat html page and proxies the chat API to work around cross-site
   scripting issues that would otherwise prevent the user from experimenting
   with the chat HTML page.
   
   This is only a sample and is NOT intended to be a production quality
   application and will not be supported as such.  It is NOT guaranteed to
   be bug free. It is merely provided as a guide for a programmer to see
   how to initiate and manage the customer-side of a chat session.

   The sample is divided into two groups of files. The first set of files
   illustrates the use of SocialMiner's REST APIs. The second set of files
   make up the node application that allows the customer to experiment with
   the REST APIs.

   SocialMiner REST API-Related Files:
     customer-chat-sample
       node
         public
           chat.html          - Sample customer chat UI
           javascripts
             customer_chat.js - Javascript wrapper around SocialMiner's chat
                                API.
             utils.js         - Javascript utils to support the sample chat
                                application.

   Support Related Files:

       node
         app.js               - HTTP server
         package.json         - Required packages for the node application
         public
           stylesheets
             style.css        - stylesheets used by the HTTP server
         routes
             index.js         - route to index.html
         views
             error.ejs        - error page template
             index.ejs        - index page template

    This sample is made available to Cisco partners and customers as a
    convenience to help minimize the cost of Cisco SocialMiner customizations.
    Please see the SocialMiner Developer's Guide for further details about the
    chat API.

II. Requirements
-------------------------------------------------------------------------------

   SocialMiner integrated with UCCX. Single-session or multi-session chat is
   configured on both SocialMiner and CCE.

   To actually run the chat html file, node.js and the required libraries
   must be installed.


III. Prerequisites
-------------------------------------------------------------------------------

   - SocialMiner and UCCX installed and single-session or multi-session chat
     feature is configured.

   - The reference URL of the chat feed configured to process chat requests. The
     ID can be obtained using the following SocialMiner REST API:
       http://<socialMiner>/ccp-webapp/ccp/feed

     This REST API will list XML for each feed configured on SocialMiner.
     Feeds with type 8 are chat feeds. The name given when the feed was
     configured is also listed. This is the easiest way to find the XML for
     the chat feed. Here is some sample output from the feed API:

     <feeds>
       <Feed>
         <changeStamp>3</changeStamp>
         <chatInactivityTimeout>300</chatInactivityTimeout>
         <chatJoinTimeout>60</chatJoinTimeout>
         <description>
           Created by CCX application as part of CCX chat configuration.
         </description>
         <name>CCX Chat Feed</name>
         <pollingInterval>0</pollingInterval>
         <pushFeedURL>
           http://socialminer/ccp-webapp/ccp/chatfeed/100053
         </pushFeedURL>
         <refURL>
           http://socialminer/ccp-webapp/ccp/feed/100053
         </refURL>
         <replyTemplateRefURL>
           http://socialminer/ccp-webapp/ccp/template/reply/303
         </replyTemplateRefURL>
         <status>1</status>
         <tags>
           <tag>ccx_chat_req</tag>
         </tags>
         <type>8</type>
       </Feed>
       ...
     </feeds>

     The reference URL of the feed is the value inside the refURL tags. In this
     example, the refURL is http://socialminer/ccp-webapp/ccp/feed/100053

   - Install node.js. It is available from this site:
       http://nodejs.org/


IV. Configuring node.js
-------------------------------------------------------------------------------

   Run these steps to ensure node.js has the necessary dependencies to run the
   HTTP server.

   - Change directory to <zipDirectory>/customer-chat-sample/node.
     <zipDirectory> is the directory in which the sample was extracted.

   - Run the following command to install dependencies:
       npm install
       
   - At this point, node has the dependencies it needs to serve up the
     chat.html page as well as proxy the SocialMiner callback API.


V. Setting up the sample
-------------------------------------------------------------------------------

   Run these steps to configure the sample chat application.

   - Open a browser and enter the following URL. Replace <socialMiner> with the
     hostname or address of your SocialMiner.

       http://<socialMiner>/ccp-webapp/ccp/feed

   - You will be prompted for SocialMiner admin credentials. Enter them. Feed
     XML will list in the browser. Look for a feed named "CCX Chat Feed".

   - Note the value of the refURL field. It will be used in the following steps.

   - Edit the file node/public/chat.html. Find the string "<chatFeedRefUrl>"

   - Replace <chatFeedRefUrl> with the reference URL for your chat feed.

   - Log into the CCX Admin and navigate to the "Chat Web Forms" under Subsystems/Chat and Email/Chat Widget List.

   - Open the code for your chat widget.

   - Find the select tag with name "extensionField_ccxqueuetag". Copy the
     options inside the select tags.

   - Find the string "<!-- options -->' in the file node/public/chat.html.

   - Replace the "<!-- options -->" with the options from your chat widget.

VI. Using the sample
-------------------------------------------------------------------------------

   Run these steps to use the sample and verify your deployment.
 
   - Open a terminal and change directory to
     <zipDirectory>/customer-chat-sample/node.

   - Run the following command:
       node app.js --socialminer <socialMinerHostOrIp>
     <socialMinerHostOrIp> is the hostname or IP address of SocialMiner

   - Open the following URL in Firefox:
       http://localhost:8080/chat.html

   - Enter a Name

   - Click Submit.

   - The UI will transition to a waiting screen and then to a chat screen when
     the agent accepts the chat. When the chat completes, the customer has the
     option to download a transcript if the customer and the agent exchanged
     messages.
