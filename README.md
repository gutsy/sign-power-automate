# sign-power-automate
A collection of Power Automate flows for Adobe Acrobat Sign

To install:
- Go to My Flows in Power Automate
- Click Import
- Select Import Package (Legacy)

During installation you will need to choose or create your connections to Sign, Sharepoint, etc. 

Flows included:

- Save Completed Agreement to Sharepoint
  - Triggers when an agreement finished
  - Pulls PDF from Sign
  - Saves to specified folder in Sharepoint. Saved file name can be defined here if desired, or you can use the name that comes along with the PDF. 
  - *note* Make sure in the File Name section of the Create File step to include the .pdf extension. It does not add that extension automatically. 
- Send New Agreement When A File Is Uploaded to Sharepoint
  - Triggers when something is uploaded to a specified Sharepoint folder
  - Pulls the file from Sharepoint and uploads to Adobe Sign 
  - Uses the transient document ID to create and send an agreement. 
- Send Agreement via HTTP Call
  - Triggered when a user sends a JSON payload to the URL provided by the trigger step
  - This step uses a JSON schema to know what content is coming in. You provide a sample payload that you will be sending and it will generate the schema so you can access the content sent. This schema is very simple and only contains the email address to send to. The payload can contain anything you'd like. 
  - Sends agreement to the provided email address, using a predetermined library document
- Send Custom Email (replaces standard Sign email)
  - Triggers whenever a new agreement is started
  - Calls to get the signing url
  - The signing URL response is a JSON array that contains other arrays. This loops through and will send an email to each signer's URL. Will only be one in cases of sequential sends, will be multiple with concurrent sends.
  - *note* A support ticket will need to be put in to request to turn off the Please Sign email for the group this applies to, otherwise the signer will get both the standard Sign email and this new custom one. 
- Save Completed Agreement to Sharepoint, Docs Seperated
  - Triggers when an agreement is completed
  - Pulls the list of document IDs for the agreement
  - loops through them and saves each one to a sharepoint folder
  
## Trigger Conditions
All of the above flows (except for the HTTP Call) can have trigger conditions applied to further restrict when a flow will run. This is useful if you do not want to have multiple groups in Sign solely for triggers to use. To add, click the three dots on the top right corner of the trigger step, select 'settings' and scroll to the trigger conditions section and add. Here are a few examples:

- @contains(triggerBody()?['agreement/name'],'search term') - Flow will only run if the completed agreement has 'search term' in its name.
- @or(contains(triggerBody()?['agreement/name'],'one'),contains(triggerBody()?['agreement/name'],'two')) - Flow will only run if one of two terms is in the name. You can also use @and. 
- @equals(triggerBody()?['eventResourceParentType'],'MEGASIGN') - Flow will only run if the agreement is part of a bulk send. 



